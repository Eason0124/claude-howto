# 更新日誌

## v2.3.0 — 2026-04-07

### 功能

- 構建和釋出每種語言的 EPUB 製品（90e9c30）@Thiên Toán
- 將缺失的 pre-tool-check.sh hook 新增到 06-hooks（b511ed1）@JiayuWang
- 在 zh/ 目錄中新增中文翻譯（89e89d4）@Luong NGUYEN
- 新增效能最佳化器子代理和依賴檢查 hook（f53d080）@qk

### Bug 修復

- Windows Git Bash 相容性 + stdin JSON 協議（2cbb10c）@Luong NGUYEN
- 修正 08-checkpoints 中的 autoCheckpoint 配置檔案（749c79f）@JiayuWang
- 用佔位符替代 SVG 影象嵌入（1b16709）@Thiên Toán
- 修復記憶體 README 中的巢狀程式碼圍欄渲染（ce24423）@Zhaoshan Duan
- 應用由 squash merge 丟棄的審查修復（34259ca）@Luong NGUYEN
- 使 hook 指令碼與 Windows Git Bash 相容並使用 stdin JSON 協議（107153d）@binyu li

### 檔案

- 與 Claude Code 最新檔案（2026 年 4 月）同步所有教程（72d3b01）@Luong NGUYEN
- 在語言切換器中新增中文語言連結（6cbaa4d）@Luong NGUYEN
- 在英文和越南文之間新增語言切換器（100c45e）@Luong NGUYEN
- 新增 GitHub #1 Trending 徽章（0ca8c37）@Luong NGUYEN
- 為上下文區域監控引入 cc-context-stats（d41b335）@Luong NGUYEN
- 引入 luongnv89/skills 集合和 luongnv89/asm skill 管理器（7e3c0b6）@Luong NGUYEN
- 更新 README 統計資料以反映當前 GitHub 指標（5900+ stars, 690+ forks）（5001525）@Luong NGUYEN
- 更新 README 統計資料以反映當前 GitHub 指標（3900+ stars, 460+ forks）（9cb92d6）@Luong NGUYEN

### 重構

- 使用本地 mmdc 渲染替代 Kroki HTTP 依賴（e76bbe4）@Luong NGUYEN
- 將質量檢查轉移到 pre-commit，CI 作為第二道防線（6d1e0ae）@Luong NGUYEN
- 縮小自動模式許可權基線（2790fb2）@Luong NGUYEN
- 用一次性許可權設定指令碼替換自動適配 hook（995a5d6）@Luong NGUYEN

### 其他

- 左移質量關卡 — 向 pre-commit 新增 mypy，修復 CI 失敗（699fb39）@Luong NGUYEN
- 新增越南語（Tiếng Việt）本地化（a70777e）@Thiên Toán

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### 檔案

- 與 Claude Code v2.1.84（f78c094）同步更新所有教程和參考檔案 @luongnv89
  - 將 slash commands 更新為 55+ 個內建命令 + 5 個 bundled skills，並標記 3 個已棄用項
  - 將 hook 事件從 18 個擴充套件到 25 個，新增 `agent` hook 型別（現在共 4 類）
  - 在高階功能中加入自動模式（Auto Mode）、通道（Channels）、語音輸入（Voice Dictation）
  - 為 skill frontmatter 增加 `effort`、`shell` 欄位；為 agent 增加 `initialPrompt`、`disallowedTools` 欄位
  - 增加 WebSocket MCP transport、elicitation、2KB 工具上限
  - 增加 plugin 的 LSP 支援、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有參考檔案（CATALOG、QUICK_REFERENCE、LEARNING-ROADMAP、INDEX）
- 將 README 重寫為落地頁結構化指南（32a0776）@luongnv89

### Bug 修復

- 為透過 CI 詞典檢查，補充缺失的 cSpell 詞條和 README 章節（93f9d51）@luongnv89
- 在 cSpell 詞典中加入 `Sandboxing`（b80ce6f）@luongnv89

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug 修復

- 移除導致 CI 連結檢查失敗的失效 marketplace 連結（3fdf0d6）@luongnv89
- 將 `sandboxed` 和 `pycache` 加入 cSpell 詞典（dc64618）@luongnv89

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 功能

- 新增自適應學習路徑，包含自我評估和 lesson quiz skills（1ef46cd）@luongnv89
  - `/self-assessment` - 覆蓋 10 個功能領域的互動式能力測驗，並生成個性化學習路徑
  - `/lesson-quiz [lesson]` - 每課知識檢查，包含 8-10 道針對性問題

