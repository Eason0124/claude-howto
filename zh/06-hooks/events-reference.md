# Hooks 事件完整參考

> 本頁依官方 [Hooks Reference](https://code.claude.com/docs/en/hooks) 與 [Hooks Guide](https://code.claude.com/docs/en/hooks-guide) 整理。若本文與官方衝突請以官方為準。

## 事件總表

Claude Code 共 **27 個 hook 事件**,依用途分類如下。

### Tool 事件（5 個）

| 事件 | 觸發時機 | 可否阻擋 | Matcher 對象 |
|------|---------|---------|-------------|
| `PreToolUse` | 工具呼叫執行前 | ✅ 可擋下 | 工具名稱（如 `Write`、`Bash`） |
| `PermissionRequest` | 彈出權限對話框時 | ✅ 可自動決策 | 工具名稱 |
| `PermissionDenied` | Auto mode 分類器拒絕工具呼叫後 | ⚠️ 可請求 retry | 工具名稱 |
| `PostToolUse` | 工具呼叫成功完成後 | ✅ 可阻止結果回傳 | 工具名稱 |
| `PostToolUseFailure` | 工具呼叫失敗後 | ✅ 可攔截錯誤 | 工具名稱 |

### Session 事件（6 個）

| 事件 | 觸發時機 | Matcher 對象 |
|------|---------|-------------|
| `SessionStart` | Session 開始或恢復 | `startup`、`resume`、`clear`、`compact` |
| `SessionEnd` | Session 結束 | 無 |
| `Stop` | Claude 回應結束 | 無 |
| `StopFailure` | 回應因 API 錯誤中斷 | 無 |
| `SubagentStart` | Subagent 被產生時 | subagent 類型名 |
| `SubagentStop` | Subagent 完成時 | subagent 類型名 |

### Prompt / Task 事件（5 個）

| 事件 | 觸發時機 | 可否阻擋 |
|------|---------|---------|
| `UserPromptSubmit` | 使用者送出 prompt,Claude 處理前 | ✅ 可擋下並附加 context |
| `UserPromptExpansion` | 使用者輸入的斜線命令展開為 prompt 時 | ✅ 可擋下 |
| `TaskCreated` | 透過 `TaskCreate` 建立任務時 | ✅ 可擋下 |
| `TaskCompleted` | 任務標記完成時 | ✅ 可擋下 |
| `TeammateIdle` | 多 agent 團隊中某位即將閒置 | ✅ 可指派任務 |

### Lifecycle 事件（11 個）

| 事件 | 觸發時機 |
|------|---------|
| `ConfigChange` | 設定檔於 session 中被變更 |
| `CwdChanged` | 工作目錄變更 |
| `FileChanged` | 被監控的檔案於磁碟上變更 |
| `PreCompact` | Context 壓縮前 |
| `PostCompact` | Context 壓縮完成後 |
| `WorktreeCreate` | Worktree 被建立時 |
| `WorktreeRemove` | Worktree 被移除時 |
| `Notification` | Claude Code 送出通知時 |
| `InstructionsLoaded` | `CLAUDE.md` 或 `.claude/rules/*.md` 被載入時 |
| `Elicitation` | MCP server 請求使用者輸入 |
| `ElicitationResult` | 使用者回應 MCP elicitation 後 |

---

## 4 種 Hook 型別

`type` 欄位決定 hook 執行方式:

### 1. `command`（最常用）

執行 shell script。

- **stdin**：事件資料的 JSON
- **stdout**：JSON 或純文字(依事件決定)
- **stderr**：錯誤訊息 / 阻擋理由
- **Exit code**:
  - `0` → 允許;對某些事件(如 `UserPromptSubmit`),stdout 會被加入 Claude context
  - `2` → 阻擋動作;stderr 內容回傳給 Claude
  - 其他 → 非阻擋錯誤,通知但繼續執行

可選欄位:
- `async: true` — 背景執行,不阻擋主流程
- `asyncRewake: true` — 背景執行,但 exit code 2 時喚醒主流程
- `shell: "bash" | "powershell"` — 指定 shell(依平台)

### 2. `http`

POST 事件 payload 到外部 endpoint。

```json
{
  "type": "http",
  "url": "https://my-hook.example.com/claude",
  "headers": { "Authorization": "Bearer $MY_TOKEN" },
  "allowedEnvVars": ["MY_TOKEN"]
}
```

URL 與 headers 中的 `$VAR` 會被展開。

### 3. `prompt`（單輪 LLM 判斷）

讓小模型做一次判斷,回傳 `{"ok": true|false, "reason": "..."}`。

```json
{
  "type": "prompt",
  "model": "claude-haiku-4-5",
  "prompt": "Is this command safe to run? $TOOL_INPUT"
}
```

### 4. `agent`（實驗性,多輪 subagent)

用完整 subagent 驗證。預設 timeout 60 秒、上限 50 turns。

---

## 通用 Config 欄位

這些欄位所有 hook 型別共用:

| 欄位 | 說明 |
|------|------|
| `matcher` | 過濾條件(見下方「Matcher 語法」) |
| `if` | 權限規則,細粒度工具篩選(需較新版本,使用前請驗證支援版本) |
| `timeout` | 秒數。預設 `command=600`、`prompt=30`、`agent=60` |
| `statusMessage` | 執行中顯示的訊息 |
| `once` | 每個 session 只跑一次(主要用於 skill / agent frontmatter) |

---

## Matcher 語法

- 無 matcher 或 `""` 或 `"*"` → 全部符合
- 純英文字母/數字/`_`/`|` → 字面比對或 pipe 分隔清單(區分大小寫),如 `"Write|Edit"`
- 其他字元 → 視為 JavaScript 正則,如 `"mcp__github__.*"`

不同事件的 matcher 對象不同:

- **Tool 事件**:工具名稱(`Write`, `Bash`, `mcp__<server>__<tool>`)
- **SessionStart**:啟動方式(`startup`, `resume`, `clear`, `compact`)
- **Notification**:通知類型(`permission_prompt`, `idle_prompt`, `auth_success`, `elicitation_dialog`)
- **ConfigChange**:設定源(`user_settings`, `project_settings`, `local_settings`, `policy_settings`, `skills`)
- **FileChanged**:字面檔名(此事件不支援正則,特殊規則)
- **其他事件**:不支援 matcher

---

## 設定檔位置與優先順序

由高到低:

1. **Managed policy**:組織級,集中部署
2. **`.claude/settings.local.json`**:專案,`.gitignore`
3. **`.claude/settings.json`**:專案,通常提交到 git
4. **`~/.claude/settings.json`**:使用者全域
5. **Plugin `hooks/hooks.json`**:外掛提供
6. **Skill / Agent frontmatter**:`hooks:` 段落

---

## 標準設定格式

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh",
            "timeout": 30
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.sh"
          }
        ]
      }
    ]
  }
}
```

> **與舊版差異**:早期教材常見 `"hooks": ["path/to/script.sh"]`(字串陣列)的寫法已過時,新版必須用 `{ "type": "command", "command": "..." }` 物件包起來。

---

## 環境變數

Hook 執行時可用的環境變數:

| 變數 | 用途 |
|------|------|
| `CLAUDE_PROJECT_DIR` | 目前專案根目錄 |
| `CLAUDE_ENV_FILE` | SessionStart / CwdChanged / FileChanged 時,持久化環境變數用的檔案路徑 |
| `CLAUDE_PLUGIN_ROOT` | Plugin 安裝目錄(plugin hook 內可用) |
| `CLAUDE_PLUGIN_DATA` | Plugin 持久化資料目錄 |

---

## Exit Code 與 JSON 決策格式

### 最簡單:exit code

```bash
#!/usr/bin/env bash
if [[ "$TOOL_INPUT" == *"rm -rf /"* ]]; then
  echo "禁止刪除根目錄" >&2
  exit 2   # 阻擋
