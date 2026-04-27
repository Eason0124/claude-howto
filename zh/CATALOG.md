<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目錄

> 一份關於 Claude Code 所有功能的快速參考：命令、agents、skills、plugins 和 hooks。

**導航**: [命令](#斜槓命令) | [許可權模式](#許可權模式) | [Subagents](#subagents) | [Skills](#skills) | [Plugins](#plugins) | [MCP Servers](#mcp-servers) | [Hooks](#hooks) | [Memory](#memory-files) | [2026 年 3 月新功能](#2026-年-3-月新功能)

---

## 概覽

| 功能 | 內建 | 示例 | 總數 | 參考 |
|---------|----------|----------|-------|-----------|
| **Slash Commands** | 55+ | 8 | 63+ | [01-slash-commands/README.md](01-slash-commands/README.md) |
| **Subagents** | 6 | 10 | 16 | [04-subagents/README.md](04-subagents/README.md) |
| **Skills** | 5 個內建 | 4 | 9 | [03-skills/README.md](03-skills/README.md) |
| **Plugins** | - | 3 | 3 | [07-plugins/README.md](07-plugins/README.md) |
| **MCP Servers** | 1 | 8 | 9 | [05-mcp/README.md](05-mcp/README.md) |
| **Hooks** | 25 個事件 | 7 | 7 | [06-hooks/README.md](06-hooks/README.md) |
| **Memory** | 7 種型別 | 3 | 3 | [02-memory/README.md](02-memory/README.md) |
| **總計** | **99** | **43** | **117** | |

---

## 斜槓命令

命令是使用者手動觸發的快捷操作，用來執行特定任務。

### 內建命令

| 命令 | 說明 | 適用場景 |
|---------|-------------|-------------|
| `/help` | 顯示幫助資訊 | 開始上手、學習命令 |
| `/btw` | 提出不寫入上下文的插話問題 | 臨時補充問題 |
| `/chrome` | 配置 Chrome 整合 | 瀏覽器自動化 |
| `/clear` | 清空對話歷史 | 重新開始、減少上下文 |
| `/diff` | 互動式 diff 檢視器 | 審查變更 |
| `/config` | 檢視/編輯配置 | 自定義行為 |
| `/status` | 顯示會話狀態 | 檢查當前狀態 |
| `/agents` | 列出可用 agents | 檢視委派選項 |
| `/skills` | 列出可用 skills | 檢視可自動觸發的能力 |
| `/hooks` | 列出已配置的 hooks | 除錯自動化 |
| `/insights` | 分析會話模式 | 最佳化會話使用 |
| `/install-slack-app` | 安裝 Claude Slack 應用 | Slack 整合 |
| `/keybindings` | 自定義鍵盤快捷鍵 | 按鍵定製 |
| `/mcp` | 列出 MCP servers | 檢查外部整合 |
| `/memory` | 檢視已載入的 memory 檔案 | 除錯上下文載入 |
| `/mobile` | 生成移動端二維碼 | 手機訪問 |
| `/passes` | 檢視使用通行證 | 訂閱資訊 |
| `/plugin` | 管理 plugins | 安裝/移除擴充套件 |
| `/plan` | 進入規劃模式 | 複雜實現 |
| `/rewind` | 回退到 checkpoint | 撤銷變更、探索替代方案 |
| `/checkpoint` | 管理 checkpoints | 儲存/恢復狀態 |
| `/cost` | 顯示 token 消耗 | 監控開銷 |
| `/context` | 顯示上下文視窗使用情況 | 管理對話長度 |
| `/export` | 匯出對話 | 儲存以供參考 |
| `/extra-usage` | 配置額外用量限制 | 限流管理 |
| `/feedback` | 提交反饋或 bug 報告 | 報告問題 |
| `/login` | 透過 Anthropic 認證 | 訪問功能 |
| `/logout` | 登出 | 切換賬號 |
| `/sandbox` | 切換 sandbox 模式 | 安全執行命令 |
| `/doctor` | 執行診斷 | 排查問題 |
| `/reload-plugins` | 重新載入已安裝的 plugins | 外掛管理 |
| `/release-notes` | 顯示更新說明 | 檢視新功能 |
| `/remote-control` | 啟用遠端控制 | 遠端訪問 |
| `/permissions` | 管理許可權 | 控制訪問 |
| `/session` | 管理會話 | 多會話工作流 |
| `/rename` | 重新命名當前會話 | 組織會話 |
| `/resume` | 恢復之前的會話 | 繼續工作 |
| `/todo` | 檢視/管理待辦列表 | 跟蹤任務 |
| `/tasks` | 檢視後臺任務 | 監控非同步操作 |
| `/copy` | 複製最後一次回覆到剪貼簿 | 快速分享輸出 |
| `/teleport` | 將會話轉移到另一臺機器 | 遠端繼續工作 |
| `/desktop` | 開啟 Claude Desktop 應用 | 切換桌面介面 |
| `/theme` | 更改顏色主題（v2.1.118 起支援 `~/.claude/themes/` JSON 自訂） | 自定義外觀 |
| `/tui` | 無閃爍全螢幕渲染（v2.1.110） | TUI 全螢幕模式 |
| `/focus` | 進入 focus view（v2.1.110） | 隱藏 UI 專注工作 |
| `/ultrareview` | 雲端全面 code review skill（v2.1.111） | 高階程式碼審查 |
| `/less-permission-prompts` | 減少權限提示 skill（v2.1.111） | 自動授權常用操作 |
| `/usage` | 合併 `/cost` + `/stats`（v2.1.118） | 統一 API 使用監控 |
| `/fork` | 分叉當前對話 | 探索替代方案 |
| `/stats` | 顯示會話統計 | 檢視會話指標 |
| `/statusline` | 配置狀態列 | 自定義狀態顯示 |
| `/stickers` | 檢視會話貼紙 | 趣味獎勵 |
| `/fast` | 切換快速輸出模式 | 加快響應 |
| `/terminal-setup` | 配置終端整合 | 設定終端特性 |
| `/upgrade` | 檢查更新 | 版本管理 |

### 自定義命令（示例）

| 命令 | 說明 | 適用場景 | 作用域 | 安裝 |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 分析程式碼以做最佳化 | 效能改進 | 專案 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 準備 Pull Request | 提交 PR 前 | 專案 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 生成 API 檔案 | 編寫 API 檔案 | 專案 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 結合上下文建立 git commit | 提交變更 | 使用者 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 先暫存、提交再 push | 快速釋出 | 使用者 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重構檔案結構 | 改進檔案 | 專案 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 搭建 CI/CD 流水線 | 新專案 | 專案 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 擴充套件測試覆蓋率 | 改進測試 | 專案 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **作用域**: `User` = 個人工作流（`~/.claude/commands/`），`Project` = 團隊共享（`.claude/commands/`）

**參考**: [01-slash-commands/README.md](01-slash-commands/README.md) | [官方檔案](https://code.claude.com/docs/en/interactive-mode)

**快速安裝（所有自定義命令）**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 許可權模式

Claude Code 提供 6 種許可權模式，用來控制工具呼叫如何被授權。

| 模式 | 說明 | 適用場景 |
|------|-------------|-------------|
| `default` | 每次工具呼叫都詢問 | 標準互動式使用 |
| `acceptEdits` | 自動接受檔案編輯，其他情況仍詢問 | 可信編輯工作流 |
| `plan` | 只允許只讀工具，不允許寫入 | 規劃與探索 |
| `auto` | 不再提示，自動接受所有工具 | 完全自主執行（Research Preview） |
| `bypassPermissions` | 跳過所有許可權檢查 | CI/CD、無頭環境 |
| `dontAsk` | 跳過需要許可權的工具 | 非互動指令碼 |

> **注意**：`auto` 模式是 Research Preview 功能（2026 年 3 月）。只有在可信且已隔離的環境中才使用 `bypassPermissions`。

**參考**: [官方檔案](https://code.claude.com/docs/en/permissions)

---

## Subagents

為特定任務準備的專門化 AI 助手，擁有隔離上下文。

### 內建 Subagents

| Agent | 說明 | 工具 | 模型 | 適用場景 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | 多步任務、研究 | 所有工具 | 繼承當前模型 | 複雜研究、多檔案任務 |
| **Plan** | 實現規劃 | Read、Glob、Grep、Bash | 繼承當前模型 | 架構設計、規劃 |
| **Explore** | 程式碼庫探索 | Read、Glob、Grep | Haiku 4.5 | 快速搜尋、理解程式碼 |
| **Bash** | 命令執行 | Bash | 繼承當前模型 | git 操作、終端任務 |
| **statusline-setup** | 狀態列配置 | Bash、Read、Write | Sonnet 4.6 | 配置狀態列顯示 |
| **Claude Code Guide** | 幫助與檔案 | Read、Glob、Grep | Haiku 4.5 | 獲取幫助、學習功能 |

### Subagent 配置欄位

| 欄位 | 型別 | 說明 |
|-------|------|-------------|
| `name` | string | agent 標識 |
| `description` | string | 這個 agent 的用途 |
| `model` | string | 模型覆蓋值（例如 `haiku-4.5`） |
| `tools` | array | 允許使用的工具列表 |
| `effort` | string | 推理強度等級（`low`、`medium`、`high`） |
| `initialPrompt` | string | agent 啟動時注入的 system prompt |
| `disallowedTools` | array | 明確禁止該 agent 使用的工具 |

### 自定義 Subagents（示例）

| Agent | 說明 | 適用場景 | 作用域 | 安裝 |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 全面的程式碼質量檢查 | 程式碼審查會話 | 專案 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 功能架構設計 | 新功能規劃 | 專案 | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 深入分析程式碼庫 | 理解已有功能 | 專案 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | 按 Clean Code 原則審查 | 可維護性審查 | 專案 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 測試策略與覆蓋率 | 測試規劃 | 專案 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技術檔案編寫 | API 檔案、指南 | 專案 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 面向安全的審查 | 安全審計 | 專案 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能實現 | 功能開發 | 專案 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根因分析 | Bug 調查 | 使用者 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查詢、資料分析 | 資料任務 | 使用者 | `cp 04-subagents/data-scientist.md .claude/agents/` |

> **作用域**: `User` = 個人（`~/.claude/agents/`），`Project` = 團隊共享（`.claude/agents/`）

**參考**: [04-subagents/README.md](04-subagents/README.md) | [官方檔案](https://code.claude.com/docs/en/sub-agents)

**快速安裝（所有自定義 agents）**：
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## Skills

可自動觸發的能力包，包含說明、指令碼和模板。

### 示例 Skills

| Skill | 說明 | 何時自動觸發 | 作用域 | 安裝 |
|-------|-------------|-------------------|-------|--------------|
| `code-review` | 全面的程式碼審查 | “Review this code”, “Check quality” | 專案 | `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` | 品牌一致性檢查器 | 編寫營銷文案時 | 專案 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 檔案生成器 | “Generate docs”, “Document API” | 專案 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系統化程式碼重構（Martin Fowler） | “Refactor this”, “Clean up code” | 使用者 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **作用域**: `User` = 個人（`~/.claude/skills/`），`Project` = 團隊共享（`.claude/skills/`）

### Skill 結構

```
~/.claude/skills/skill-name/
├── SKILL.md          # skill 定義與說明
├── scripts/          # 輔助指令碼
└── templates/        # 輸出模板
```

### Skill Frontmatter 欄位

Skills 支援在 `SKILL.md` 中使用 YAML frontmatter 進行配置：

| 欄位 | 型別 | 說明 |
|-------|------|-------------|
| `name` | string | skill 顯示名稱 |
| `description` | string | 這個 skill 的作用 |
| `autoInvoke` | array | 自動觸發的關鍵詞 |
| `effort` | string | 推理強度等級（`low`、`medium`、`high`） |
| `shell` | string | 指令碼使用的 shell（`bash`、`zsh`、`sh`） |

**參考**: [03-skills/README.md](03-skills/README.md) | [官方檔案](https://code.claude.com/docs/en/skills)

**快速安裝（所有 skills）**：
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 內建 Skills

| Skill | 說明 | 何時自動觸發 |
|-------|-------------|-------------------|
| `/simplify` | 審查程式碼質量 | 寫完程式碼後 |
| `/batch` | 對多個檔案執行提示詞 | 批次操作 |
| `/debug` | 除錯失敗的測試/錯誤 | 除錯會話 |
| `/loop` | 按間隔執行提示詞 | 週期性任務 |
| `/claude-api` | 使用 Claude API 構建應用 | API 開發 |

---

## Plugins

把 commands、agents、MCP servers 和 hooks 打包在一起的集合。

### 示例 Plugins

| Plugin | 說明 | 元件 | 適用場景 | 作用域 | 安裝 |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PR 審查工作流 | 3 個 commands、3 個 agents、GitHub MCP | 程式碼審查 | 專案 | `/plugin install pr-review` |
| `devops-automation` | 部署與監控 | 4 個 commands、3 個 agents、K8s MCP | DevOps 任務 | 專案 | `/plugin install devops-automation` |
| `documentation` | 檔案生成套件 | 4 個 commands、3 個 agents、模板 | 檔案編寫 | 專案 | `/plugin install documentation` |

> **作用域**: `Project` = 團隊共享，`User` = 個人工作流

### Plugin 結構

```
.claude-plugin/
├── plugin.json       # manifest 檔案
├── commands/         # Slash commands
├── agents/           # Subagents
├── skills/           # Skills
├── mcp/              # MCP 配置
├── hooks/            # Hook 指令碼
└── scripts/          # 工具指令碼
```

**參考**: [07-plugins/README.md](07-plugins/README.md) | [官方檔案](https://code.claude.com/docs/en/plugins)

**Plugin 管理命令**：
```bash
/plugin list              # 列出已安裝的 plugins
/plugin install <name>    # 安裝 plugin
/plugin remove <name>     # 移除 plugin
/plugin update <name>     # 更新 plugin
```

---

## MCP Servers

用於訪問外部工具和 API 的 Model Context Protocol servers。

### 常見 MCP Servers

| Server | 說明 | 適用場景 | 作用域 | 安裝 |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR 管理、issues、程式碼 | GitHub 工作流 | 專案 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查詢、資料訪問 | 資料庫操作 | 專案 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 高階檔案操作 | 複雜檔案任務 | 使用者 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 團隊溝通 | 通知、更新 | 專案 | 在設定中配置 |
| **Google Docs** | 檔案訪問 | 檔案編輯、審閱 | 專案 | 在設定中配置 |
| **Asana** | 專案管理 | 任務跟蹤 | 專案 | 在設定中配置 |
| **Stripe** | 支付資料 | 財務分析 | 專案 | 在設定中配置 |
| **Memory** | 持久記憶 | 跨會話回憶 | 使用者 | 在設定中配置 |
| **Context7** | 庫檔案 | 查詢最新檔案 | 內建 | 內建 |

> **作用域**: `Project` = 團隊（`.mcp.json`），`User` = 個人（`~/.claude.json`），`Built-in` = 預裝

### MCP 配置示例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**參考**: [05-mcp/README.md](05-mcp/README.md) | [MCP 協議檔案](https://modelcontextprotocol.io)

**快速安裝（GitHub MCP）**：
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## Hooks

在 Claude Code 事件發生時執行的事件驅動自動化。

### Hook 事件

| 事件 | 說明 | 觸發時機 | 使用場景 |
|-------|-------------|----------------|-----------|
| `SessionStart` | 會話開始/恢復 | 會話初始化 | 初始化任務 |
| `InstructionsLoaded` | 指令已載入 | `CLAUDE.md` 或規則檔案載入 | 自定義指令處理 |
| `UserPromptSubmit` | 提示詞提交前 | 使用者傳送訊息 | 輸入校驗 |
| `PreToolUse` | 工具執行前 | 任意工具執行之前 | 校驗、日誌 |
| `PermissionRequest` | 顯示許可權對話方塊 | 敏感操作前 | 自定義審批流程 |
| `PostToolUse` | 工具成功後 | 任意工具完成後 | 格式化、通知 |
| `PostToolUseFailure` | 工具執行失敗 | 工具報錯後 | 錯誤處理、日誌 |
| `Notification` | 傳送通知時 | Claude 傳送通知 | 外部提醒 |
| `SubagentStart` | 啟動 subagent | subagent 任務開始 | 初始化上下文 |
| `SubagentStop` | subagent 完成 | subagent 任務結束 | 鏈式動作 |
| `Stop` | Claude 完成響應 | 響應完成 | 清理、彙報 |
| `StopFailure` | API 錯誤導致結束 | API 錯誤發生 | 錯誤恢復、日誌 |
| `TeammateIdle` | 隊友 agent 空閒 | agent team 協調 | 分配工作 |
| `TaskCompleted` | 任務標記完成 | 任務完成 | 任務後處理 |
| `TaskCreated` | 透過 TaskCreate 建立任務 | 新任務建立 | 任務追蹤、日誌 |
| `ConfigChange` | 配置更新 | 設定被修改 | 響應配置變化 |
| `CwdChanged` | 當前工作目錄變化 | 目錄切換 | 目錄級初始化 |
| `FileChanged` | 監控檔案發生變化 | 檔案被修改 | 檔案監控、重建 |
| `PreCompact` | 壓縮前 | 上下文壓縮前 | 狀態保留 |
| `PostCompact` | 壓縮完成後 | 壓縮完成 | 壓縮後動作 |
| `WorktreeCreate` | worktree 建立中 | git worktree 建立 | 設定 worktree 環境 |
| `WorktreeRemove` | worktree 被移除 | git worktree 刪除 | 清理 worktree 資源 |
| `Elicitation` | MCP server 請求輸入 | MCP elicitation | 輸入校驗 |
| `ElicitationResult` | 使用者響應 elicitation | 使用者回答 | 響應處理 |
| `SessionEnd` | 會話結束 | 會話終止 | 清理、儲存狀態 |

### 示例 Hooks

| Hook | 說明 | 事件 | 作用域 | 安裝 |
|------|-------------|-------|-------|--------------|
| `validate-bash.py` | 命令校驗 | PreToolUse:Bash | 專案 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 安全掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自動格式化 | PostToolUse:Write | 使用者 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | 提示詞校驗 | UserPromptSubmit | 專案 | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | token 使用跟蹤 | Stop | 使用者 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | 提交前校驗 | PreToolUse:Bash | 專案 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 命令日誌記錄 | PostToolUse:Bash | 使用者 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |

> **作用域**: `Project` = 團隊（`.claude/settings.json`），`User` = 個人（`~/.claude/settings.json`）

### Hook 配置

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**參考**: [06-hooks/README.md](06-hooks/README.md) | [官方檔案](https://code.claude.com/docs/en/hooks)

**快速安裝（所有 hooks）**：
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## Memory Files

會在多個會話之間自動載入的持久上下文。

### Memory 型別

| 型別 | 位置 | 作用域 | 適用場景 |
|------|----------|-------|-------------|
| **Managed Policy** | 組織管理的策略 | Organization | 統一組織標準 |
| **Project** | `./CLAUDE.md` | Project（團隊） | 團隊規範、專案上下文 |
| **Project Rules** | `.claude/rules/` | Project（團隊） | 模組化專案規則 |
| **User** | `~/.claude/CLAUDE.md` | User（個人） | 個人偏好 |
| **User Rules** | `~/.claude/rules/` | User（個人） | 模組化個人規則 |
| **Local** | `./CLAUDE.local.md` | Local（git 忽略） | 機器特定覆蓋（截至 2026 年 3 月不在官方檔案中，可能是歷史遺留） |
| **Auto Memory** | 自動 | Session | 自動捕捉的洞察和修正 |

> **作用域**: `Organization` = 管理員管理，`Project` = 透過 git 與團隊共享，`User` = 個人偏好，`Local` = 不提交，`Session` = 自動管理

**參考**: [02-memory/README.md](02-memory/README.md) | [官方檔案](https://code.claude.com/docs/en/memory)

**快速安裝**：
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 2026 年 3 月新功能

| 功能 | 說明 | 使用方式 |
|---------|-------------|------------|
| **Remote Control** | 透過 API 遠端控制 Claude Code 會話 | 使用遠端控制 API 以程式設計方式傳送提示並接收響應 |
| **Web Sessions** | 在瀏覽器環境中執行 Claude Code | 透過 `claude web` 或 Anthropic Console 訪問 |
| **Desktop App** | Claude Code 的原生桌面應用 | 使用 `/desktop` 或從 Anthropic 網站下載 |
| **Agent Teams** | 協調多個 agent 共同處理相關任務 | 配置協作並共享上下文的 teammate agents |
| **Task List** | 後臺任務管理與監控 | 使用 `/tasks` 檢視和管理後臺操作 |
| **Prompt Suggestions** | 上下文感知的命令建議 | 根據當前上下文自動出現建議 |
| **Git Worktrees** | 用於並行開發的隔離 git worktree | 使用 worktree 命令進行安全的並行分支工作 |
| **Sandboxing** | 安全隔離的執行環境 | 使用 `/sandbox` 切換，在受限環境中執行命令 |
| **MCP OAuth** | 為 MCP servers 提供 OAuth 認證 | 在 MCP server 設定中配置 OAuth 憑據以安全訪問 |
| **MCP Tool Search** | 動態搜尋和發現 MCP 工具 | 使用工具搜尋查詢已連線 server 上可用的 MCP 工具 |
| **Scheduled Tasks** | 使用 `/loop` 和 cron 工具設定週期任務 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome Integration** | 使用無頭 Chromium 做瀏覽器自動化 | 使用 `--chrome` 標誌或 `/chrome` 命令 |
| **Keyboard Customization** | 自定義按鍵對映並支援 chord | 使用 `/keybindings` 或編輯 `~/.claude/keybindings.json` |
| **自動模式（Auto Mode）** | 無需許可權提示的完全自主執行（Research Preview） | 使用 `--mode auto` 或 `/permissions auto`；2026 年 3 月 |
| **通道（Channels）** | 多通道通訊（Telegram、Slack 等）（Research Preview） | 配置 channel plugins；2026 年 3 月 |
| **語音輸入（Voice Dictation）** | 用語音輸入提示詞 | 使用麥克風圖示或語音快捷鍵 |
| **Agent Hook Type** | 觸發 subagent 而不是執行 shell 命令的 hook | 在 hook 配置中設定 `"type": "agent"` |
| **Prompt Hook Type** | 將 prompt 文字注入對話的 hook | 在 hook 配置中設定 `"type": "prompt"` |
| **MCP Elicitation** | MCP servers 可在工具執行期間請求使用者輸入 | 透過 `Elicitation` 和 `ElicitationResult` hook 事件處理 |
| **WebSocket MCP Transport** | 用 WebSocket 連線 MCP server | 在 MCP server 配置中使用 `"transport": "websocket"` |
| **Plugin LSP Support** | 透過 plugins 整合 Language Server Protocol | 在 `plugin.json` 中配置 LSP servers，以獲得編輯器能力 |
| **Managed Drop-ins** | 組織管理的 drop-in 配置（v2.1.83） | 透過 managed policies 由管理員配置，自動應用到所有使用者 |

---

## 快速參考矩陣

### 功能選擇指南

| 需求 | 推薦功能 | 原因 |
|------|---------------------|-----|
| 快捷命令 | Slash Command | 手動、立即執行 |
| 持久上下文 | Memory | 自動載入 |
| 複雜自動化 | Skill | 自動觸發 |
| 專門任務 | Subagent | 隔離上下文 |
| 外部資料 | MCP Server | 實時訪問 |
| 事件自動化 | Hook | 事件觸發 |
| 完整解決方案 | Plugin | 一體化打包 |

### 安裝優先順序

| 優先順序 | 功能 | 命令 |
|----------|---------|---------|
| 1. 必裝 | Memory | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | Slash Commands | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 質量提升 | Subagents | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自動化 | Hooks | `cp 06-hooks/*.sh ~/.claude/hooks/` |
| 5. 整合 | MCP Servers | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 全量打包 | Plugins | `/plugin install <name>` |

---

## 一條命令完成安裝

```bash
# 建立目錄
mkdir -p .claude/commands .claude/agents ~/.claude/hooks ~/.claude/skills

# 安裝所有功能
cp 01-slash-commands/*.md .claude/commands/
cp 04-subagents/*.md .claude/agents/
cp -r 03-skills/* ~/.claude/skills/
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 配置 MCP
export GITHUB_TOKEN="your_token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 使用 Plugins
/plugin install pr-review
/plugin install devops-automation
```

---

## Output Styles、Status Line 與 UI / Keyboard

新增三個主題資料夾,對應官方獨立章節:

| 主題 | 位置 | 用途 |
|------|------|------|
| Output Styles | `11-output-styles/` | 切換 Default / Explanatory / Learning,或自訂工作模式 |
| Status Line | `12-status-line/` | 底部狀態列,顯示 context / cost / git 等 |
| UI / Keyboard | `13-ui-and-keyboard/` | 主題 JSON、Vim visual mode、鍵盤快捷鍵(v2.1.109+) |
| Settings & Env | `10-cli/config-and-env-reference.md` | settings.json 持久化、環境變數、`--from-pr` 擴充(v2.1.119) |

## 其他資源

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Skills Repository](https://github.com/luongnv89/skills) - 現成 skills 集合
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny's Claude Code Workflow](https://x.com/bcherny/status/2007179832300581177) - Claude Code 的創造者分享了他的系統化工作流：並行 agents、共享 `CLAUDE.md`、Plan mode、slash commands、subagents，以及用於長時間自主會話的驗證 hooks。

---

**最後更新**: 2026 年 4 月 27 日
**Claude Code 版本**: 2.1.119
