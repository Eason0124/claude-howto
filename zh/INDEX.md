<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 示例 - 完整索引

這份檔案是倉庫裡所有示例檔案的完整索引，按功能型別整理，方便你快速定位需要的內容。

## 概覽統計

- **總檔案數**：100+ 檔案
- **分類數**：10 個功能分類
- **外掛**：3 個完整外掛
- **Skills**：6 個完整 skills
- **Hooks**：8 個示例 hook
- **可直接使用**：所有示例都可直接拿來用

---

## 01. Slash Commands（10 個檔案）

用於常見工作流的使用者觸發快捷命令。

| 檔案 | 說明 | 使用場景 |
|------|------|----------|
| `optimize.md` | 程式碼最佳化分析器 | 查詢效能問題 |
| `pr.md` | Pull Request 準備 | PR 工作流自動化 |
| `generate-api-docs.md` | API 檔案生成器 | 生成 API 檔案 |
| `commit.md` | 提交資訊助手 | 統一提交風格 |
| `setup-ci-cd.md` | CI/CD 流水線設定 | DevOps 自動化 |
| `push-all.md` | 推送所有變更 | 快速推送工作流 |
| `unit-test-expand.md` | 擴充套件單元測試覆蓋率 | 測試自動化 |
| `doc-refactor.md` | 檔案重構 | 檔案最佳化 |
| `pr-slash-command.png` | 截圖示例 | 視覺參考 |
| `README.md` | 檔案 | 安裝與使用指南 |

**安裝路徑**：`.claude/commands/`

**使用方式**：`/optimize`、`/pr`、`/generate-api-docs`、`/commit`、`/setup-ci-cd`、`/push-all`、`/unit-test-expand`、`/doc-refactor`

---

## 02. Memory（6 個檔案）

持久化上下文和專案規範。

| 檔案 | 說明 | 範圍 | 位置 |
|------|------|------|------|
| `project-CLAUDE.md` | 團隊專案規範 | 整個專案 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API 專用規則 | 目錄級 | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` | 個人偏好 | 使用者級 | `~/.claude/CLAUDE.md` |
| `memory-saved.png` | 截圖：記憶已儲存 | - | 視覺參考 |
| `memory-ask-claude.png` | 截圖：詢問 Claude | - | 視覺參考 |
| `README.md` | 檔案 | - | 參考說明 |

**安裝方式**：複製到對應位置

**使用方式**：Claude 會自動載入

---

## 03. Skills（28 個檔案）

帶指令碼和模板的自動觸發能力。

### 程式碼審查 Skill（5 個檔案）

```text
code-review/
├── SKILL.md                     # Skill 定義
├── scripts/
│   ├── analyze-metrics.py          # 程式碼指標分析器
│   └── compare-complexity.py       # 複雜度對比
└── templates/
    ├── review-checklist.md      # 審查清單
    └── finding-template.md      # 問題記錄模板
```

**用途**：結合安全、效能和質量分析的完整程式碼審查

**自動觸發**：在審查程式碼時

### 品牌語氣 Skill（4 個檔案）

```text
brand-voice/
├── SKILL.md                     # Skill 定義
├── templates/
│   ├── email-template.txt          # 郵件格式
│   └── social-post-template.txt    # 社交媒體格式
└── tone-examples.md             # 示例訊息
```

**用途**：確保對外溝通中的品牌語氣一致

**自動觸發**：在編寫營銷文案時

### 檔案生成 Skill（2 個檔案）

```text
doc-generator/
├── SKILL.md                     # Skill 定義
└── generate-docs.py                 # Python 檔案提取器
```

**用途**：從原始碼生成完整的 API 檔案

**自動觸發**：在建立或更新 API 檔案時

### 重構 Skill（5 個檔案）

```text
refactor/
├── SKILL.md                     # Skill 定義
├── scripts/
│   ├── analyze-complexity.py       # 複雜度分析器
│   └── detect-smells.py            # 程式碼異味檢測器
├── references/
│   ├── code-smells.md           # 程式碼異味目錄
│   └── refactoring-catalog.md   # 重構模式目錄
└── templates/
    └── refactoring-plan.md      # 重構計劃模板