### Bug 修復

- 更新失效 URL、棄用項和過時引用（8fe4520）@luongnv89
- 修復 resources 和 self-assessment skill 中的損壞連結（7a05863）@luongnv89
- 在 concepts guide 中為巢狀程式碼塊使用 tilde fences（5f82719）@VikalpP
- 為 cSpell 詞典補充缺失詞彙（8df7572）@luongnv89

### 檔案

- Phase 5 QA - 修復各檔案中的一致性、URL 和術語問題（00bbe4c）@luongnv89
- 完成 Phase 3-4 - 補充新功能覆蓋和參考檔案更新（132de29）@luongnv89
- 在 MCP 上下文膨脹章節加入 MCPorter runtime（ef52705）@luongnv89
- 為 6 份指南補充缺失命令、功能和設定（4bc8f15）@luongnv89
- 基於倉庫現有規範補充 style guide（84141d0）@luongnv89
- 在指南對比表中加入自我評估行（8fe0c96）@luongnv89
- 將 VikalpP 加入貢獻者名單，記錄 PR #7（d5b4350）@luongnv89
- 在 README 和 roadmap 中加入 self-assessment 與 lesson-quiz skill 參考（d5a6106）@luongnv89

### 新貢獻者

- @VikalpP 完成了他們的首次貢獻，見 #7

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 功能

- 與 Claude Code 2026 年 2 月功能同步更新全部檔案（487c96d）
  - 更新所有 10 個教程目錄和 7 份參考檔案中的 26 個檔案
  - 補充 **Auto Memory** 檔案 - 每個專案的持久學習能力
  - 補充 **Remote Control**、**Web Sessions** 和 **Desktop App** 檔案
  - 補充 **Agent Teams** 檔案（實驗性多 agent 協作）
  - 補充 **MCP OAuth 2.0**、**Tool Search** 和 **Claude.ai Connectors** 檔案
  - 補充 subagents 的 **Persistent Memory** 和 **Worktree Isolation** 檔案
  - 補充 **Background Subagents**、**Task List**、**Prompt Suggestions** 檔案
  - 補充 **Sandboxing** 和 **Managed Settings**（Enterprise）檔案
  - 補充 **HTTP Hooks** 和 7 個新 hook 事件的檔案
  - 補充 **Plugin Settings**、**LSP Servers** 和 marketplace 更新檔案
  - 補充 Checkpoint 的 **Summarize from Checkpoint** 回退選項檔案
  - 記錄 17 個新的 slash commands（`/fork`、`/desktop`、`/teleport`、`/tasks`、`/fast` 等）
  - 記錄新的 CLI flags（`--worktree`、`--from-pr`、`--remote`、`--teleport`、`--teammate-mode` 等）
  - 記錄 auto memory、effort 等級、agent teams 等新的環境變數

### 設計

- 將 logo 重設計為簡潔調色的 compass-bracket 標誌（20779db）

### Bug 修復 / 修正

- 更新模型名稱：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修正 permission mode 名稱：用真實的 `default` / `acceptEdits` / `plan` / `dontAsk` / `bypassPermissions` 替代虛構的 “Unrestricted/Confirm/Read-only”
- 修正 hook 事件：移除虛構的 `PreCommit` / `PostCommit` / `PrePush`，加入真實事件（`SubagentStart`、`WorktreeCreate`、`ConfigChange` 等）
- 修正 CLI 語法：用 `claude -p`（print mode）替代虛構的 `claude-code --headless`
- 修正 checkpoint 命令：用真實的 `Esc+Esc` / `/rewind` 介面替代虛構的 `/checkpoint save/list/rewind/diff`
- 修正 session 管理：用真實的 `/resume` / `/rename` / `/fork` 替代虛構的 `/session list/new/switch/save`
- 修正 plugin manifest 格式：從 `plugin.yaml` 遷移到 `.claude-plugin/plugin.json`
- 修正 MCP 配置路徑：`~/.claude/mcp.json` → `.mcp.json`（專案）/ `~/.claude.json`（使用者）
- 修正檔案 URL：`docs.claude.com` → `docs.anthropic.com`；移除虛構的 `plugins.claude.com`
- 移除多個檔案中虛構的配置欄位
- 將所有 “Last Updated” 日期更新到 2026 年 2 月

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