fi
exit 0     # 允許
```

### 進階:JSON 決策

從 stdout 輸出 JSON,可做更精細的控制。**每個事件的 JSON schema 不同**,最常用的有:

**PreToolUse — 允許 / 拒絕 / 詢問**:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "已通過 lint",
    "updatedInput": { "...": "可選,修改 tool input" },
    "additionalContext": "可選,附加 context 給 Claude"
  }
}
```

`permissionDecision` 可選值:`allow` / `deny` / `ask` / `defer`。

**UserPromptSubmit — 擋下使用者 prompt**:

```json
{
  "decision": "block",
  "reason": "此 prompt 含機敏關鍵字",
  "hookSpecificOutput": {
    "hookEventName": "UserPromptSubmit",
    "additionalContext": "可選",
    "sessionTitle": "可選"
  }
}
```

**通用欄位**(所有事件可用):

```json
{
  "continue": true,
  "stopReason": "若 continue=false 顯示此訊息",
  "suppressOutput": false,
  "systemMessage": "給使用者看的警示"
}
```

詳細每事件 schema 請參考 [官方 Hooks Reference](https://code.claude.com/docs/en/hooks)。

---

## 事件特定 Input Payload(節選)

Hook 的 stdin 會收到該事件的 JSON。常見欄位:

- **SessionStart**:`{source, model}`
- **UserPromptSubmit**:`{permission_mode, prompt}`
- **PreToolUse**:`{tool_name, tool_input, tool_use_id}`
- **PostToolUse**:`{tool_name, tool_input, tool_response, tool_use_id}`
- **PostToolUseFailure**:`{tool_name, tool_input, tool_use_id, error, is_interrupt}`
- **PermissionDenied**:`{tool_name, tool_input, tool_use_id, reason}`
- **ConfigChange**:`{config_source}`
- **InstructionsLoaded**:`{file_path, memory_type, load_reason, ...}`

---

## 管理 Hooks

- `/hooks` 斜線命令:檢視目前啟用的 hooks
- `disableAllHooks: true`(settings.json):整批停用
- Plugin hooks 受 plugin 啟用狀態控制

---

## 使用前自我檢查

1. 目標事件是否真的存在?本表以官方為準,版本差異可能導致部分事件尚未推出。
2. 你的 `settings.json` 是否用了新格式(`type: "command"` 物件)?
3. 寫入路徑前有沒有先測試 script 能獨立執行?
4. 若 hook 很敏感(擋 `rm`、送 Slack 通知等),建議先放在 `.claude/settings.local.json` 試跑,成熟後再 promote 到 `.claude/settings.json`。

---

## 相關文件

- [Hooks 總覽](README.md)
- [官方 Hooks Reference](https://code.claude.com/docs/en/hooks)
- [官方 Hooks Guide（教學）](https://code.claude.com/docs/en/hooks-guide)
- [Settings 完整參考](https://code.claude.com/docs/en/settings)