```

**用途**：結合複雜度分析進行系統化程式碼重構

**自動觸發**：在重構程式碼時

### Claude MD Skill（1 個檔案）

```text
claude-md/
└── SKILL.md                     # Skill 定義
```

**用途**：管理和最佳化 `CLAUDE.md` 檔案

### 部落格草稿 Skill（3 個檔案）

```text
blog-draft/
├── SKILL.md                     # Skill 定義
└── templates/
    ├── draft-template.md        # 部落格草稿模板
    └── outline-template.md      # 部落格大綱模板
```

**用途**：生成結構統一的部落格草稿

**補充**：`README.md` - Skills 總覽與使用指南

**安裝路徑**：`~/.claude/skills/` 或 `.claude/skills/`

---

## 04. Subagents（9 個檔案）

帶自定義能力的專門化 AI 助手。

| 檔案 | 說明 | 工具 | 使用場景 |
|------|------|------|----------|
| `code-reviewer.md` | 程式碼質量分析 | read, grep, diff, lint_runner | 完整審查 |
| `test-engineer.md` | 測試覆蓋分析 | read, write, bash, grep | 測試自動化 |
| `documentation-writer.md` | 檔案建立 | read, write, grep | 檔案生成 |
| `secure-reviewer.md` | 安全審查（只讀） | read, grep | 安全審計 |
| `implementation-agent.md` | 完整實現 | read, write, bash, grep, edit, glob | 功能開發 |
| `debugger.md` | 除錯專家 | read, bash, grep | Bug 排查 |
| `data-scientist.md` | 資料分析專家 | read, write, bash | 資料工作流 |
| `clean-code-reviewer.md` | 程式碼整潔性規範 | read, grep | 程式碼質量 |
| `README.md` | 檔案 | - | 安裝與使用指南 |

**安裝路徑**：`.claude/agents/`

**使用方式**：由主 agent 自動委派

---

## 05. MCP Protocol（5 個檔案）

外部工具和 API 整合。

| 檔案 | 說明 | 整合物件 | 使用場景 |
|------|------|----------|----------|
| `github-mcp.json` | GitHub 整合 | GitHub API | PR / issue 管理 |
| `database-mcp.json` | 資料庫查詢 | PostgreSQL / MySQL | 實時資料查詢 |
| `filesystem-mcp.json` | 檔案操作 | 本地檔案系統 | 檔案管理 |
| `multi-mcp.json` | 多伺服器配置 | GitHub + DB + Slack | 完整整合 |
| `README.md` | 檔案 | - | 安裝與使用指南 |

**安裝路徑**：專案級 `.mcp.json` 或使用者級 `~/.claude.json`

**使用方式**：例如 `/mcp__github__list_prs`

---

## 06. Hooks（9 個檔案）

事件驅動的自動化指令碼，會自動執行。

| 檔案 | 說明 | 事件 | 使用場景 |
|------|------|------|----------|
| `format-code.sh` | 自動格式化程式碼 | `PreToolUse:Write` | 程式碼格式化 |
| `pre-commit.sh` | 提交前執行測試 | `PreToolUse:Bash` | 測試自動化 |
| `security-scan.sh` | 安全掃描 | `PostToolUse:Write` | 安全檢查 |
| `log-bash.sh` | 記錄 bash 命令 | `PostToolUse:Bash` | 命令日誌 |
| `validate-prompt.sh` | 驗證提示詞 | `PreToolUse` | 輸入校驗 |
| `notify-team.sh` | 傳送通知 | `Notification` | 團隊通知 |
| `context-tracker.py` | 跟蹤上下文視窗使用量 | `PostToolUse` | 上下文監控 |
| `context-tracker-tiktoken.py` | 基於 token 的上下文跟蹤 | `PostToolUse` | 精確 token 統計 |
| `README.md` | 檔案 | - | 安裝與使用指南 |

**安裝路徑**：在 `~/.claude/settings.json` 中配置

**使用方式**：在設定中配置後自動執行

**Hook 型別**（4 類，25 個事件）：
- 工具 Hook：`PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionRequest`
- 會話 Hook：`SessionStart`、`SessionEnd`、`Stop`、`StopFailure`、`SubagentStart`、`SubagentStop`
- 任務 Hook：`UserPromptSubmit`、`TaskCompleted`、`TaskCreated`、`TeammateIdle`
- 生命週期 Hook：`ConfigChange`、`CwdChanged`、`FileChanged`、`PreCompact`、`PostCompact`、`WorktreeCreate`、`WorktreeRemove`、`Notification`、`InstructionsLoaded`、`Elicitation`、`ElicitationResult`

---

## 07. Plugins（3 個完整外掛，40 個檔案）

打包好的功能集合。

### PR Review 外掛（10 個檔案）

```text
pr-review/
├── .claude-plugin/
│   └── plugin.json                  # 外掛清單
├── commands/
│   ├── review-pr.md              # 綜合審查
│   ├── check-security.md         # 安全檢查
│   └── check-tests.md            # 測試覆蓋檢查
├── agents/
│   ├── security-reviewer.md      # 安全專家
│   ├── test-checker.md           # 測試專家
│   └── performance-analyzer.md   # 效能專家
├── mcp/
│   └── github-config.json           # GitHub 整合
├── hooks/
│   └── pre-review.js                # 審查前校驗
└── README.md                     # 外掛檔案
```

**功能**：安全分析、測試覆蓋、效能影響

**命令**：`/review-pr`、`/check-security`、`/check-tests`

**安裝**：`/plugin install pr-review`

### DevOps Automation 外掛（15 個檔案）

```text
devops-automation/
├── .claude-plugin/
│   └── plugin.json                  # 外掛清單
├── commands/
│   ├── deploy.md                 # 部署
│   ├── rollback.md               # 回滾
│   ├── status.md                 # 系統狀態
│   └── incident.md               # 事件響應
├── agents/
│   ├── deployment-specialist.md  # 部署專家
│   ├── incident-commander.md     # 事件協調
│   └── alert-analyzer.md         # 告警分析
├── mcp/
│   └── kubernetes-config.json       # Kubernetes 整合
├── hooks/
│   ├── pre-deploy.js                # 部署前檢查
│   └── post-deploy.js               # 部署後任務
├── scripts/
│   ├── deploy.sh                    # 部署自動化
│   ├── rollback.sh                  # 回滾自動化
│   └── health-check.sh              # 健康檢查
└── README.md                     # 外掛檔案
```

**功能**：Kubernetes 部署、回滾、監控、事件響應

**命令**：`/deploy`、`/rollback`、`/status`、`/incident`

**安裝**：`/plugin install devops-automation`

### Documentation 外掛（14 個檔案）

```text
documentation/
├── .claude-plugin/
│   └── plugin.json                  # 外掛清單
├── commands/
│   ├── generate-api-docs.md      # API 檔案生成
│   ├── generate-readme.md        # README 建立
│   ├── sync-docs.md              # 檔案同步
│   └── validate-docs.md          # 檔案校驗
├── agents/
│   ├── api-documenter.md         # API 檔案專家
│   ├── code-commentator.md       # 程式碼註釋專家
│   └── example-generator.md      # 示例生成器
├── mcp/
│   └── github-docs-config.json      # GitHub 整合
├── templates/
│   ├── api-endpoint.md           # API 端點模板
│   ├── function-docs.md          # 函式檔案模板
│   └── adr-template.md           # ADR 模板
└── README.md                     # 外掛檔案
```

**功能**：API 檔案、README 生成、檔案同步、檔案校驗

**命令**：`/generate-api-docs`、`/generate-readme`、`/sync-docs`、`/validate-docs`

**安裝**：`/plugin install documentation`

**補充**：`README.md` - 外掛總覽與使用指南

---

## 08. Checkpoints and Rewind（2 個檔案）

儲存會話狀態，並探索替代方案。

| 檔案 | 說明 | 內容 |
|------|------|------|
| `README.md` | 檔案 | 完整的 checkpoint 指南 |
| `checkpoint-examples.md` | 真實示例 | 資料庫遷移、效能最佳化、UI 迭代、除錯 |

**核心概念**：
- **Checkpoint**：會話狀態快照
- **Rewind**：回到之前的 checkpoint
- **Branch Point**：探索多種方案

**使用方式**：

```bash
# 每次使用者提示後都會自動建立 checkpoint
# 如需回退，按兩次 Esc，或使用：
/rewind
# 然後選擇：恢復程式碼和對話、恢復對話、
# 恢復程式碼、從這裡開始總結，或算了
```

**使用場景**：
- 嘗試不同實現方式
- 從錯誤中恢復
- 安全試驗
- 比較不同方案
- A/B 測試

---

## 09. Advanced Features（3 個檔案）

複雜工作流下的高階能力。

| 檔案 | 說明 | 功能 |
|------|------|------|
| `README.md` | 完整指南 | 全部高階功能說明 |
| `config-examples.json` | 配置示例 | 10+ 個場景化配置 |
| `planning-mode-examples.md` | 規劃示例 | REST API、資料庫遷移、重構 |

**擴充套件功能**：
- Scheduled Tasks：使用 `/loop` 和 cron 工具的週期任務
- Chrome Integration：透過無頭 Chromium 做瀏覽器自動化
- Remote Control（擴充套件版）：連線方式、安全性、對比表
- Keyboard Customization：自定義快捷鍵、組合鍵支援、上下文
- Desktop App（擴充套件版）：聯結器、`launch.json`、企業功能

### 已覆蓋的高階功能

#### Planning Mode
- 編寫詳細實現計劃
- 時間預估與風險評估
- 系統化拆解任務

#### Extended Thinking
- 面向複雜問題的深度推理
- 架構決策分析
- 權衡取捨評估

#### Background Tasks
- 長時間執行且不阻塞當前會話
- 並行開發工作流
- 任務管理與監控

#### Permission Modes
- **default**：風險操作需要審批
- **acceptEdits**：自動接受檔案編輯，其他操作仍需審批
- **plan**：只讀分析，不做修改
- **auto**：自動批准安全操作，風險操作仍提示
- **dontAsk**：除危險操作外全部接受
- **bypassPermissions**：全部接受（需要 `--dangerously-skip-permissions`）

#### Headless Mode（`claude -p`）
- CI/CD 整合
- 自動化任務執行
- 批處理

#### Session Management
- 多會話管理
- 會話切換與儲存
- 會話持久化

#### 互動功能
- 鍵盤快捷鍵
- 命令歷史
- Tab 補全
- 多行輸入

#### 配置
- 完整設定管理
- 環境相關配置
- 按專案定製

#### Scheduled Tasks
- 使用 `/loop` 建立週期任務
- Cron 工具：`CronCreate`、`CronList`、`CronDelete`
- 自動化的重複工作流

#### Chrome Integration
- 透過無頭 Chromium 進行瀏覽器自動化
- 頁面測試與抓取能力
- 頁面互動與資料提取

#### Remote Control（擴充套件版）
- 連線方式與協議
- 安全注意事項與最佳實踐
- 遠端訪問方案對比表

#### Keyboard Customization
- 自定義按鍵繫結配置
- 支援多鍵組合快捷鍵
- 基於上下文的按鍵啟用

#### Desktop App（擴充套件版）
- 用於 IDE 整合的聯結器
- `launch.json` 配置
- 企業功能與部署

---

## 10. CLI Usage(3 個檔案)

命令列介面使用方式與參考。

| 檔案 | 說明 | 內容 |
|------|------|------|
| `README.md` | CLI 指南 | 引數、選項與使用模式 |
| `flags-reference.md` | **完整 flag 對照表** | 官方 cli-reference 整理 + 已移除 flag 清單 |
| `print-mode.md` | `-p` 深度用法 | CI / script / stream-json 聯動 |

**核心 CLI 功能**:
- `claude` - 啟動互動式會話
- `claude -p "prompt"` - 無頭/非互動模式
- `claude web` - 開啟 Web 會話
- `claude --model` - 選擇模型(Sonnet 4.6、Opus 4.7)
- `claude --permission-mode` - 設定許可權模式(包含 `auto` — 舊 `--enable-auto-mode` 已移除)
- `claude --remote` - 透過 WebSocket 啟用遠端控制

---

## 11. Output Styles(1 個檔案)

改變 Claude 回應的**風格**(Default / Explanatory / Learning)或自訂工作模式。

| 檔案 | 說明 |
|------|------|
| `README.md` | 三種預設 style + 自訂 frontmatter + 與 system prompt 覆寫的差異 |

---

## 12. Status Line(1 個檔案)

底部狀態列,用 shell script 顯示 context / cost / git / 當前模型 / 自訂指標。

| 檔案 | 說明 |
|------|------|
| `README.md` | stdin/stdout 協議 + 3 個範例腳本 + 與 Hook 的差異 |

---

## 13. UI / Keyboard(1 個檔案)

主題、Vim 模式、鍵盤快捷鍵的彙整(v2.1.109 ~ v2.1.119 變更)。

| 檔案 | 說明 |
|------|------|
| `README.md` | 自訂主題 JSON、Vim visual mode、Ctrl+U/O、Shift+↑↓、Thinking spinner |

---

## 檔案檔案(19 個主要檔案)

| 檔案 | 位置 | 說明 |
|------|------|------|
| `README.md` | `/` | 主總覽 |
| `INDEX.md` | `/` | 本完整索引 |
| `QUICK_REFERENCE.md` | `/` | 速查卡片 |
| `README.md` | `/01-slash-commands/` | Slash Commands 指南 |
| `README.md` | `/02-memory/` | Memory 指南 |
| `README.md` | `/03-skills/` | Skills 指南 |
| `README.md` | `/04-subagents/` | Subagents 指南 |
| `README.md` | `/05-mcp/` | MCP 指南 |
| `README.md` | `/06-hooks/` | Hooks 指南 |
| `README.md` | `/07-plugins/` | Plugins 指南 |
| `README.md` | `/08-checkpoints/` | Checkpoints 指南 |
| `README.md` | `/09-advanced-features/` | Advanced Features 指南 |
| `README.md` | `/10-cli/` | CLI 指南 |
| `flags-reference.md` | `/10-cli/` | CLI 完整 flag 對照表(權威) |
| `print-mode.md` | `/10-cli/` | `-p` 深度用法 |
| `config-and-env-reference.md` | `/10-cli/` | Settings、環境變數與 `/config`(v2.1.119) |
| `events-reference.md` | `/06-hooks/` | 27 個 hook 事件完整參考 |
| `transports.md` | `/05-mcp/` | MCP transport 參考 |
| `oauth-setup.md` | `/05-mcp/` | MCP OAuth 設定 |
| `managed-mcp.md` | `/05-mcp/` | 企業受管 MCP |
| `frontmatter-reference.md` | `/04-subagents/` | Subagent 16 欄位完整表 |
| `builtin-agents.md` | `/04-subagents/` | 內建 subagent 清單 |
| `skills-reference.md` | `/03-skills/` | Skills 完整欄位 / 變數 / bundled |
| `README.md` | `/11-output-styles/` | Output Styles 指南 |
| `README.md` | `/12-status-line/` | Status Line 指南 |
| `README.md` | `/13-ui-and-keyboard/` | UI / 主題 / Vim / 鍵盤(v2.1.111+) |

---

## 完整檔案樹

```text
claude-howto/
├── README.md                                  # 主總覽
├── INDEX.md                                   # 本檔案
├── QUICK_REFERENCE.md                         # 速查卡片
├── claude_concepts_guide.md                   # 原始指南
│
├── 01-slash-commands/                            # Slash Commands
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   ├── commit.md
│   ├── setup-ci-cd.md
│   ├── push-all.md
│   ├── unit-test-expand.md
│   ├── doc-refactor.md
│   ├── pr-slash-command.png
│   └── README.md
│
├── 02-memory/                                    # Memory
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md
│
├── 03-skills/                                    # Skills
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-metrics.py
│   │   │   └── compare-complexity.py
│   │   └── templates/
│   │       ├── review-checklist.md
│   │       └── finding-template.md
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   │   ├── email-template.txt
│   │   │   └── social-post-template.txt
│   │   └── tone-examples.md
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   ├── refactor/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-complexity.py
│   │   │   └── detect-smells.py
│   │   ├── references/
│   │   │   ├── code-smells.md
│   │   │   └── refactoring-catalog.md
│   │   └── templates/
│   │       └── refactoring-plan.md
│   ├── claude-md/
│   │   └── SKILL.md
│   ├── blog-draft/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── draft-template.md
│   │       └── outline-template.md
│   └── README.md
│
├── 04-subagents/                                 # Subagents
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   ├── debugger.md
│   ├── data-scientist.md
│   ├── clean-code-reviewer.md
│   └── README.md
│
├── 05-mcp/                                       # MCP Protocol
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
│
├── 06-hooks/                                     # Hooks
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   ├── context-tracker.py
│   ├── context-tracker-tiktoken.py
│   └── README.md
│
├── 07-plugins/                                   # Plugins
│   ├── pr-review/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── review-pr.md
│   │   │   ├── check-security.md
│   │   │   └── check-tests.md
│   │   ├── agents/
│   │   │   ├── security-reviewer.md
│   │   │   ├── test-checker.md
│   │   │   └── performance-analyzer.md
│   │   ├── mcp/
│   │   │   └── github-config.json
│   │   ├── hooks/
│   │   │   └── pre-review.js
│   │   └── README.md
│   ├── devops-automation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── deploy.md
│   │   │   ├── rollback.md
│   │   │   ├── status.md
│   │   │   └── incident.md
│   │   ├── agents/
│   │   │   ├── deployment-specialist.md
│   │   │   ├── incident-commander.md
│   │   │   └── alert-analyzer.md
│   │   ├── mcp/
│   │   │   └── kubernetes-config.json
│   │   ├── hooks/
│   │   │   ├── pre-deploy.js
│   │   │   └── post-deploy.js
│   │   ├── scripts/
│   │   │   ├── deploy.sh
│   │   │   ├── rollback.sh
│   │   │   └── health-check.sh
│   │   └── README.md
│   ├── documentation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── generate-api-docs.md
│   │   │   ├── generate-readme.md
│   │   │   ├── sync-docs.md
│   │   │   └── validate-docs.md
│   │   ├── agents/
│   │   │   ├── api-documenter.md
│   │   │   ├── code-commentator.md
│   │   │   └── example-generator.md
│   │   ├── mcp/
│   │   │   └── github-docs-config.json
│   │   ├── templates/
│   │   │   ├── api-endpoint.md
│   │   │   ├── function-docs.md
│   │   │   └── adr-template.md
│   │   └── README.md
│   └── README.md
│
├── 08-checkpoints/                               # Checkpoints
│   ├── checkpoint-examples.md
│   └── README.md
│
├── 09-advanced-features/                         # Advanced Features
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
│
└── 10-cli/                                       # CLI 使用
    └── README.md
