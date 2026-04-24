<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP(Model Context Protocol)

MCP 是 Claude Code 連上外部工具、服務、API 的標準協議。用它可以把 GitHub、資料庫、K8s、內部工具接進 Claude 的對話裡。

> **深度參考**:
> - [transports.md](transports.md) — stdio / http / sse(已棄用)三種傳輸與自動重連
> - [oauth-setup.md](oauth-setup.md) — OAuth 完整設定與 CLI flag
> - [managed-mcp.md](managed-mcp.md) — 企業集中管理與 allow/deny 策略

## 概覽

MCP 通常由三部分構成:

1. Claude Code(client)
2. MCP server(local process 或 remote endpoint)
3. 外部工具或資料來源(GitHub API、DB、K8s cluster…)

Claude 透過 MCP 協議向 server 發起工具呼叫,server 回傳結構化結果。

## Transport 類型

| Transport | 用途 | 狀態 |
|-----------|------|------|
| `http` | 遠端 server,**官方推薦** | 主力 |
| `stdio` | 本地子程序 | 主力 |
| `sse` | Server-Sent Events | **已棄用**,請改用 `http` |

詳情與自動重連規則見 [transports.md](transports.md)。

## 設定檔:`.mcp.json`

完整欄位表:

| 欄位 | 說明 |
|------|------|
| `type` | `http` / `stdio`(`sse` 已棄用) |
| `command` | stdio server 的可執行檔路徑 |
| `args` | stdio server 的啟動參數陣列 |
| `env` | 環境變數(物件) |
| `url` | HTTP / SSE server URL |
| `headers` | HTTP 靜態 headers |
| `headersHelper` | 動態產生 headers 的 shell 命令(回傳 JSON) |
| `oauth` | OAuth 設定物件(見 [oauth-setup.md](oauth-setup.md)) |
| `authServerMetadataUrl` | OAuth metadata URL override(RFC 8414/9728) |

### 環境變數展開

在 `command` / `args` / `env` / `url` / `headers` 中可使用:

- `${VAR}` — 展開環境變數 `VAR`
- `${VAR:-default}` — 未設定時使用預設值

### 範例

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp",
      "headers": { "Authorization": "Bearer ${GITHUB_TOKEN}" }
    },
    "fs": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "${HOME}/work"]
    }
  }
}
```

## Scope(作用域)

| Scope | 儲存位置 | 共享範圍 |
|-------|---------|---------|
| `local`(預設) | `~/.claude.json`(per-project) | 只有當前專案、不共享 |
| `project` | 專案根目錄的 `.mcp.json` | 跟著 git 共享給團隊 |
| `user` | `~/.claude.json` | 所有專案 |
| `plugin` | 外掛內定義 | 啟用外掛時自動 |

**優先順序**(高 → 低):Local > Project > User > Plugin > claude.ai connectors。

## CLI 指令

```bash
# 新增 stdio server
claude mcp add <name> -- <command> [args...]

# 新增 HTTP server
claude mcp add --transport http <name> <url>

# 從 JSON 新增
claude mcp add-json <name> '<json-config>'

# 從 Claude Desktop 匯入
claude mcp add-from-claude-desktop

# 查看
claude mcp list
claude mcp get <name>

# 移除
claude mcp remove <name>

# 重設 project 級核准選擇
claude mcp reset-project-choices

# 讓 Claude 自己變成一個 MCP server
claude mcp serve
```

### 常用 flags

- `--transport {http|stdio|sse}` — 指定傳輸(`sse` 已棄用)
- `--scope {local|project|user}` — 作用域,預設 `local`
- `--env KEY=value` — 環境變數(可重複)
- `--header "Name: value"` — HTTP headers(可重複)
- `--callback-port <port>` — 固定 OAuth 回呼埠
- `--client-id <id>` / `--client-secret` — OAuth 憑證

## CLI 層級的 MCP 設定覆寫

- `--mcp-config <path|json>` — 載入額外 MCP 設定(檔案或 JSON 字串)
- `--strict-mcp-config` — 只使用 `--mcp-config`,忽略 `.mcp.json` / 全域設定
- `--bare` — 跳過 MCP servers 自動發現(加速啟動)

## MCP Resources(@ 提及)

用 `@` 把 MCP resource 當附件帶進 prompt:

```text
@github:issue://123
@github:pr://456
```

語法:`@<server-name>:<protocol>://<path>`。

## MCP Prompts(變成 slash command)

Server 定義的 prompt 會自動暴露成斜線命令:

```text
/mcp__<server-name>__<prompt-name> [arguments]
```

範例:

```text
/mcp__github__list_prs
/mcp__github__pr_review 456
/mcp__jira__create_issue "Bug title" high
```

