# Subagent Frontmatter 完整欄位參考

> 依官方 [Sub-agents](https://code.claude.com/docs/en/sub-agents) 頁面整理。若本文與官方衝突以官方為準。

## 檔案格式

Subagent 是 markdown 檔,置於:

- `.claude/agents/<name>.md`(專案級)
- `~/.claude/agents/<name>.md`(使用者級)
- Plugin 的 `agents/` 目錄
- Managed settings 的 `.claude/agents/`(組織級,最高優先)

YAML frontmatter 後接 markdown 說明 / system prompt。

## 完整欄位表

| 欄位 | 必填 | 說明 | 預設 |
|------|------|------|------|
| `name` | ✅ | 小寫英文 / 連字號的唯一識別符 | — |
| `description` | ✅ | 何時應該委派給此 subagent(Claude 自動判斷的依據) | — |
| `tools` | ❌ | 可用工具清單(字串陣列或 comma-separated) | 繼承全部 |
| `disallowedTools` | ❌ | 從 `tools` 或繼承清單中扣除的工具 | — |
| `model` | ❌ | `sonnet` / `opus` / `haiku` / 完整 ID / `inherit` | `inherit` |
| `permissionMode` | ❌ | 見下方 Permission Modes | 繼承主會話 |
| `maxTurns` | ❌ | subagent 停止前的最大輪數 | 無上限 |
| `skills` | ❌ | 啟動時要注入 context 的 skill 名稱 | — |
| `mcpServers` | ❌ | 可用的 MCP servers(內聯定義或字串參照) | 繼承主會話 |
| `hooks` | ❌ | subagent 生命週期的 hooks(見下方 Hooks) | — |
| `memory` | ❌ | 持久記憶範圍:`user` / `project` / `local` | — |
| `background` | ❌ | 是否以背景模式執行 | `false` |
| `effort` | ❌ | 推理強度,覆蓋主會話等級 | 繼承 |
| `isolation` | ❌ | 設為 `worktree` 則在臨時 git worktree 內執行 | — |
| `color` | ❌ | 在任務清單中顯示的顏色 | — |
| `initialPrompt` | ❌ | 第一個使用者 turn 自動送出的初始 prompt | — |

## Permission Modes

| 模式 | 行為 |
|------|------|
| `default` | 標準權限檢查 |
| `acceptEdits` | 自動接受工作目錄(或 `additionalDirectories`)內的檔案編輯 |
| `auto` | 背景分類器判斷指令是否安全 |
| `dontAsk` | 自動拒絕所有權限提示 |
| `bypassPermissions` | 跳過所有提示(⚠️ 風險極高) |
| `plan` | Plan mode(唯讀) |

## Hooks(frontmatter 內)

只在 subagent 執行期間觸發。支援事件:

- `PreToolUse` — subagent 使用工具前
- `PostToolUse` — subagent 使用工具後
- `Stop` — subagent 完成(對主會話而言等同 `SubagentStop`)

主會話的 `settings.json` 另外支援:

- `SubagentStart` — subagent 啟動
- `SubagentStop` — subagent 終止

## 工具限制

### `tools` + `disallowedTools` 規則

- 同時列出時,**`disallowedTools` 優先**(先擴展 tools,再扣除 disallowed)
- 省略 `tools` = 繼承全部
- `tools: []` = 完全無工具

### Agent 工具限制(誰能派生哪些 subagent)

`Agent(agent_type)` 語法控制主會話可派生的 subagent:

```yaml
tools: Agent(worker, researcher), Read, Bash
```

- `Agent(a, b)` — 只允許派生 `a` 和 `b`
- `Agent`(無括號) — 允許派生任何 subagent
- 省略 `Agent` — 主會話不能派生任何 subagent

**重要限制**:subagent 本身**不能**派生其他 subagent。在 subagent frontmatter 裡寫 `Agent(...)` 無效。若要巢狀委派,用 skills 串接或從主會話鏈接。

## MCP servers 欄位

### 方式 1:內聯定義

```yaml
mcpServers:
  my-db:
    type: stdio
    command: mcp-postgres
    args: ["--readonly"]
```

與 `.mcp.json` 同 schema。

### 方式 2:字串參照(共享主會話連線)

```yaml
mcpServers: [github, mongodb]
```

## 範例 frontmatter

### 最小

```yaml
---
name: code-reviewer
description: Use proactively after any code change to scan for bugs.
---

You are a senior code reviewer. Focus on...
```

### 進階

```yaml
---
name: db-scribe
description: Query production DB (read-only) to gather evidence.
model: sonnet
tools: [Read, Grep, Glob, Bash]
disallowedTools: [Edit, Write]
permissionMode: acceptEdits
maxTurns: 15
mcpServers: [mongodb-readonly]
isolation: worktree
color: blue
effort: high
hooks:
  PostToolUse:
    - type: command
      matcher: Bash
      command: $CLAUDE_PROJECT_DIR/.claude/hooks/audit-db.sh
---

You are a database query scribe. Never write, only read...
```

### 背景 + 自動首轉

```yaml
---
name: log-watcher
description: Continuously tail app logs and surface anomalies.
background: true
initialPrompt: "Tail app.log for the past 10 minutes, summarize errors."
tools: [Bash, Read]
---
```

## 優先順序(同名衝突)

由高到低:

1. Managed settings(組織)
2. `--agents` CLI flag
3. `.claude/agents/`(專案)
4. `~/.claude/agents/`(使用者)
5. Plugin 的 `agents/`(最低)

## Plugin subagent 限制

外掛 subagent **不支援**以下欄位(安全理由):

- `hooks`
- `mcpServers`
- `permissionMode`

若要使用,複製到 `.claude/agents/` 或 `~/.claude/agents/`。

## 相關文件

- [Subagents 總覽](README.md)
- [內建 Subagents](builtin-agents.md)
- [Hooks 事件表](../06-hooks/events-reference.md)
- [官方 Sub-agents 文件](https://code.claude.com/docs/en/sub-agents)