```

---

## 按使用場景快速開始

### 程式碼質量與審查

```bash
# 安裝 slash command
cp 01-slash-commands/optimize.md .claude/commands/

# 安裝 subagent
cp 04-subagents/code-reviewer.md .claude/agents/

# 安裝 skill
cp -r 03-skills/code-review ~/.claude/skills/

# 或直接安裝完整外掛
/plugin install pr-review
```

### DevOps 與部署

```bash
# 安裝外掛（包含全部功能）
/plugin install devops-automation
```

### 檔案

```bash
# 安裝 slash command
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# 安裝 subagent
cp 04-subagents/documentation-writer.md .claude/agents/

# 安裝 skill
cp -r 03-skills/doc-generator ~/.claude/skills/

# 或直接安裝完整外掛
/plugin install documentation
```

### 團隊規範

```bash
# 設定專案 memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 根據團隊規範調整內容
```

### 外部整合

```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 安裝 MCP 配置（專案級）
cp 05-mcp/multi-mcp.json .mcp.json
```

### 自動化與校驗

```bash
# 安裝 hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 在 settings 中配置 hooks（~/.claude/settings.json）
# 參見 06-hooks/README.md
```

### 安全實驗

```bash
# 每次使用者提示後都會自動建立 checkpoint
# 回退：按 Esc 兩次或使用 /rewind
# 然後選擇要從回退選單中恢復的內容