## Elicitation(server 主動請使用者輸入)

MCP server 可請求結構化輸入,Claude Code 會彈出對話框。也可透過 `Elicitation` / `ElicitationResult` hook 攔截(見 [06-hooks](../06-hooks/README.md))。

## Tool Search(預設啟用)

Claude Code 預設**延遲載入工具**。環境變數 `ENABLE_TOOL_SEARCH`:

- (unset) — 延遲(預設,非第一方 `ANTHROPIC_BASE_URL` 時關閉)
- `true` — 強制延遲
- `auto` — 閾值模式(10% context)
- `auto:<N>` — 自訂閾值
- `false` — 全量預載

## 輸出限制

- 預設最大 **25,000 tokens**;超過 10,000 會警示
- 環境變數 `MAX_MCP_OUTPUT_TOKENS` 可調整
- 工具可在 `_meta["anthropic/maxResultSizeChars"]` 單獨提高(上限 500,000)

## 動態工具更新

MCP server 發出 `list_changed` 通知時,Claude 會自動刷新工具清單。

## 自動重連

- HTTP / SSE server:指數退避,最多 5 次(1s → 2s → 4s → 8s → 16s)
- Stdio server:**不自動重連**

## Windows 注意

原生 Windows 執行 `npx`、`npm` 等 Node.js 工具時,常需要 `cmd /c` wrapper:

```json
{
  "mcpServers": {
    "example": {
      "type": "stdio",
      "command": "cmd",
      "args": ["/c", "npx", "-y", "some-mcp-server"]
    }
  }
}
```

## 企業:受管 MCP

系統管理員可集中控管:

- `managed-mcp.json` 獨佔(使用者無法改)
- Policy-based allowlist / denylist:`allowedMcpServers`、`deniedMcpServers`

完整規則與路徑見 [managed-mcp.md](managed-mcp.md)。

## Plugin MCP Servers

外掛可打包自己的 MCP server,環境變數:

- `${CLAUDE_PLUGIN_ROOT}` — 外掛安裝目錄
- `${CLAUDE_PLUGIN_DATA}` — 外掛持久化資料目錄

## 最佳實踐

### 安全

- 最小權限原則:只授權必要工具
- Token 用環境變數,不要硬編碼到 `.mcp.json`
- production DB / K8s 用唯讀 server,或用 [Hooks](../06-hooks/README.md) 再擋一層
- `.mcp.json` 公開到 git 前,先確認沒有機敏 URL 或預設 token

### 設定

- 每個 server 加註用途的註解(或 README 中列清單)
- `project` scope 放團隊共享的(如 GitHub)
- `user` scope 放個人工具
- `local` 適合快速試用、尚未成熟的

### 效能

- 先做 filter / pagination,避免一次拉回整張表
- 對大結果善用 Tool Search 延遲載入
- 監控 `MAX_MCP_OUTPUT_TOKENS` 警示,把重結果封裝成彙整工具

## 故障排查

### 找不到 server

- `claude mcp list` 確認有註冊
- 檢查 scope(對的專案嗎?)
- `--mcp-config` 有沒有被 `--strict-mcp-config` 排除

### 認證失敗

- 環境變數有 `export` 嗎?
- Token 權限 / 過期?
- OAuth 請見 [oauth-setup.md](oauth-setup.md)

### 連線超時

- 手動 curl server URL 看通不通
- 檢查防火牆、proxy
- HTTP server 會自動重連 5 次,觀察 log

### Server 崩潰

- stdio server 看 stderr
- 降低併發、簡化請求
- Windows 注意 `cmd /c` wrapper

## 常見 server

| Server | 用途 | 連接方式 |
|--------|------|---------|
| GitHub | issue / PR / repo 操作 | HTTP + Bearer token 或 OAuth |
| GitLab | 同上 | HTTP |
| MongoDB | 查 collection / aggregation | stdio 或 HTTP |
| Redis | get / set / 觀察 | stdio |
| Kubernetes | pod / deployment / log | stdio |
| Filesystem | 讀寫指定目錄 | stdio |

## 相關概念

- [Hooks — 用 Elicitation hook 接 MCP 輸入](../06-hooks/README.md)
- [Memory 中文指南](../02-memory/README.md)
- [Subagents 中文參考](../04-subagents/README.md)
- [Plugins 中文指南](../07-plugins/README.md)

## 更多資源

- [根目錄中文指南](../README.md)
- [MCP 協議規範](https://modelcontextprotocol.io)
- [官方 Claude Code MCP 文件](https://code.claude.com/docs/en/mcp)
- [官方 Settings 文件](https://code.claude.com/docs/en/settings)
