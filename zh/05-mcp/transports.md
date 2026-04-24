# MCP Transports 參考

MCP 支援三種 transport:`http`、`stdio`、`sse`(已棄用)。

## `http`(遠端 server,官方推薦)

用於 SaaS 型 MCP server(GitHub、Notion…)。

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp",
      "headers": {
        "Authorization": "Bearer ${GITHUB_TOKEN}"
      }
    }
  }
}
```

### 動態 headers

若 token 必須在每次請求時動態產生(如短期 token、簽章),用 `headersHelper`:

```json
{
  "mcpServers": {
    "internal": {
      "type": "http",
      "url": "https://mcp.internal.example/",
      "headersHelper": "./scripts/gen-mcp-headers.sh"
    }
  }
}
```

`headersHelper` 是 shell 命令,必須輸出 JSON(object)到 stdout。

環境變數:
- `CLAUDE_CODE_MCP_SERVER_NAME` — 當前 server 名
- `CLAUDE_CODE_MCP_SERVER_URL` — 當前 server URL

## `stdio`(本地子程序)

最常見於 npm / pip 發佈的 MCP server。

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "${HOME}/work"]
    }
  }
}
```

### Windows 注意

Windows 原生執行 Node.js 工具(`npx`、`npm`、`npm.cmd`)時,須透過 `cmd /c`:

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-filesystem", "%USERPROFILE%\\work"]
    }
  }
}
```

### Stdio server 不會自動重連

若子程序 crash,必須手動重啟 Claude Code 或 `claude mcp` 重新連接。

## `sse`(已棄用)

Server-Sent Events 版本。**官方標記為 deprecated**,請改用 `http`。

```json
{
  "mcpServers": {
    "legacy": {
      "type": "sse",
      "url": "https://mcp.legacy.example/sse"
    }
  }
}
```

若看到舊教材寫 `type: "sse"`,建議問 server 提供方是否已支援 `http`;多數現代 server 已切換。

## 自動重連策略

| Transport | 自動重連 | 策略 |
|-----------|---------|------|
| `http` | ✅ | 指數退避,最多 5 次(1s, 2s, 4s, 8s, 16s) |
| `sse` | ✅ | 同上(已棄用) |
| `stdio` | ❌ | 不重連,子程序終止後需手動恢復 |

## 動態工具更新

Server 發出 MCP 規範的 `list_changed` 通知時,Claude 自動重新發現工具清單,無需重連。

## 輸出大小管理

- 單次工具呼叫輸出預設上限 **25,000 tokens**,超過 10,000 顯示警示
- 可透過環境變數 `MAX_MCP_OUTPUT_TOKENS` 調整(值為整數 token 數,如 `50000`)
- 特定工具可於 `_meta["anthropic/maxResultSizeChars"]` 提升至最多 500,000 字元

## 常見錯誤與排查

### 「MCP server connection failed」

- `http`:curl 看 URL 是否可達、header auth 是否通
- `stdio`:手動在 terminal 跑一次 `command args`,看 stderr
- 看 `~/.claude/logs/mcp-*.log`(若有)

### `MCP_TIMEOUT` 相關

Server 啟動逾時可用環境變數 `MCP_TIMEOUT` 延長(官方未明確預設秒數,需實測或見最新 release notes)。

### `cmd /c` wrapper 為何需要

Windows 的 `CreateProcess` 無法直接執行 `.cmd` 或 `.bat` 批次檔,Node.js 的 `npx` / `npm` 實際上是 `.cmd`,所以必須透過 `cmd` 來間接啟動。

## 相關文件

- [MCP 總覽](README.md)
- [OAuth 設定](oauth-setup.md)
- [官方 MCP 文件](https://code.claude.com/docs/en/mcp)