# 參見 08-checkpoints/README.md 的示例
```

### 高階工作流

```bash
# 配置高階功能
# 參見 09-advanced-features/config-examples.json

# 使用 planning mode
/plan 實現功能 X

# 使用許可權模式
claude --permission-mode plan          # 用於程式碼審查（只讀）
claude --permission-mode acceptEdits   # 自動接受編輯
claude --permission-mode auto          # 自動批准安全操作

# 以 headless 模式執行 CI/CD
claude -p "Run tests and report results"

# 執行後臺任務
在後臺執行測試

# 參見 09-advanced-features/README.md 獲取完整指南
```

---

## 功能覆蓋矩陣

| 分類 | Commands | Agents | MCP | Hooks | Scripts | Templates | Docs | Images | 總計 |
|------|----------|--------|-----|-------|---------|-----------|------|--------|------|
| **01 Slash Commands** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 Memory** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 Skills** | - | - | - | - | 5 | 9 | 1 | - | **28** |
| **04 Subagents** | - | 8 | - | - | - | - | 1 | - | **9** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 Hooks** | - | - | - | 8 | - | - | 1 | - | **9** |
| **07 Plugins** | 11 | 9 | 3 | 3 | 3 | 3 | 4 | - | **40** |
| **08 Checkpoints** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 Advanced** | - | - | - | - | - | - | 1 | 2 | **3** |
| **10 CLI** | - | - | - | - | - | - | 1 | - | **1** |

---

## 學習路徑

### 入門（第 1 周）

1. ✅ 閱讀 `README.md`
2. ✅ 安裝 1 到 2 個 slash command
3. ✅ 建立專案 memory 檔案
4. ✅ 試用基礎命令

### 中級（第 2-3 周）

1. ✅ 搭建 GitHub MCP
2. ✅ 安裝一個 subagent
3. ✅ 嘗試委派任務
4. ✅ 安裝一個 skill

### 高階（第 4 周及以後）

1. ✅ 安裝完整外掛
2. ✅ 建立自定義 slash command
3. ✅ 建立自定義 subagent
4. ✅ 建立自定義 skill
5. ✅ 構建自己的外掛

### 專家（第 5 周及以後）

1. ✅ 配置 hooks 做自動化
2. ✅ 用 checkpoints 做實驗
3. ✅ 配置 planning mode
4. ✅ 充分利用許可權模式
5. ✅ 為 CI/CD 配置 headless mode
6. ✅ 熟練掌握會話管理

---

## 按關鍵詞查詢

### 效能

- `01-slash-commands/optimize.md` - 效能分析
- `04-subagents/code-reviewer.md` - 效能審查
- `03-skills/code-review/` - 效能指標
- `07-plugins/pr-review/agents/performance-analyzer.md` - 效能專家

### 安全

- `04-subagents/secure-reviewer.md` - 安全審查
- `03-skills/code-review/` - 安全分析
- `07-plugins/pr-review/` - 安全檢查

### 測試

- `04-subagents/test-engineer.md` - 測試工程師
- `07-plugins/pr-review/commands/check-tests.md` - 測試覆蓋率

### 檔案

- `01-slash-commands/generate-api-docs.md` - API 檔案命令
- `04-subagents/documentation-writer.md` - 檔案編寫 agent
- `03-skills/doc-generator/` - 檔案生成 skill
- `07-plugins/documentation/` - 完整檔案外掛

### 部署

- `07-plugins/devops-automation/` - 完整 DevOps 方案

### 自動化

- `06-hooks/` - 事件驅動自動化
- `06-hooks/pre-commit.sh` - 提交前自動化
- `06-hooks/format-code.sh` - 自動格式化
- `09-advanced-features/` - headless 模式用於 CI/CD

### 校驗

- `06-hooks/security-scan.sh` - 安全校驗
- `06-hooks/validate-prompt.sh` - 提示詞校驗

### 實驗

- `08-checkpoints/` - 使用 rewind 做安全實驗
- `08-checkpoints/checkpoint-examples.md` - 真實示例

### 規劃

- `09-advanced-features/planning-mode-examples.md` - 規劃模式示例
- `09-advanced-features/README.md` - 深度思考

### 配置

- `09-advanced-features/config-examples.json` - 配置示例

---

## 說明

- 所有示例都可直接使用
- 按需修改以適配你的專案
- 示例遵循 Claude Code 最佳實踐
- 每個分類都有自己的 README，包含詳細說明
- 指令碼包含正確的錯誤處理
- 模板都可自定義

---

## 參與貢獻

如果你想增加更多示例，可以按照下面的結構：
1. 建立合適的子目錄
2. 包含帶使用說明的 README.md
3. 遵循命名規範
4. 充分測試
5. 更新這個索引

---

**最後更新**：2026 年 4 月 27 日(v2.1.119 同步)
**總示例數**：100+ 檔案
**分類數**：10 個功能
**Hooks**：8 個自動化指令碼
**配置示例**：10+ 個場景
**可直接使用**：所有示例
