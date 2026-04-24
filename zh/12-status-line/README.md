<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Status Line

Status Line 是 Claude Code 底部的狀態列,你可以寫 shell 腳本往上面顯示 context 用量、成本、git 狀態、當前模型、自訂指標等。

## 基本設定

在 `settings.json` 放一支腳本路徑:

```json
{
  "statusLine": "$CLAUDE_PROJECT_DIR/.claude/status-line.sh"
}
```

設定檔位置(擇一):

- `~/.claude/settings.json` — 個人全局
- `.claude/settings.json` — 專案共享
- `.claude/settings.local.json` — 專案私密

## 腳本 IO 協議

- **stdin**:JSON,Claude Code 給當前會話的資訊
- **stdout**:要顯示的文字(**每行變一行狀態**,支援多行)
- **stderr**:錯誤訊息(不顯示在狀態列)
- **Exit code**:非 0 時不顯示

## stdin 內容

常見欄位:

- `session_id` — 當前會話 ID
- `cwd` — 目前工作目錄
- `model` — 使用的模型
- `transcript_path` — 對話記錄路徑
- context / tokens / cost(視版本)

用 `jq` 抽取即可:

```bash
INPUT=$(cat)
MODEL=$(echo "$INPUT" | jq -r '.model')
```

## 範例 1:最小版

```bash
#!/usr/bin/env bash
INPUT=$(cat)
MODEL=$(echo "$INPUT" | jq -r '.model.display_name // .model // "?"')
BRANCH=$(git symbolic-ref --short HEAD 2>/dev/null || echo "no-git")

echo "🔷 $MODEL | 🌿 $BRANCH"
```

## 範例 2:遊戲後端工程師常用(多行)

```bash
#!/usr/bin/env bash
INPUT=$(cat)

MODEL=$(echo "$INPUT" | jq -r '.model.display_name // "?"')
CWD=$(echo "$INPUT" | jq -r '.cwd')
BRANCH=$(git -C "$CWD" symbolic-ref --short HEAD 2>/dev/null || echo "-")
DIRTY=$(git -C "$CWD" status --porcelain 2>/dev/null | wc -l | xargs)

# 第 1 行
echo "🤖 $MODEL  📁 $(basename "$CWD")  🌿 $BRANCH  ✏️  $DIRTY changed"

# 第 2 行 — 提醒這是 dev 還是其他環境
case "$CWD" in
  *prod*) echo "🚨 PROD repo — 不要隨便改" ;;
  *staging*) echo "⚠️  Staging repo" ;;
  *) echo "💻 Dev" ;;
esac
```

## 範例 3:接 cost / context 資訊

```bash
#!/usr/bin/env bash
INPUT=$(cat)

TOKENS=$(echo "$INPUT" | jq -r '.context.total_tokens // 0')
COST=$(echo "$INPUT" | jq -r '.cost // 0')

# 簡單的 bar(10 格)
PCT=$(echo "$TOKENS * 10 / 200000" | bc)
BAR=""
for i in $(seq 1 10); do [ "$i" -le "$PCT" ] && BAR+="█" || BAR+="░"; done

printf "📊 %s  %s tokens  \$%.4f\n" "$BAR" "$TOKENS" "$COST"
```

## 效能建議

- 狀態列**每次螢幕重繪**都會跑腳本,保持快(< 50ms)
- 避免網路呼叫(除非 cache)
- 避免 `find`、`grep -r` 等重動作
- git 狀態用 `git status --porcelain | wc -l`,比全狀態快

## 除錯

```bash
# 手動餵假 JSON 試腳本
echo '{"model":{"display_name":"Opus"},"cwd":"/path"}' | \
  bash ~/.claude/status-line.sh
```

## 與 Hook 的差異

| 功能 | Status Line | Hooks |
|------|-------------|-------|
| 觸發時機 | 每次重繪 | 事件發生 |
| 可阻擋動作 | ❌ | ✅ |
| 典型用途 | 顯示狀態 | 格式化 / 擋操作 / 通知 |

## 相關概念

- [Output Styles](../11-output-styles/README.md)
- [Hooks](../06-hooks/README.md)
- [Settings 檔案](../02-memory/README.md)

## 更多資源

- [官方 Status Line 文件](https://code.claude.com/docs/en/statusline)
