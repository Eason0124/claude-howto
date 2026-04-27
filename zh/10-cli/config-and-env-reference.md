# Settings、環境變數與 `/config` 完整參考

> 本頁集中收錄 v2.1.110 ~ v2.1.119 之間新增的 settings 欄位、環境變數與 CLI 行為更新。若本文與官方衝突請以 [官方 Settings 文件](https://code.claude.com/docs/en/settings) 為準。

---

## `/config` 與 settings.json 持久化（v2.1.119）

`/config` 命令現在會把變更寫回 `settings.json` 並**根據優先級**選擇正確的層級,不再只暫存於當前會話。

### 優先順序（高 → 低）

1. **Managed policy** — 組織級,集中部署,使用者無法覆寫
2. **`.claude/settings.local.json`** — 專案私密(gitignored)
3. **`.claude/settings.json`** — 專案共享(通常 commit)
4. **`~/.claude/settings.json`** — 使用者全域
5. **Plugin settings** — 外掛內定義

`/config` 預設寫到「使用者全域」,可透過參數指定:

```bash
/config --scope project    # 寫到 .claude/settings.json
/config --scope local      # 寫到 .claude/settings.local.json
/config --scope user       # 寫到 ~/.claude/settings.json(預設)
```

---

## 新增 settings 欄位（v2.1.110 ~ v2.1.119）

### `prUrlTemplate`（v2.1.119）

自訂 code-review URL 樣板。當使用 `--from-pr` 或在 review 流程顯示 PR 連結時套用。

```json
{
  "prUrlTemplate": "https://gitlab.company.com/{owner}/{repo}/merge_requests/{number}"
}
```

預設值依 `--from-pr` 解析的 host 自動推斷(GitHub / GitLab / Bitbucket / GitHub Enterprise)。

### `sandbox.network.deniedDomains`（v2.1.113）

沙盒模式下的 domain blocklist,優先級高於 `allowedDomains`。

```json
{
  "sandbox": {
    "network": {
      "deniedDomains": ["evil.example.com", "*.tracker.io"]
    }
  }
}
```

### `autoScrollEnabled`（v2.1.110）

控制 fullscreen / TUI 模式下是否自動捲動到最新輸出。

```json
{ "autoScrollEnabled": false }
```

預設 `true`。設為 `false` 適合需要固定檢視長輸出的場景(配合 Shift+↑/↓ 手動捲動)。

---

## 新增 / 更新環境變數

### `CLAUDE_CODE_HIDE_CWD`（v2.1.119）

在 transcript / 截圖中隱藏當前工作目錄,避免洩露專案路徑。

```bash
export CLAUDE_CODE_HIDE_CWD=1
claude
```

適合錄影、demo、線上分享 transcript 的場景。

### `DISABLE_UPDATES`（v2.1.118）

完全禁用 Claude Code 自動更新檢查與下載。

```bash
export DISABLE_UPDATES=1
```

適合企業受管環境或需要鎖死版本的 CI/CD pipeline。

### `CLAUDE_CODE_FORK_SUBAGENT`（v2.1.117）

啟用 forked subagent 模式,每個 subagent 在獨立程序執行,真正並行。詳見 [Subagents 指南](../04-subagents/README.md#forked-subagentsv21117)。

```bash
export CLAUDE_CODE_FORK_SUBAGENT=1
```

### `CLAUDE_CODE_USE_POWERSHELL_TOOL`（v2.1.111）

Windows 平台漸進推出。設為 `1` 啟用 PowerShell 工具,設為 `0` 強制使用 cmd / bash。

```powershell
$env:CLAUDE_CODE_USE_POWERSHELL_TOOL = "1"
claude
```

---

## CLI 行為更新

### `--from-pr` 擴充支援（v2.1.119）

過去只支援 GitHub PR 編號或 URL,v2.1.119 起擴充為:

| 平台 | 範例 |
|------|------|
| GitHub | `--from-pr 42` 或 `--from-pr https://github.com/owner/repo/pull/42` |
| GitHub Enterprise | `--from-pr https://github.company.com/owner/repo/pull/42` |
| GitLab | `--from-pr https://gitlab.company.com/group/proj/-/merge_requests/123` |
| Bitbucket | `--from-pr https://bitbucket.org/owner/repo/pull-requests/7` |

URL 顯示由 `prUrlTemplate` 控制(見上)。

### `--print` 尊重 agent frontmatter（v2.1.119）

Print 模式(`-p`)現在會讀取 agent frontmatter 中的 `tools:` 與 `disallowedTools:`,先前只看 CLI flag。優先順序:

1. CLI `--tools` / `--allowedTools` / `--disallowedTools`
2. Agent frontmatter `tools:` / `disallowedTools:`
3. 系統預設

```bash
claude -p --agent secure-reviewer "audit the code"
# 自動套用 secure-reviewer.md 中宣告的 tools 與 disallowedTools
```

### Auto mode `$defaults`（v2.1.118）

Auto mode 自訂規則新增 `$defaults` 變數,用來繼承內建分類器的預設規則,避免使用者規則完全覆蓋。

```yaml
# ~/.claude/settings.json
{
  "autoMode": {
    "rules": [
      "$defaults",
      "allow Bash(git status:*)",
      "deny Bash(rm -rf:*)"
    ]
  }
}
```

執行 `claude auto-mode defaults` 可印出當前內建規則。

---

## 受管設定（v2.1.118 起 WSL 同步繼承）

WSL 環境現在會自動繼承 Windows 主機的受管設定(透過 plist / Registry / 受管檔案部署),不需在 WSL 內重複配置。受管設定優先級**最高**,使用者無法以 `/config` 或 `settings.json` 覆寫被鎖定的欄位。

---

## 快速參考表

| 變數 / 設定 | 版本 | 用途 |
|------------|------|------|
| `prUrlTemplate` | v2.1.119 | 自訂 PR / MR URL 樣板 |
| `sandbox.network.deniedDomains` | v2.1.113 | 沙盒網路黑名單 |
| `autoScrollEnabled` | v2.1.110 | 控制 TUI 自動捲動 |
| `CLAUDE_CODE_HIDE_CWD` | v2.1.119 | 隱藏 cwd |
| `DISABLE_UPDATES` | v2.1.118 | 禁用自動更新 |
| `CLAUDE_CODE_FORK_SUBAGENT` | v2.1.117 | 啟用 forked subagents |
| `CLAUDE_CODE_USE_POWERSHELL_TOOL` | v2.1.111 | Windows PowerShell 工具 |
| `--from-pr` 擴充 | v2.1.119 | 支援 GitLab / Bitbucket / GHE |
| `--print` 讀 agent frontmatter | v2.1.119 | tools / disallowedTools 階層生效 |
| Auto mode `$defaults` | v2.1.118 | 繼承內建規則 |
| WSL 繼承 Windows 受管設定 | v2.1.118 | 跨平台政策一致 |

---

## 相關文件

- [CLI 總覽](README.md)
- [Flags 完整對照表](flags-reference.md)
- [Print 模式深度用法](print-mode.md)
- [Subagents — Forked Subagents](../04-subagents/README.md#forked-subagentsv21117)
- [Hooks — duration_ms 與 mcp_tool 型別](../06-hooks/events-reference.md)
- [官方 Settings 文件](https://code.claude.com/docs/en/settings)
