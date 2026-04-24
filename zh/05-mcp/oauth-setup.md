# MCP OAuth 設定

支援 OAuth 的 MCP server(例如 Linear、Notion、部分自建 server),可在 `.mcp.json` 用 `oauth` 物件配置憑證流程。

## `oauth` 物件欄位

| 欄位 | 說明 |
|------|------|
| `clientId` | OAuth client ID |
| `callbackPort` | 固定的 localhost 回呼埠,用於完成 redirect |
| `scopes` | 空格分隔的 scope 字串 |
| `authServerMetadataUrl` | OAuth metadata URL override(符合 RFC 8414 / 9728) |

## 動態 client registration(預設)

若 server 支援 RFC 7591 dynamic client registration,可以不用預先申請 client:

```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "url": "https://mcp.example.com/",
      "oauth": {
        "scopes": "read write",
        "callbackPort": 7777
      }
    }
  }
}
```

## 預設 client(需要 client ID)

有些 server 要求事先註冊 client:

```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "url": "https://mcp.example.com/",
      "oauth": {
        "clientId": "client_xxx_yyy",
        "scopes": "read write",
        "callbackPort": 7777
      }
    }
  }
}
```

`client_secret` 一律從環境變數或互動式 prompt 取得,**不要**寫入 JSON。

## Metadata URL override

若 server 把 authorization / token endpoint 放在非標準路徑,可指定 metadata URL:

```json
{
  "oauth": {
    "authServerMetadataUrl": "https://auth.example.com/.well-known/oauth-authorization-server"
  }
}
```

## CLI 方式設定

加一個帶 OAuth 的 HTTP server:

```bash
claude mcp add --transport http my-server https://mcp.example.com/ \
  --client-id client_xxx_yyy \
  --client-secret \
  --callback-port 7777 \
  --scope local
```

- `--client-secret` 不接值,會觸發隱藏輸入 prompt
- `--callback-port` 與 `oauth.callbackPort` 等效

## 首次認證流程

1. 啟動 Claude Code(或執行 `claude mcp add`)
2. Claude Code 打開瀏覽器到 server 的 authorization endpoint
3. 使用者登入並授權
4. Server 重導向到 `http://localhost:<callbackPort>/...`
5. Claude Code 完成 token 交換,store 在本機 keychain / credential store

後續 session 自動使用已 store 的 token(過期會自動 refresh,若 server 支援 refresh token)。

## 清除已 store 的憑證

```bash
# 重設特定 server 的 OAuth 憑證(會觸發下次重新登入)
claude mcp reset-project-choices
```

完整清除需手動刪除系統 keychain 對應 entry,或重裝 Claude Code。

## 安全建議

- **`callbackPort` 固定**:避免埠跳動,防火牆 / SSH tunnel 更穩定
- **`scopes` 最小化**:只要必要權限
- **避免 HTTP(非 TLS)**:auth 流程必須走 HTTPS
- **公司 SSO 整合**:可透過 `authServerMetadataUrl` 指向內部 IdP

## 常見問題

### 回呼失敗(port 被佔用)

換一個 `callbackPort`,或先 `lsof -i :7777` 找出占用程序。

### Token 過期後沒 refresh

- 確認 server 有回 refresh_token
- 手動 `reset-project-choices` 清除重新登入

### 內網 server + 公司 CA

需把公司 CA 加到系統信任鏈(macOS 用 Keychain、Linux 用 `/etc/ssl/certs/` + `update-ca-certificates`)。

## 相關文件

- [MCP 總覽](README.md)
- [Transports](transports.md)
- [官方 MCP 文件](https://code.claude.com/docs/en/mcp)
