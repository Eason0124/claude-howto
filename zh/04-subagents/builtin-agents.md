# 內建 Subagents

Claude Code 自帶幾個內建 subagent,**不需要自己寫**就能使用。

## 清單

| Agent | 模型 | 工具 | 用途 |
|-------|------|------|------|
| `Explore` | Haiku(快) | 唯讀(禁 Write / Edit) | 搜程式碼、找檔案、探索 repo |
| `Plan` | 繼承主會話 | 唯讀 | Plan mode 下蒐集 context |
| `general-purpose` | 繼承 | 全部 | 複雜多步任務、混合研究與修改 |
| `statusline-setup` | Sonnet | — | 執行 `/statusline` 時被派出 |
| `Claude Code Guide` | Haiku | — | 使用者問 Claude Code 本身問題時 |

## 用法

### 自然語言派出

```text
Use the Explore agent to find all files that touch Redis.
```

### `@` 提及

```text
@"Explore (agent)" 找出所有 WebSocket 連線點
```

這種用法**保證**派出指定 agent。

### 會話級

```bash
claude --agent general-purpose "refactor matchmaking"
```

整個會話都用該 subagent 的 system prompt、tools、model。

## Explore — 用途實例

- "找出所有 payment 相關的 function"
- "列出 src/game/ 下所有 class 的繼承關係"
- "找 Redis key 命名規則不一致的地方"

由於 Haiku 模型、唯讀工具,搜尋時比主會話快且安全。

## Plan — 什麼時候被自動派出

進 Plan mode(`Shift+Tab`)後,Claude 若需要蒐集更多 context(讀多個檔),會自動派 Plan subagent,避免污染主會話 context。

## general-purpose — 推薦用法

多步驟且需混合搜尋 + 修改時:

- "重構 src/game/matchmaker.ts,拆成 MatchFinder 和 MatchRoom 兩個 class"
- "跨多支檔找出所有未處理的 error,加上 try/catch"
- "整合三個 module 的 logger 呼叫到單一 wrapper"

不建議用於:

- 只需要搜尋(用 Explore)
- 只需要單檔小改(主會話直接做)
- 一次性問答(主會話直接答)

## 管理 / 觀察

```bash
# 看所有已設定 subagent(內建 + 自訂 + plugin)
claude agents

# 互動分頁管理(Running / Library)
claude --agents
# ↑ 注意:這裡的 --agents 是開啟管理介面,不同於 --agents '{JSON}' 定義 subagent
```

## 相關文件

- [Subagents 總覽](README.md)
- [Frontmatter 完整欄位](frontmatter-reference.md)
- [官方 Sub-agents 文件](https://code.claude.com/docs/en/sub-agents)
