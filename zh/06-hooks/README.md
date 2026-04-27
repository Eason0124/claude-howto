<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Hooks 參考

Hooks 是在 Claude Code 事件發生時自動執行的處理器,用來做格式化、校驗、通知、審計等自動化工作。

> **完整事件表、Matcher 語法、Exit code 與 JSON 決策格式** 請見 [events-reference.md](events-reference.md)。

## 概覽

Hooks 是事件驅動的自動化機制。它們會在 Claude Code 發生某些動作時自動執行,不需要你手動觸發。

常見用途:

- 寫檔案前自動格式化
- 提交前執行測試
- 掃描安全問題
- 記錄 bash 命令
- 校驗使用者提示詞
- 傳送團隊通知
- 攔截危險操作(如 `rm -rf`、誤刪 production Redis key)

## 事件總表(共 27 個)

Claude Code 共 **27 個 hook 事件**,依用途分 4 類:

- **Tool Hooks(5 個)**:`PreToolUse`、`PermissionRequest`、`PermissionDenied`、`PostToolUse`、`PostToolUseFailure`
- **Session Hooks(6 個)**:`SessionStart`、`SessionEnd`、`Stop`、`StopFailure`、`SubagentStart`、`SubagentStop`
- **Prompt / Task Hooks(5 個)**:`UserPromptSubmit`、`UserPromptExpansion`、`TaskCreated`、`TaskCompleted`、`TeammateIdle`
- **Lifecycle Hooks(11 個)**:`ConfigChange`、`CwdChanged`、`FileChanged`、`PreCompact`、`PostCompact`、`WorktreeCreate`、`WorktreeRemove`、`Notification`、`InstructionsLoaded`、`Elicitation`、`ElicitationResult`

每個事件的觸發時機、input payload、決策格式請見 [events-reference.md](events-reference.md)。

## Hook 型別

不只是 shell script,共有 5 種 `type`:

- `command`(最常用) — 執行 shell script
- `http` — POST 到外部 endpoint
- `prompt` — 讓小模型做單輪判斷
- `agent`(實驗性) — 用 subagent 多輪驗證
- `mcp_tool`(v2.1.118) — 直接呼叫 MCP server 的工具

> **`duration_ms` 新欄位（v2.1.119）**：用於替工具呼叫設定毫秒級執行時間限制,超時視為 timeout。詳細欄位定義見 [events-reference.md](events-reference.md#通用-config-欄位)。

## 設定檔位置(由高到低優先)

1. Managed policy(組織級)
2. `.claude/settings.local.json`(專案,gitignored)
3. `.claude/settings.json`(專案,通常提交)
4. `~/.claude/settings.json`(使用者全域)
5. Plugin `hooks/hooks.json`
6. Skill / Agent frontmatter 的 `hooks:` 段

## 安裝

```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

然後在 `~/.claude/settings.json` 或 `.claude/settings.json` 裡配置:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.sh"
          }
        ]
      }
    ]
  }
}
```

> **注意**:舊版本教材常寫成 `"hooks": ["path/to/script.sh"]`(字串陣列),新格式必須用 `{ "type": "command", "command": "..." }` 物件包起來。

## 使用方法

Hooks 會在匹配到事件時自動執行。你可以把它理解成 Claude Code 的事件回呼。

- 透過 `/hooks` 斜線命令檢視目前啟用的 hooks
- 需要整批停用時,在 settings.json 加 `"disableAllHooks": true`

## 常見示例

- `format-code.sh` — 寫入前自動格式化
- `pre-commit.sh` — 提交前跑測試
- `security-scan.sh` — 做安全掃描
- `log-bash.sh` — 記錄 bash 命令
- `validate-prompt.sh` — 校驗輸入
- `notify-team.sh` — 發通知

## 最佳實踐

- 把 hooks 保持短小明確
- 只做單一職責
- 先在本地測試(建議放在 `.claude/settings.local.json`,成熟後再 promote)
- 不要在 hook 裡放複雜業務邏輯
- 對副作用保持謹慎(擋 `rm`、送外部通知等,務必先離線測試)
- 敏感環境變數請用 `allowedEnvVars`(http hook),不要硬編碼

## 故障排查

- 檢查檔案路徑和許可權(`chmod +x`)
- 確認 script 能獨立跑(離線用 sample JSON 餵 stdin 試)
- 檢查 settings.json 語法(JSON 格式、新版 `type: "command"` 物件)
- `/hooks` 命令檢查是否被載入
- 檢視 Claude Code 版本相容性(某些欄位如 `if`、`async` 需較新版本)

## 相關概念

- [Hooks 事件完整參考](events-reference.md)
- [Checkpoints and Rewind](../08-checkpoints/README.md)
- [Slash Commands](../01-slash-commands/README.md)
- [Skills](../03-skills/README.md)
- [Subagents](../04-subagents/README.md)
- [Plugins](../07-plugins/README.md)
- [Advanced Features](../09-advanced-features/README.md)

## 更多資源

- [Memory Guide](../02-memory/README.md)
- [Official Hooks Documentation](https://code.claude.com/docs/en/hooks)
- [Official Hooks Guide](https://code.claude.com/docs/en/hooks-guide)
- [CLI Reference](https://code.claude.com/docs/en/cli-reference)
