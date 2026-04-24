<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

<p align="center">
  <a href="https://github.com/trending">
    <img src="https://img.shields.io/badge/GitHub-🔥%20%231%20Trending-purple?style=for-the-badge&logo=github"/>
  </a>
</p>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.1.112-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

# 用一個週末掌握 Claude Code

從輸入 `claude` 開始，到編排 agents、hooks、skills 和 MCP servers，全程配有視覺化教程、可直接複製貼上的模板，以及循序漸進的學習路徑。

**[15 分鐘快速上手](#15-分鐘快速上手)** | **[找到適合你的起點](#不知道從哪裡開始)** | **[瀏覽功能目錄](CATALOG.md)**

---

## 目錄

- [問題在哪裡](#問題在哪裡)
- [Claude How To 如何解決這些問題](#claude-how-to-如何解決這些問題)
- [它是如何工作的](#它是如何工作的)
- [不知道從哪裡開始？](#不知道從哪裡開始)
- [15 分鐘快速上手](#15-分鐘快速上手)
- [你能用它做什麼？](#你能用它做什麼)
- [常見問題](#常見問題)
- [參與貢獻](#參與貢獻)
- [許可證](#許可證)

---

## 問題在哪裡

你已經安裝了 Claude Code。也試著跑了幾個提示詞。然後呢？

- **官方檔案會介紹功能，但不會告訴你如何把它們組合起來。** 你知道 slash commands 存在，但不知道怎樣把它們和 hooks、memory、subagents 串起來，形成一個真正能節省數小時的工作流。
- **沒有清晰的學習路徑。** 應該先學 MCP 還是 hooks？先學 skills 還是 subagents？結果往往是每樣都看了一點，但沒有一個真正掌握。
- **示例太基礎。** 一個 “hello world” 級別的 slash command，並不能幫你構建一個可用於生產環境的程式碼審查流水線，也無法讓它結合 memory、委派給專門 agent、並自動執行安全掃描。

你實際上只用上了 Claude Code 10% 的能力，而剩下的 90% 你甚至不知道自己錯過了什麼。

---

## Claude How To 如何解決這些問題

這不是另一份功能參考手冊，而是一份**結構化、視覺化、以示例驅動的指南**。它會教你如何使用 Claude Code 的每一項功能，並提供今天就能複製到專案裡的真實場景模板。

| | 官方檔案 | 本指南 |
|--|---------------|------------|
| **形式** | 參考檔案 | 帶 Mermaid 圖表的視覺化教程 |
| **深度** | 功能說明 | 底層工作原理解析 |
| **示例** | 基礎程式碼片段 | 可立即上手的生產級模板 |
| **結構** | 按功能組織 | 遞進式學習路徑（從入門到高階） |
| **上手方式** | 自主摸索 | 帶時間預估的引導式路線圖 |
| **自我評估** | 沒有 | 互動式測驗，幫你找出短板並生成個性化路徑 |

### 你將獲得：

- **10 個教程模組**，覆蓋 Claude Code 的所有核心功能，從 slash commands 到自定義 agent 團隊
- **可複製貼上的配置**，包括 slash commands、`CLAUDE.md` 模板、hook 指令碼、MCP 配置、subagent 定義，以及完整 plugin 打包
- **Mermaid 圖表**，展示每個功能的內部工作機制，讓你不僅知道“怎麼做”，還知道“為什麼這麼做”
- **一條引導式學習路徑**，幫助你在 11 到 13 小時內從初學者成長為高階使用者
- **內建自測能力**，可以直接在 Claude Code 中執行 `/self-assessment` 或 `/lesson-quiz hooks` 找出知識盲點

**[開始學習路線  ->](LEARNING-ROADMAP.md)**

---

## 它是如何工作的

### 1. 先找到你的水平

完成[自我評估測驗](LEARNING-ROADMAP.md#find-your-level)，或者在 Claude Code 中執行 `/self-assessment`。系統會根據你已經掌握的內容，生成一份個性化學習路線。

### 2. 按引導路徑學習

按照順序學習 10 個模組，每個模組都建立在前一個模組的基礎上。你可以一邊學，一邊把模板直接複製到自己的專案中。

### 3. 把多個功能組合成工作流

真正的威力來自功能組合。你會學到如何把 slash commands、memory、subagents 和 hooks 連線成自動化流水線，用於程式碼審查、部署和檔案生成等任務。

### 4. 測試你的理解程度

每學完一個模組後執行 `/lesson-quiz [topic]`。測驗會精準指出你漏掉的知識點，讓你快速補齊短板。

**[15 分鐘快速上手](#15-分鐘快速上手)**

---

## 已被 21,800+ 開發者信賴

- **21,800+ GitHub stars**，來自每天都在使用 Claude Code 的開發者
- **2,585+ forks**，許多團隊已將這份指南改造成自己的工作流版本
- **持續維護中**，會與每次 Claude Code 釋出保持同步（最新版本：v2.1.112，2026 年 4 月）
- **社群驅動**，貢獻者會分享他們在真實工作中的配置和經驗

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 不知道從哪裡開始？

先做自我評估，或者直接按你的水平開始：

| Level | 你可以…… | 從這裡開始 | 時間 |
|-------|-----------|------------|------|
| **Beginner** | 啟動 Claude Code 並進行對話 | [Slash Commands](01-slash-commands/README.md) | 約 2.5 小時 |
| **Intermediate** | 使用 `CLAUDE.md` 和自定義命令 | [Skills](03-skills/README.md) | 約 3.5 小時 |
| **Advanced** | 配置 MCP servers 和 hooks | [Advanced Features](09-advanced-features/README.md) | 約 5 小時 |

**完整學習路徑，共 10 個模組：**

| 順序 | 模組 | 水平 | 時間 |
|-------|--------|-------|------|
| 1 | [Slash Commands](01-slash-commands/README.md) | 初學者 | 30 分鐘 |
| 2 | [Memory](02-memory/README.md) | 初學者+ | 45 分鐘 |
| 3 | [Checkpoints](08-checkpoints/README.md) | 中級 | 45 分鐘 |
| 4 | [CLI Basics](10-cli/README.md) | 初學者+ | 30 分鐘 |
| 5 | [Skills](03-skills/README.md) | 中級 | 1 小時 |
| 6 | [Hooks](06-hooks/README.md) | 中級 | 1 小時 |
| 7 | [MCP](05-mcp/README.md) | 中級+ | 1 小時 |
| 8 | [Subagents](04-subagents/README.md) | 中級+ | 1.5 小時 |
| 9 | [Advanced Features](09-advanced-features/README.md) | 高階 | 2 到 3 小時 |
| 10 | [Plugins](07-plugins/README.md) | 高階 | 2 小時 |

**[完整學習路線圖 ->](LEARNING-ROADMAP.md)**

---

## 15 分鐘快速上手

```bash
# 1. 克隆這份指南
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. 複製你的第一個 slash command
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 試試看：在 Claude Code 中輸入
# /optimize

# 4. 想更進一步？設定專案記憶：
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. 安裝一個 skill：
cp -r 03-skills/code-review ~/.claude/skills/
```

如果你想完成更完整的基礎配置，這裡有一個**1 小時的關鍵配置方案**：

```bash
# Slash commands（15 分鐘）
cp 01-slash-commands/*.md .claude/commands/

# 專案記憶（15 分鐘）
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 安裝一個 skill（15 分鐘）
cp -r 03-skills/code-review ~/.claude/skills/

# 週末目標：繼續新增 hooks、subagents、MCP 和 plugins
# 按學習路徑逐步完成配置
```

**[檢視完整安裝參考](#15-分鐘快速上手)**

---

## 你能用它做什麼？

| 使用場景 | 你會組合使用的功能 |
|----------|------------------------|
| **自動化程式碼審查** | Slash Commands + Subagents + Memory + MCP |
| **團隊入職培訓** | Memory + Slash Commands + Plugins |
| **CI/CD 自動化** | CLI Reference + Hooks + Background Tasks |
| **檔案生成** | Skills + Subagents + Plugins |
| **安全審計** | Subagents + Skills + Hooks（只讀模式） |
| **DevOps 流水線** | Plugins + MCP + Hooks + Background Tasks |
| **複雜重構** | Checkpoints + Planning Mode + Hooks |

---

## 常見問題

**這是免費的嗎？**
是的。MIT 許可證，永久免費。你可以將它用於個人專案、工作專案或團隊中，唯一要求只是保留許可證宣告。

**它有人維護嗎？**
是的，而且持續維護。該指南會與每次 Claude Code 釋出同步。當前版本是 v2.1.112（2026 年 4 月），相容 Claude Code 2.1+。

**它和官方檔案有什麼不同？**
官方檔案是功能參考手冊；這份指南則是教程，包含圖示、生產級模板和漸進式學習路徑。兩者是互補關係，建議先用本指南學習，再在需要具體細節時查官方檔案。

**完整學完需要多久？**
完整路徑大約需要 11 到 13 小時。但你在 15 分鐘內就能獲得直接收益，只要複製一個 slash command 模板並試用即可。

**我可以搭配 Claude Sonnet / Haiku / Opus 使用嗎？**
可以。所有模板都適用於 Claude Sonnet 4.6、Claude Opus 4.6 和 Claude Haiku 4.5。

**我可以參與貢獻嗎？**
當然可以。請檢視 [CONTRIBUTING.md](CONTRIBUTING.md) 瞭解貢獻規範。我們歡迎新的示例、bug 修復、檔案改進以及社群模板。

**我可以離線閱讀嗎？**
可以。執行 `uv run scripts/build_epub.py`，即可生成包含全部內容和渲染後 Mermaid 圖表的 EPUB 電子書。

---

## 今天就開始掌握 Claude Code

你已經安裝好了 Claude Code。你和 10 倍生產力之間，差的只是知道該怎麼正確使用它。這份指南會提供結構化路徑、視覺化解釋和可直接複製的模板，幫助你真正掌握它。

MIT 許可證，永久免費。克隆它、fork 它、改造成適合你自己的版本。

**[開始學習路線 ->](LEARNING-ROADMAP.md)** | **[瀏覽功能目錄](CATALOG.md)** | **[15 分鐘快速上手](#15-分鐘快速上手)**

---

<details>
<summary>快速導航：全部功能</summary>

| 功能 | 說明 | 資料夾 |
|---------|-------------|--------|
| **功能目錄** | 包含安裝命令的完整參考 | [CATALOG.md](CATALOG.md) |
| **Slash Commands** | 使用者手動觸發的快捷命令 | [01-slash-commands/README.md](01-slash-commands/README.md) |
| **Memory** | 持久上下文 | [02-memory/README.md](02-memory/README.md) |
| **Skills** | 可複用能力 | [03-skills/README.md](03-skills/README.md) |
| **Subagents** | 專門化 AI 助手 | [04-subagents/README.md](04-subagents/README.md) |
| **MCP Protocol** | 訪問外部工具 | [05-mcp/README.md](05-mcp/README.md) |
| **Hooks** | 事件驅動自動化 | [06-hooks/README.md](06-hooks/README.md) |
| **Plugins** | 打包後的功能集合 | [07-plugins/README.md](07-plugins/README.md) |
| **Checkpoints** | 會話快照與回退 | [08-checkpoints/README.md](08-checkpoints/README.md) |
| **Advanced Features** | 規劃、思考、後臺任務 | [09-advanced-features/README.md](09-advanced-features/README.md) |
| **CLI Reference** | 命令、引數與選項 | [10-cli/README.md](10-cli/README.md) |
| **Output Styles** | 切換回應風格 / 自訂工作模式 | [11-output-styles/README.md](11-output-styles/README.md) |
| **Status Line** | 底部狀態列 | [12-status-line/README.md](12-status-line/README.md) |
| **部落格文章** | 真實使用案例 | [Blog Posts](https://medium.com/@luongnv89) |

</details>

<details>
<summary>功能對比</summary>

| 功能 | 呼叫方式 | 永續性 | 最適合 |
|---------|-----------|------------|----------|
| **Slash Commands** | 手動（`/cmd`） | 僅當前會話 | 快速快捷操作 |
| **Memory** | 自動載入 | 跨會話 | 長期記憶與學習 |
| **Skills** | 自動觸發 | 檔案系統級 | 自動化工作流 |
| **Subagents** | 自動委派 | 隔離上下文 | 任務拆分 |
| **MCP Protocol** | 自動查詢 | 實時 | 獲取實時資料 |
| **Hooks** | 事件觸發 | 已配置 | 自動化與校驗 |
| **Plugins** | 一條命令 | 全功能打包 | 完整解決方案 |
| **Checkpoints** | 手動/自動 | 會話級 | 安全試驗 |
| **Planning Mode** | 手動/自動 | 規劃階段 | 複雜實現 |
| **Background Tasks** | 手動 | 任務持續期間 | 長時間執行的操作 |
| **CLI Reference** | 終端命令 | 會話/指令碼級 | 自動化與指令碼開發 |

</details>

<details>
<summary>安裝速查表</summary>

```bash
# Slash Commands
cp 01-slash-commands/*.md .claude/commands/

# Memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Skills
cp -r 03-skills/code-review ~/.claude/skills/

# Subagents
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Plugins
/plugin install pr-review

# Checkpoints（預設自動開啟，可在設定中配置）
# 參見 08-checkpoints/README.md

# Advanced Features（在設定中配置）
# 參見 09-advanced-features/config-examples.json

# CLI Reference（無需安裝）
# 使用示例見 10-cli/README.md
```

</details>

<details>
<summary>01. Slash Commands</summary>

**位置**: [01-slash-commands/README.md](01-slash-commands/README.md)

**是什麼**: 以 Markdown 檔案形式儲存、由使用者手動觸發的快捷命令

**示例**:
- `optimize.md` - 程式碼最佳化分析
- `pr.md` - Pull Request 準備
- `generate-api-docs.md` - API 檔案生成器

**安裝**:
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**使用方法**:
```
/optimize
/pr
/generate-api-docs
```

**瞭解更多**: [Discovering Claude Code Slash Commands](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. Memory</summary>

**位置**: [02-memory/README.md](02-memory/README.md)

**是什麼**: 跨會話持久化的上下文

**示例**:
- `project-CLAUDE.md` - 團隊共享的專案規範
- `directory-api-CLAUDE.md` - 目錄級規則
- `personal-CLAUDE.md` - 個人偏好

**安裝**:
```bash
# 專案記憶
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# 目錄記憶
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# 個人記憶
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**使用方法**: Claude 會自動載入

</details>

<details>
<summary>03. Skills</summary>

**位置**: [03-skills/README.md](03-skills/README.md)

**是什麼**: 可複用、會在合適時自動觸發的能力包，包含說明和指令碼

**示例**:
- `code-review/` - 帶指令碼的完整程式碼審查能力
- `brand-voice/` - 品牌語氣一致性檢查
- `doc-generator/` - API 檔案生成器

**安裝**:
```bash
# 個人 skills
cp -r 03-skills/code-review ~/.claude/skills/

# 專案 skills
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```

**使用方法**: 在相關場景下自動觸發

</details>

<details>
<summary>04. Subagents</summary>

**位置**: [04-subagents/README.md](04-subagents/README.md)

**是什麼**: 擁有隔離上下文和自定義提示詞的專門化 AI 助手

**示例**:
- `code-reviewer.md` - 全面程式碼質量分析
- `test-engineer.md` - 測試策略與覆蓋率
- `documentation-writer.md` - 技術檔案編寫
- `secure-reviewer.md` - 面向安全的審查（只讀）
- `implementation-agent.md` - 完整功能實現

**安裝**:
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**使用方法**: 由主 agent 自動委派

</details>

<details>
<summary>05. MCP Protocol</summary>

**位置**: [05-mcp/README.md](05-mcp/README.md)

**是什麼**: 用於訪問外部工具和 API 的 Model Context Protocol

**示例**:
- `github-mcp.json` - GitHub 整合
- `database-mcp.json` - 資料庫查詢
- `filesystem-mcp.json` - 檔案操作
- `multi-mcp.json` - 多個 MCP servers 配置

**安裝**:
```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 透過 CLI 新增 MCP server
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 或手動新增到專案 .mcp.json（參見 [05-mcp/README.md](05-mcp/README.md) 中的示例）
```

**使用方法**: 配置完成後，Claude 會自動獲得 MCP 工具能力

</details>

<details>
<summary>06. Hooks</summary>

**位置**: [06-hooks/README.md](06-hooks/README.md)

**是什麼**: 基於事件觸發的 shell 命令，會在 Claude Code 事件發生時自動執行

**示例**:
- `format-code.sh` - 寫入前自動格式化程式碼
- `pre-commit.sh` - 提交前執行測試
- `security-scan.sh` - 掃描安全問題
- `log-bash.sh` - 記錄所有 bash 命令
- `validate-prompt.sh` - 校驗使用者提示詞
- `notify-team.sh` - 事件發生時傳送通知

**安裝**:
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

在 `~/.claude/settings.json` 中配置 hooks：
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/format-code.sh"]
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/security-scan.sh"]
    }]
  }
}
```

**使用方法**: hooks 會在事件發生時自動執行

**Hook 型別**（4 類，25 個事件）:
- **工具 Hook**: `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`
- **會話 Hook**: `SessionStart`, `SessionEnd`, `Stop`, `StopFailure`, `SubagentStart`, `SubagentStop`
- **任務 Hook**: `UserPromptSubmit`, `TaskCompleted`, `TaskCreated`, `TeammateIdle`
- **生命週期 Hook**: `ConfigChange`, `CwdChanged`, `FileChanged`, `PreCompact`, `PostCompact`, `WorktreeCreate`, `WorktreeRemove`, `Notification`, `InstructionsLoaded`, `Elicitation`, `ElicitationResult`

</details>

<details>
<summary>07. Plugins</summary>

**位置**: [07-plugins/README.md](07-plugins/README.md)

**是什麼**: 把 commands、agents、MCP 和 hooks 打包在一起的功能集合

**示例**:
- `pr-review/` - 完整的 PR 審查工作流
- `devops-automation/` - 部署與監控
- `documentation/` - 檔案生成

**安裝**:
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**使用方法**: 使用其中打包好的 slash commands 和功能

</details>

<details>
<summary>08. Checkpoints and Rewind</summary>

**位置**: [08-checkpoints/README.md](08-checkpoints/README.md)

**是什麼**: 儲存對話狀態，並回退到之前的時間點，以探索不同方案

**核心概念**:
- **Checkpoint**: 對話狀態快照
- **Rewind**: 回到之前的 checkpoint
- **Branch Point**: 從同一個 checkpoint 分叉出多種方案

**使用方法**:
```
# 每次使用者提示後都會自動建立 checkpoint
# 如需回退，按兩次 Esc，或者使用：
/rewind

# 然後從以下五個選項中選擇：
# 1. 恢復程式碼和對話
# 2. 恢復對話
# 3. 恢復程式碼
# 4. 從這裡開始總結
# 5. 算了
```

**使用場景**:
- 嘗試不同實現方式
- 從錯誤中恢復
- 安全試驗
- 比較不同備選方案
- 對不同設計做 A/B 測試

</details>

<details>
<summary>09. Advanced Features</summary>

**位置**: [09-advanced-features/README.md](09-advanced-features/README.md)

**是什麼**: 面向複雜工作流和自動化的高階能力

**包括**:
- **Planning Mode**：編碼前先建立詳細實現計劃
- **Extended Thinking**：用於複雜問題的深度推理（用 `Alt+T` / `Option+T` 切換）
- **Background Tasks**：執行長時間操作且不阻塞當前會話
- **Permission Modes**：`default`、`acceptEdits`、`plan`、`dontAsk`、`bypassPermissions`
- **Headless Mode**：在 CI/CD 中執行 Claude Code：`claude -p "Run tests and generate report"`
- **Session Management**：`/resume`、`/rename`、`/fork`、`claude -c`、`claude -r`
- **Configuration**：在 `~/.claude/settings.json` 中自定義行為

完整配置請參見 [config-examples.json](09-advanced-features/config-examples.json)。

</details>

<details>
<summary>10. CLI Reference</summary>

**位置**: [10-cli/README.md](10-cli/README.md)

**是什麼**: Claude Code 的完整命令列介面參考

**快速示例**:
```bash
# 互動模式
claude "explain this project"

# 輸出模式（非互動）
claude -p "review this code"

# 處理檔案內容
cat error.log | claude -p "explain this error"

# 為指令碼輸出 JSON
claude -p --output-format json "list functions"

# 恢復會話
claude -r "feature-auth" "continue implementation"
```

**使用場景**: CI/CD 流水線整合、指令碼自動化、批處理、多會話工作流、自定義 agent 配置

</details>

<details>
<summary>示例工作流</summary>

### 完整程式碼審查工作流

```markdown
# 使用：Slash Commands + Subagents + Memory + MCP

使用者：/review-pr

Claude：
1. 載入專案 memory（編碼規範）
2. 透過 GitHub MCP 獲取 PR
3. 委派給 code-reviewer subagent
4. 委派給 test-engineer subagent
5. 彙總發現
6. 提供完整審查結果
```

### 自動化檔案生成

```markdown
# 使用：Skills + Subagents + Memory

使用者："為 auth 模組生成 API 檔案"

Claude：
1. 載入專案 memory（檔案規範）
2. 識別出檔案生成請求
3. 自動觸發 doc-generator skill
4. 委派給 api-documenter subagent
5. 生成帶示例的完整檔案
```

### DevOps 部署

```markdown
# 使用：Plugins + MCP + Hooks

使用者：/deploy production

Claude：
1. 執行部署前 hook（校驗環境）
2. 委派給 deployment-specialist subagent
3. 透過 Kubernetes MCP 執行部署
4. 監控進度
5. 執行部署後 hook（健康檢查）
6. 報告狀態
```

</details>

<details>
<summary>目錄結構</summary>

```
├── 01-slash-commands/
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   └── README.md
├── 02-memory/
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   └── README.md
├── 03-skills/
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── templates/
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   └── templates/
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   └── README.md
├── 04-subagents/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   └── README.md
├── 05-mcp/
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
├── 06-hooks/
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   └── README.md
├── 07-plugins/
│   ├── pr-review/
│   ├── devops-automation/
│   ├── documentation/
│   └── README.md
├── 08-checkpoints/
│   ├── checkpoint-examples.md
│   └── README.md
├── 09-advanced-features/
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
├── 10-cli/
│   └── README.md
└── README.md（本檔案）
```

</details>

<details>
<summary>最佳實踐</summary>

### 應該做的
- 從簡單的 slash commands 開始
- 逐步增加功能
- 用 memory 儲存團隊規範
- 先在本地測試配置
- 為自定義實現寫檔案
- 對專案配置做版本控制
- 和團隊共享 plugins

### 不應該做的
- 不要建立重複功能
- 不要硬編碼憑據
- 不要跳過檔案
- 不要把簡單任務過度複雜化
- 不要忽視安全最佳實踐
- 不要提交敏感資料

</details>

<details>
<summary>故障排查</summary>

### 功能沒有載入
1. 檢查檔案位置和命名
2. 驗證 YAML frontmatter 語法
3. 檢查檔案許可權
4. 檢視 Claude Code 版本相容性

### MCP 連線失敗
1. 檢查環境變數
2. 檢查 MCP server 是否正確安裝
3. 測試憑據
4. 檢查網路連通性

### Subagent 沒有發生委派
1. 檢查工具許可權
2. 確認 agent 描述是否清晰
3. 檢視任務複雜度
4. 單獨測試 agent

</details>

<details>
<summary>測試</summary>

本專案包含完整的自動化測試：

- **單元測試**：基於 pytest 的 Python 測試（Python 3.10、3.11、3.12）
- **程式碼質量**：使用 Ruff 做 lint 和格式化檢查
- **安全**：使用 Bandit 做漏洞掃描
- **型別檢查**：使用 mypy 做靜態型別分析
- **構建驗證**：測試 EPUB 生成功能
- **覆蓋率跟蹤**：整合 Codecov

```bash
# 安裝開發依賴
uv pip install -r requirements-dev.txt

# 執行全部單元測試
pytest scripts/tests/ -v

# 執行帶覆蓋率報告的測試
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 執行程式碼質量檢查
ruff check scripts/
ruff format --check scripts/

# 執行安全掃描
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 執行型別檢查
mypy scripts/ --ignore-missing-imports
```

每次 push 到 `main` / `develop`，以及每次向 `main` 提交 PR 時，測試都會自動執行。詳情見 [TESTING.md](.github/TESTING.md)。

</details>

<details>
<summary>EPUB 生成</summary>

想離線閱讀這份指南？你可以生成 EPUB 電子書：

```bash
uv run scripts/build_epub.py
```

這會生成包含全部內容與 Mermaid 渲染圖表的 `claude-howto-guide.epub`。

更多選項見 [scripts/README.md](scripts/README.md)。

</details>

<details>
<summary>參與貢獻</summary>

發現問題，或者想貢獻一個示例？非常歡迎你的幫助。

**請先閱讀 [CONTRIBUTING.md](CONTRIBUTING.md)，瞭解以下詳細規範：**
- 可貢獻的內容型別（示例、檔案、功能、bug、反饋）
- 如何搭建開發環境
- 目錄結構以及如何新增內容
- 編寫規範與最佳實踐
- Commit 和 PR 流程

**我們的社群標準：**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - 我們如何彼此相處
- [SECURITY.md](SECURITY.md) - 安全政策與漏洞報告方式

### 報告安全問題

如果你發現了安全漏洞，請負責任地報告：

1. **使用 GitHub 私密漏洞報告功能**：https://github.com/luongnv89/claude-howto/security/advisories
2. **或者閱讀** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) 獲取詳細說明
3. **不要**公開提交安全漏洞 issue

快速開始：
1. Fork 並克隆倉庫
2. 建立一個清晰描述用途的分支（`add/feature-name`、`fix/bug`、`docs/improvement`）
3. 按規範進行修改
4. 提交一個說明清楚的 Pull Request

**需要幫助？** 開啟一個 issue 或 discussion，我們會引導你完成整個過程。

</details>

<details>
<summary>更多資源</summary>

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Skills Repository](https://github.com/luongnv89/skills) - 可直接使用的 skills 集合
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny's Claude Code Workflow](https://x.com/bcherny/status/2007179832300581177) - Claude Code 的創造者分享了他系統化的工作流：並行 agents、共享 `CLAUDE.md`、Plan mode、slash commands、subagents，以及用於長時間自主會話的驗證 hooks。

</details>

---

## 參與貢獻

歡迎貢獻內容。如何開始請檢視我們的[貢獻指南](CONTRIBUTING.md)。

## 貢獻者

感謝所有為本專案做出貢獻的人。

| 貢獻者 | PRs |
|-------------|-----|
| [wjhrdy](https://github.com/wjhrdy) | [#1 - add a tool to create an epub](https://github.com/luongnv89/claude-howto/pull/1) |
| [VikalpP](https://github.com/VikalpP) | [#7 - fix(docs): Use tilde fences for nested code blocks in concepts guide](https://github.com/luongnv89/claude-howto/pull/7) |

---

## 許可證

MIT 許可證，詳見 [LICENSE](LICENSE)。你可以自由使用、修改和分發，唯一要求是保留許可證宣告。

---

**最後更新**：2026 年 3 月
**Claude Code 版本**：2.1+
**相容模型**：Claude Sonnet 4.6、Claude Opus 4.6、Claude Haiku 4.5
