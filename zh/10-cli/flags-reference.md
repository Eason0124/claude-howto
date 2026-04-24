# Claude Code CLI Flag 完整對照表

> 本表依官方 [CLI Reference](https://code.claude.com/docs/en/cli-reference) 整理。若本文與官方衝突請以官方為準。
> 本檔案是 [README.md](README.md) 的權威補充 — README 有幾個歷史上列出但**官方實際沒有**的 flag,請以本檔為準。

---

## 主命令形式

```bash
claude                           # 進入互動 REPL
claude "query"                   # 帶初始 prompt 進入 REPL
claude -p "query"                # 列印模式,單次查詢後退出
cat file | claude -p "query"     # 管道輸入
claude -c                        # 繼續最近會話
claude -r <id-or-name> "query"   # 按 ID 或名稱恢復
```

---

## 會話管理

| Flag | 說明 |
|------|------|
| `--continue`, `-c` | 載入最近一次會話 |
| `--resume`, `-r` | 按 ID 或名稱恢復指定會話 |
| `--session-id <uuid>` | 使用特定會話 ID(必須為合法 UUID) |
| `--fork-session` | 恢復時建立新 session ID,而非重用原 ID |
| `--from-pr <pr>` | 恢復與 GitHub PR 關聯的會話(PR 號或 URL) |
| `--name <name>`, `-n` | 設定會話顯示名稱(顯示在 `/resume` 與終端標題) |

> ⚠️ 舊教材常見的 `--session`、`--new-session`、`--resume-last` **不在官方清單中**;用 `--name` 與 `--continue` / `--resume` 即可。

---

## 執行模式

| Flag | 說明 |
|------|------|
| `--print`, `-p` | 列印模式(查詢後退出,不進互動) |
| `--remote "task"` | 在 claude.ai 建立 web session |
| `--teleport` | 把 web session 恢復到本地終端 |
| `--remote-control`, `--rc [name]` | 啟用 Remote Control,從 claude.ai 或 app 控制本機 |
| `--bare` | 極簡模式:跳過 hooks / skills / plugins / MCP / auto memory / CLAUDE.md,只保留 Bash / Read / Edit。等同環境變數 `CLAUDE_CODE_SIMPLE=1` |
| `--init` | 執行 initialization hooks 並啟動互動 |
| `--init-only` | 執行 initialization hooks 並退出(不啟動互動) |
| `--maintenance` | 執行 maintenance hooks 並啟動互動 |

---

## 工作環境

| Flag | 說明 |
|------|------|
| `--worktree [name]`, `-w` | 在隔離的 git worktree(`.claude/worktrees/<name>`)啟動,無名稱時自動生成 |
| `--add-dir <path...>` | 添加額外工作目錄供讀寫(可多個) |
| `--setting-sources <list>` | 逗號分隔的設定來源:`user`、`project`、`local` |
| `--settings <path-or-json>` | 載入額外 settings(JSON 檔或字串) |
| `--tmux[=classic]` | 為 worktree 建立 tmux session(需 `--worktree`),iTerm2 預設用原生 panes,`=classic` 改為傳統 tmux |

> ⚠️ 舊教材常見的 `--working-directory` **官方沒有**;用 `--add-dir` 加目錄,`cd` 後再啟動 `claude` 即可設定主目錄。

---

## 模型與推理

| Flag | 說明 |
|------|------|
| `--model <name>` | 設定模型(`sonnet` / `opus` / `haiku` 或完整 ID,也支援 alias 如 `opusplan`) |
| `--fallback-model <name>` | 預設模型超載時切換(**僅列印模式**) |
| `--effort <level>` | 推理強度:`low` / `medium` / `high` / `xhigh` / `max`(可用等級依模型) |
| `--betas <headers>` | API 請求 Beta headers(僅 API key 使用者) |

---

## 系統提示詞(四 flag 相容矩陣)

| Flag | 互動模式 | 列印模式 | 備註 |
|------|---------|---------|------|
| `--system-prompt` | ✅ | ✅ | 替換預設 prompt。與 `--system-prompt-file` 互斥 |
| `--system-prompt-file <path>` | ❌ | ✅ | 列印模式專用 |
| `--append-system-prompt` | ✅ | ✅ | 追加,保留預設能力 |
| `--append-system-prompt-file <path>` | ✅ | ✅ | 追加,保留預設能力 |
| `--exclude-dynamic-system-prompt-sections` | (不建議) | ✅ | 把動態片段(cwd / env / memory path / git status)移到首則 user message,改善 prompt cache 重用。僅對預設 prompt 生效,被 `--system-prompt*` 覆蓋 |

**實務建議**:除非要完全自訂人格,否則優先用 append flags 保留內建能力。

---

## 工具與許可權

| Flag | 說明 |
|------|------|
| `--tools <list>` | 限制可用內建工具。`""`=全禁用;`"default"`=全啟用;或 `"Bash,Edit,Read"` |
| `--allowedTools <list>` | 不需提示即可用的工具(支援 permission rule 語法) |
| `--disallowedTools <list>` | 從 context 完全移除的工具 |
| `--permission-mode <mode>` | `default` / `acceptEdits` / `plan` / `auto` / `dontAsk` / `bypassPermissions` |
| `--dangerously-skip-permissions` | 等同 `--permission-mode bypassPermissions` |
| `--allow-dangerously-skip-permissions` | 把 `bypassPermissions` 加入 `Shift+Tab` 模式輪迴,但不以此啟動 |
| `--permission-prompt-tool <mcp-tool>` | 在非互動模式用 MCP tool 處理權限提示 |

> ⚠️ `--enable-auto-mode` 已在 **v2.1.111 移除**;用 `--permission-mode auto` 取代。

---

## Agent 與 Subagent

| Flag | 說明 |
|------|------|
| `--agent <name>` | 為本會話指定 agent(覆蓋 `agent` 設定) |
| `--agents <json-or-path>` | 用 JSON 動態定義 subagents(欄位同 frontmatter,加 `prompt`) |

---

## MCP 與外掛

| Flag | 說明 |
|------|------|
| `--mcp-config <path-or-json>` | 載入 MCP 設定 |
| `--strict-mcp-config` | 只用 `--mcp-config`,忽略其他來源 |
| `--channels <list>` | (研究預覽) MCP channel 通知,格式 `plugin:<name>@<marketplace>`,需 Claude.ai 認證 |
| `--dangerously-load-development-channels <list>` | 啟用非 allow-list 的 channels(本地開發用) |
| `--plugin-dir <path>` | 本會話載入 plugin 目錄(可重複) |
| `--disable-slash-commands` | 禁用本會話所有 skills 與 slash commands |

> ⚠️ 舊教材的 `--mcp`、`--mcp-list` **官方沒有**;改用 `claude mcp list` / `claude mcp add`。

---

## IDE 與瀏覽器

| Flag | 說明 |
|------|------|
| `--ide` | 若恰好有一個可用 IDE,啟動時自動連接 |
| `--chrome` / `--no-chrome` | 啟用 / 禁用 Chrome 瀏覽器整合 |

---

## 輸出與輸入格式

| Flag | 僅列印模式? | 說明 |
|------|-----------|------|
| `--output-format <fmt>` | ✅ | `text` / `json` / `stream-json` |
| `--input-format <fmt>` | ✅ | `text` / `stream-json` |
| `--include-partial-messages` | ✅ | 包含 streaming events,需 `--output-format stream-json` |
| `--include-hook-events` | ✅ | 包含所有 hook 生命週期事件,需 `--output-format stream-json` |
| `--replay-user-messages` | ✅ | 把 stdin 的 user messages 原樣 echo 到 stdout,需 `--input-format stream-json` + `--output-format stream-json` |
| `--json-schema <schema>` | ✅ | 取得符合 JSON Schema 的驗證輸出 |
| `--verbose` | 兩者 | 詳細 log,顯示完整逐輪輸出 |

---

## 列印模式專用上限

| Flag | 說明 |
|------|------|
| `--max-turns <n>` | 限制 agentic turns 數量,達限以錯誤退出(預設無限) |
| `--max-budget-usd <amount>` | API 花費上限(十進位,如 `5.00`) |
| `--no-session-persistence` | 不寫入磁碟,無法 resume |

---

## 除錯

| Flag | 說明 |
|------|------|
| `--debug [categories]` | 啟用除錯,可用 `"api,hooks"` 或排除式 `"!statsig,!file"` |
| `--debug-file <path>` | 寫 debug log 到檔案,隱含啟用 `--debug`;優先級高於環境變數 `CLAUDE_CODE_DEBUG_LOGS_DIR` |

---

## 團隊模式

| Flag | 說明 |
|------|------|
| `--teammate-mode <mode>` | `auto`(預設) / `in-process` / `tmux`,agent team 隊友顯示方式 |
| `--remote-control-session-name-prefix <prefix>` | 自訂 Remote Control 自動生成會話名稱的前綴(預設為機器主機名)。也可用環境變數 `CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX` |

---

## 通用

| Flag | 說明 |
|------|------|
| `--version`, `-v` | 輸出版本號 |
| `--help` | 顯示幫助(注意:`--help` 不列出所有 flag,某 flag 未列不表示不可用) |

---

## 子命令速查

```bash
# 更新
claude update

# 列出已設定的 subagents
claude agents

# 看 auto mode 內建分類規則
claude auto-mode defaults
claude auto-mode config

# 產 CI / script 用的長期 OAuth token(不存本機)
claude setup-token

# 啟動 Remote Control server mode
claude remote-control

# 認證
claude auth login [--email <email>] [--sso] [--console]
claude auth logout
claude auth status [--text]

# MCP(見 ../05-mcp/)
claude mcp add / add-json / list / get / remove / serve / reset-project-choices
  / add-from-claude-desktop

# Plugin(詳見 ../07-plugins/)
claude plugin install <name>@<marketplace>
claude plugin ...
```

---

## 環境變數清單

| 變數 | 用途 |
|------|------|
| `ANTHROPIC_API_KEY` | API 金鑰 |
| `CLAUDE_CODE_SIMPLE` | `--bare` 設為 `1` 時的內部旗標 |
| `CLAUDE_CODE_DEBUG_LOGS_DIR` | 除錯 log 目錄(被 `--debug-file` 覆蓋) |
| `CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX` | Remote Control 名稱前綴 |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | 設 `1` 禁用背景任務 |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | 控制提示建議生成 |
| `CLAUDE_CODE_TASK_LIST_ID` | `~/.claude/tasks/` 內跨會話共享任務清單的命名目錄 |
| `MCP_TIMEOUT` | MCP server 啟動逾時 |
| `MAX_MCP_OUTPUT_TOKENS` | 單次 MCP 工具輸出 token 上限 |
| `ENABLE_TOOL_SEARCH` | MCP Tool Search 模式控制 |

---

## 已移除 / 已棄用

| 項目 | 狀態 | 替代方式 |
|------|------|---------|
| `--enable-auto-mode` | 已在 v2.1.111 移除 | `--permission-mode auto` |

---

## 實驗性 / 研究預覽

- `--channels`
- `--dangerously-load-development-channels`

---

## 相關文件

- [CLI 總覽](README.md)
- [Print 模式深度用法](print-mode.md)
- [官方 CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [官方 Interactive Mode](https://code.claude.com/docs/en/interactive-mode)
- [官方 Settings](https://code.claude.com/docs/en/settings)
