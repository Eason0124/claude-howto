# 受管 MCP(Managed MCP)

讓組織集中控管哪些 MCP server 可被使用、哪些不行。適合企業 IT 或 SRE 部署。

## 兩種模式

### Option 1:完全獨佔(`managed-mcp.json`)

系統管理員定義的 server 清單,使用者**完全無法**新增 / 修改 / 停用。

### Option 2:Allow / Deny 清單(policy-based)

使用者仍可自己加 server,但必須在允許清單內(或不在禁止清單內)。

## 檔案路徑

### `managed-mcp.json`(主設定)

| 平台 | 路徑 |
|------|------|
| macOS | `/Library/Application Support/ClaudeCode/managed-mcp.json` |
| Linux / WSL | `/etc/claude-code/managed-mcp.json` |
| Windows | `C:\Program Files\ClaudeCode\managed-mcp.json` |

### `managed-mcp.d/`(drop-in directory)

多個檔案會合併,方便 MDM / Puppet / Ansible 分段部署:

| 平台 | 路徑 |
|------|------|
| macOS | `/Library/Application Support/ClaudeCode/managed-mcp.d/` |
| Linux / WSL | `/etc/claude-code/managed-mcp.d/` |
| Windows | `C:\Program Files\ClaudeCode\managed-mcp.d\` |

## Option 1:獨佔設定範例

```json
{
  "managedMcpServers": {
    "internal-knowledge": {
      "type": "http",
      "url": "https://kb.internal.example/mcp",
      "headers": { "Authorization": "Bearer ${INTERNAL_KB_TOKEN}" }
    }
  }
}
```

此模式下:
- 所有使用者自動擁有 `internal-knowledge`
- 使用者 `.mcp.json` / `claude mcp add` 增加的 server 視情況被允許或被拒(依 Option 2 規則)

## Option 2:Allow / Deny 清單

Managed settings(通常放在 managed 的 `settings.json`)中:

| 欄位 | 說明 |
|------|------|
| `allowedMcpServers` | 允許清單 |
| `deniedMcpServers` | 禁止清單(**絕對優先**) |
| `allowManagedMcpServersOnly` | 只允許 `managedMcpServers` 內的 |
| `enableAllProjectMcpServers` | 自動核准所有 `.mcp.json` 中的 server |
| `enabledMcpjsonServers` | `.mcp.json` 的白名單(按 server name) |
| `disabledMcpjsonServers` | `.mcp.json` 的黑名單 |

## 三種匹配方式

清單中的條目可用三種 key 匹配:

### 1. `serverName`

```json
{
  "allowedMcpServers": [
    { "serverName": "github" },
    { "serverName": "mongodb" }
  ]
}
```

### 2. `serverCommand`(stdio server,完全匹配 command + args)

```json
{
  "deniedMcpServers": [
    {
      "serverCommand": ["npx", "-y", "some-sketchy-mcp"]
    }
  ]
}
```

### 3. `serverUrl`(HTTP server,支援 `*` wildcard)

```json
{
  "allowedMcpServers": [
    { "serverUrl": "https://mcp.internal.example/*" },
    { "serverUrl": "https://api.githubcopilot.com/mcp" }
  ]
}
```

## 優先順序

- **Deny 永遠贏**:任何 server 只要中 deny 條件就被擋,即使同時在 allow 清單也不放行
- `allowManagedMcpServersOnly: true` 最嚴格:只有 `managedMcpServers` 內的可用
- `enableAllProjectMcpServers: true` 搭配 deny 清單,是常見的「自動信任專案、封鎖黑名單」模式

## 常見部署情境

### 情境 A:只允許公司內部 MCP

```json
{
  "allowManagedMcpServersOnly": true,
  "managedMcpServers": {
    "internal-kb": { "type": "http", "url": "https://kb.internal/mcp" }
  }
}
```

### 情境 B:允許知名公有 + 封禁未經審核

```json
{
  "allowedMcpServers": [
    { "serverUrl": "https://api.githubcopilot.com/mcp" },
    { "serverUrl": "https://mcp.*.internal.example/*" }
  ],
  "deniedMcpServers": [
    { "serverCommand": ["npx", "-y", "*-unofficial-*"] }
  ]
}
```

### 情境 C:專案 `.mcp.json` 自動信任(但黑名單擋特定)

```json
{
  "enableAllProjectMcpServers": true,
  "disabledMcpjsonServers": ["dangerous-tool", "legacy-service"]
}
```

## 驗證部署

```bash
# 列出目前可用的 servers,看 managed 是否生效
claude mcp list

# 試一個禁止清單裡的 server,應該會被拒
claude mcp add --transport http test https://denied.example/
```

## 相關文件

- [MCP 總覽](README.md)
- [官方 Settings 文件](https://code.claude.com/docs/en/settings)
- [官方 MCP 文件](https://code.claude.com/docs/en/mcp)
