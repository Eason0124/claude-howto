# Print 模式(`-p` / `--print`)深度用法

Print 模式讓 Claude Code 變成一次性 CLI 工具 — 送一個 prompt、拿到結果、退出。適合寫 shell script、CI 流程、批次處理。

## 基本形式

```bash
# 單次查詢
claude -p "解釋這個錯誤"

# 管道輸入
cat error.log | claude -p "找出 root cause"

# 混合 — stdin 是上下文,query 是問題
cat src/game/matchmaker.ts | claude -p "列出所有 race condition"

# 延續上次對話
claude -c -p "補一個 edge case 測試"
```

## 輸出格式

```bash
# 純文字(預設)
claude -p "query"

# JSON — 適合程式消費
claude -p --output-format json "query"

# Streaming JSON — 適合 UI / log 即時顯示
claude -p --output-format stream-json "query"
```

### Streaming JSON 的事件

搭配 `--output-format stream-json` 可加:

- `--include-partial-messages` — 含 streaming 事件
- `--include-hook-events` — 含所有 hook lifecycle
- `--replay-user-messages` — 把 stdin 的 user message 原樣 echo 到 stdout(要 `--input-format stream-json` + `--output-format stream-json`)

## Schema 驗證輸出

想要結構化結果:

```bash
claude -p --json-schema '{"type":"object","properties":{"bugs":{"type":"array","items":{"type":"string"}}}}' \
  "找出這段程式的 bug,以 JSON 回傳" < src/matchmaker.ts
```

## 花費與輪數上限(僅 print mode)

避免 CI 跑飛:

```bash
claude -p --max-budget-usd 5.00 --max-turns 10 "重構這支檔案"
```

達到上限會以錯誤退出。

## 不持久化

```bash
claude -p --no-session-persistence "臨時問一下"
# session 不寫入磁碟,不能 resume
```

適合敏感 prompt、一次性任務。

## 系統提示詞覆寫

Print 模式特有:

```bash
# 從檔案載入整段系統 prompt(僅 print 模式)
claude -p --system-prompt-file ./prompts/reviewer.txt "review"

# 改善 prompt cache 命中率
claude -p --exclude-dynamic-system-prompt-sections "query"
```

## Fallback model

高峰期避免失敗:

```bash
claude -p --model opus --fallback-model sonnet "重構核心模組"
```

## 非互動權限處理

```bash
# 完全不問 — 常用於 CI
claude -p --dangerously-skip-permissions "跑完整 refactor"

# 精準控制
claude -p --allowedTools "Read,Grep" --disallowedTools "Bash" "掃全專案"

# 用 MCP tool 接管權限決策
claude -p --permission-prompt-tool mcp__auth__gate "query"
```

## 與 stdin stream-json 聯動

CI 想把多輪對話串起來(少見):

```bash
cat session-events.jsonl | \
  claude -p \
    --input-format stream-json \
    --output-format stream-json \
    --replay-user-messages \
    "續這個對話"
```

## 常見 CI / script 用法

### CI:PR 自動 review

```bash
PR_DIFF=$(gh pr diff $PR_NUMBER)

echo "$PR_DIFF" | claude -p \
  --output-format json \
  --max-budget-usd 2.00 \
  --max-turns 5 \
  "審查此 diff,列出潛在問題,回 JSON: {issues: [{file, line, concern}]}"
```

### 本地:一鍵 refactor

```bash
claude -p \
  --model opus \
  --allowedTools "Read,Edit,Grep" \
  --max-turns 20 \
  "把 src/game/ 下所有 any 型別改成精確型別"
```

### Log 分析

```bash
tail -1000 app.log | claude -p \
  --output-format json \
  "彙整錯誤分布,回 JSON: {errors: [{pattern, count, example}]}"
```

## 長期 token(CI 專用)

用 API key 或長期 OAuth token 登入:

```bash
# 生一個不存本機、可複製到 CI secrets 的 OAuth token
claude setup-token
```

把 token 貼到 GitHub Actions 的 `ANTHROPIC_API_KEY` secret。

## 與互動模式差異

| 面向 | 互動模式 | Print 模式 |
|------|---------|-----------|
| 多輪 | ✅ | ❌(除非 `-c`) |
| Tab 補全 | ✅ | ❌ |
| 快捷鍵 | ✅ | ❌ |
| 可管道 | ❌ | ✅ |
| 可指令碼化 | ❌ | ✅ |
| `--max-turns` | ❌ | ✅ |
| `--max-budget-usd` | ❌ | ✅ |
| `--fallback-model` | ❌ | ✅ |
| `--json-schema` | ❌ | ✅ |
| `--no-session-persistence` | ❌ | ✅ |

## 相關文件

- [CLI 總覽](README.md)
- [完整 flag 對照表](flags-reference.md)
- [官方 CLI Reference](https://code.claude.com/docs/en/cli-reference)
