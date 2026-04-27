<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 高階功能

這是一份關於 Claude Code 高階能力的完整指南，涵蓋 planning mode、extended thinking、auto mode、後臺任務、許可權模式、列印模式（非互動）、會話管理、互動功能、訊息通道（Channels）、語音輸入、遠端控制、網頁會話、桌面應用、任務列表、提示詞建議、Git 工作樹、沙盒、受管設定和配置。

## 目錄

1. [概覽](#概覽)
2. [規劃模式](#規劃模式)
3. [擴充套件思考](#擴充套件思考)
4. [自動模式](#自動模式)
5. [後臺任務](#後臺任務)
6. [定時任務](#定時任務)
7. [許可權模式](#許可權模式)
8. [無頭模式](#無頭模式)
9. [會話管理](#會話管理)
10. [互動功能](#互動功能)
11. [語音輸入](#語音輸入)
12. [訊息通道（Channels）](#訊息通道channels)
13. [Chrome 整合](#chrome-整合)
14. [遠端控制](#遠端控制)
15. [網頁會話](#網頁會話)
16. [桌面應用](#桌面應用)
17. [Push Notification Tool](#push-notification)
18. [任務列表](#任務列表)
19. [提示詞建議](#提示詞建議)
20. [Git 工作樹（Git Worktrees）](#git-工作樹git-worktrees)
21. [沙盒](#沙盒)
22. [企業受管設定](#企業受管設定)
23. [配置與設定](#配置與設定)
24. [平台改進](#platform-improvements)
25. [最佳實踐](#最佳實踐)
26. [更多資源](#更多資源)

---

<a id="overview"></a>
## 概覽

Claude Code 的高階功能把基礎能力擴充套件到了規劃、推理、自動化和控制層面。它們能支援更復雜的開發任務、程式碼審查、自動化流程以及多會話管理。

**核心高階功能包括:**
- **Planning Mode**:先寫詳細實現計劃,再開始編碼
- **Extended Thinking**:對複雜問題進行更深入的推理
- **Auto Mode**:由後臺安全分類器在每一步執行前進行審查(Research Preview)
- **Background Tasks**:長時間任務不阻塞對話
- **Permission Modes**:控制 Claude 可以做什麼(`default`、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`)
- **Print Mode**:非互動式執行,適合自動化和 CI/CD(`claude -p`,詳見 [10-cli/print-mode.md](../10-cli/print-mode.md))
- **Session Management**:管理多個會話
- **Interactive Features**:快捷鍵、多行輸入、歷史記錄
- **Voice Dictation**:按住說話,支援 20 種語言的語音識別
- **Channels**:訊息通道,允許 MCP server 向執行中的會話推送訊息(Research Preview)
- **Remote Control**:從 Claude.ai 或 Claude app 控制本地會話
- **Web Sessions**:在瀏覽器中執行 Claude Code
- **Desktop App**:支援視覺化 diff 審查和多會話的獨立應用
- **Task List**:跨 context compaction 持久跟蹤任務
- **Prompt Suggestions**:根據上下文智慧推薦命令
- **Git 工作樹(Git Worktrees)**:隔離的 worktree 分支,適合並行工作
- **Sandboxing**:作業系統級檔案系統和網路隔離
- **Managed Settings**:透過 plist、Registry 或受管檔案進行企業部署
- **Configuration**:用 JSON 配置檔案定製行為
- **Output Styles**:改變回應風格(Default / Explanatory / Learning)→ 詳見 [../11-output-styles/](../11-output-styles/README.md)
- **Status Line**:底部狀態列顯示 context / cost / git 等 → 詳見 [../12-status-line/](../12-status-line/README.md)

<a id="planning-mode"></a>
## 規劃模式

更多規劃示例見 [planning-mode-examples.md](planning-mode-examples.md)。

Planning mode 允許 Claude 在真正實現前先梳理複雜任務，生成一份你可以審閱並批准的詳細計劃。

### 什麼是 Planning Mode？

Planning mode 是一個兩階段流程：
1. **規劃階段**：Claude 分析任務並生成詳細實現計劃
2. **實現階段**：在你批准後，Claude 執行計劃

### 什麼時候使用 Planning Mode

✅ 適合：
- 複雜的多檔案重構
- 新功能開發
- 架構調整
- 資料庫遷移
- 大型 API 重設計

❌ 不建議：
- 簡單 bug 修復
- 格式化修改
- 單檔案編輯
- 快速查詢

### 如何啟動 Planning Mode

**Slash command：**
```bash
/plan Implement user authentication system
```

**CLI 引數：**
```bash
claude --permission-mode plan
```

**設為預設：**
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**快捷鍵：**
- `Shift + Tab` - 切換許可權模式（包括 plan）
- `Alt + M` - 切換許可權模式（Windows/Linux）

### Planning Mode 的好處

- **結構化實施方式**：把實現步驟拆得更清楚
- **審閱和批准**：先看計劃，再執行
- **風險識別**：先發現潛在問題
- **階段清晰**：有明確的里程碑和階段劃分

### 示例：功能實現

```text
User: /plan Add a real-time notification system

Claude: 我會為實時通知系統制定計劃。

## 實現計劃

### 階段 1：基礎設施
1. 安裝 WebSocket 庫（socket.io）
2. 搭建 WebSocket server
3. 配置連線管理

### 階段 2：後端
4. 建立通知服務
5. 實現通知佇列
6. 為通知新增資料庫 schema
7. 建立 API 端點

### 階段 3：前端
8. 搭建 WebSocket client
9. 建立通知 UI 元件
10. 實現通知狀態管理
11. 新增通知許可權

### 階段 4：測試與釋出
12. 編寫單元測試
13. 編寫整合測試
14. 更新檔案
15. 部署到 staging

預計時間：3-4 小時
需要修改的檔案：12 個
需要建立的新檔案：8 個

準備繼續嗎？（yes/no/modify plan）
```

### 計劃修改

```text
User: 修改計劃 - 先跳過佇列，後面再加

Claude: 已更新計劃：
[展示刪除佇列後的計劃]

User: 看起來不錯，繼續

Claude: [按修改後的計劃開始實現]
```

### Planning Mode 配置

你可以透過 CLI 引數或 slash command 啟用 planning mode：

```bash
# 透過 CLI 啟用 plan mode
claude --permission-mode plan

# 或在 REPL 裡使用 /plan
/plan Implement user authentication system
```

**規劃專用模型別名**：使用 `opusplan`，規劃用 Opus，執行用 Sonnet：

```bash
claude --model opusplan "design and implement the new API"
```

**外部編輯計劃**：按 `Ctrl+G` 可以把當前計劃開啟到外部編輯器裡進行更詳細的修改。

**Plan 檔案命名（v2.1.111）**：Plan 檔案現在以提示詞自動命名,格式為 `<action>-<adjective>-<animal>.md`(例如 `fix-auth-race-snug-otter.md`、`refactor-payment-eager-phoenix.md`),取代舊版的隨機 hash。命名出現在 `~/.claude/plans/`,方便日後 grep / 回顧。

<a id="extended-thinking"></a>
## 擴充套件思考

Extended thinking 讓 Claude 在給出解決方案前，花更多時間進行復雜推理。

### 什麼是 Extended Thinking？

這是一個有意識的、分步驟的推理過程，Claude 會：
- 拆解複雜問題
- 比較多種方案
- 評估權衡
- 推導邊界情況

### 如何啟用

**快捷鍵：**
- `Option + T`（macOS）/ `Alt + T`（Windows/Linux）- 切換 extended thinking

**自動啟用：**
- 對所有模型預設開啟（Opus 4.7、Sonnet 4.6、Haiku 4.5）
- Opus 4.7（v2.1.111）支援自適應推理，effort 等級為 `low`（○）、`medium`（◐）、`high`（●）、`xhigh`、`max`（`xhigh` 與 `max` 僅 Opus 4.7）
- 透過 `/effort` 可開啟互動式滑塊（v2.1.111）

### 好處

- 更適合複雜問題
- 更容易發現邊界條件
- 便於比較多個方案
- 輸出通常更穩健

### 示例：架構決策

```text
Context Analysis
Approach 1: Microservices
Approach 2: Monolith
Decision Factors
Recommendation
```

### 配置

```json
{
  "thinking": {
    "enabled": true,
    "effort": "high"
  }
}
```

<a id="auto-mode"></a>
## 自動模式

Auto mode 是一個研究預覽功能。它會在每個動作執行前，用後臺安全分類器評估風險，幫助你在自動化和安全之間取得平衡。**自 v2.1.111 起對 Max 訂閱用戶開放使用 Opus 4.7。**

### 需求

- 支援 auto mode 的 Claude Code 版本
- 合適的許可權和配置
- 使用 Opus 4.7 需 Max 訂閱（v2.1.111+）

### 啟用方式

```bash
# 直接以 auto mode 啟動
claude --permission-mode auto

# 或啟動後用 Shift+Tab 循環切換
```

> ⚠️ 舊 flag `--enable-auto-mode` 已在 **v2.1.111 移除**,改用 `--permission-mode auto`。

### 安全機制

- Auto mode 的自訂規則**不從專案共享設定**(`.claude/settings.json`)讀取
- 這避免不受信任的 repo 透過共享設定控制你的自動核准
- 只從 `~/.claude/settings.json`(使用者)與 `.claude/settings.local.json`(本地私密)讀
- 組織管理員可在 managed settings 設 `"disableAutoMode": "disable"` 完全移除 auto 選項

### 分類器如何工作

在執行工具前，後臺分類器會判斷這個動作是否安全、是否需要提示、是否應該被阻止。

### 預設阻止的動作

- 明顯危險的檔案操作
- 高風險 shell 命令
- 可能洩露憑據的操作

### 預設允許的動作

- 只讀檢查
- 常規分析
- 低風險修改

### 配置 Auto Mode

Auto mode 可以在設定中進一步約束、放寬或與許可權模式結合使用。

### 回退行為

如果分類器無法判定，Claude 會回退到更保守的許可權處理方式。

### 預置一份類似 Auto-Mode 的保守許可權基線

```bash
# 先預覽將要新增的許可權（不會寫入）

# 應用保守基線

# 只有在需要時再逐步增加能力
```

<a id="background-tasks"></a>
## 後臺任務

後臺任務允許 Claude 在不阻塞對話的情況下執行長時間操作。

### 什麼是後臺任務？

後臺任務適合長時間執行的工作，例如構建、測試、下載、掃描或並行分析。你可以把它們交給 Claude，讓對話繼續保持響應。

### 啟動後臺任務

你可以從命令、計劃或工具鏈中啟動後臺任務，讓 Claude 在後臺繼續工作。

### 管理後臺任務

- 檢視任務狀態
- 中止任務
- 等待任務完成
- 從結果中恢復上下文

### 示例：並行開發

```text
任務 A：執行測試
任務 B：生成檔案
任務 C：執行安全掃描
```

### 配置

後臺任務的超時、併發和通知行為都可以在設定中調整。

<a id="scheduled-tasks"></a>
## 定時任務

Scheduled tasks 讓 Claude 按計劃重複執行某些提示詞或任務。

### `/loop` 命令

```bash
# 顯式間隔
/loop 10m Run tests and report failures

# 自然語言
/loop every day at 9am check release notes
```

### 一次性提醒

```bash
/schedule "remind me to review the PR in 30 minutes"
```

### 管理定時任務

- 檢視任務列表
- 修改執行頻率
- 暫停或取消任務

### 行為細節

定時任務會遵循當前會話與許可權配置，並按計劃觸發執行。

### 雲端定時任務

部分場景可以在雲端環境中執行排程任務。

### 禁用定時任務

如果你不希望自動觸發，關閉相關設定即可。

### 示例：監控部署

```text
每 5 分鐘檢查一次部署狀態，直到 health check 透過。
```

<a id="permission-modes"></a>
## 許可權模式

Permission modes 決定 Claude 可以直接執行哪些操作。

### 可用模式

- `default`
- `acceptEdits`
- `plan`
- `auto`
- `dontAsk`
- `bypassPermissions`

### 啟用方式

可以透過命令列、設定或會話中的快捷操作切換許可權模式。

### 示例

#### Default Mode

預設的平衡模式，必要時會詢問許可權。

#### Plan Mode

只做規劃，不直接執行高風險改動。

#### Accept Edits Mode

更偏向自動接受編輯，適合受控環境。

### 使用場景

- 程式碼審查
- 自動化重構
- CI/CD
- 高風險操作前的審批流程

<a id="headless-mode"></a>
## 無頭模式

Headless mode 指非互動式執行 Claude Code，常用於自動化和 CI/CD。

### 什麼是 Print Mode？

Print mode 就是 `claude -p`：給一個任務，輸出結果，然後退出。

### 在 Print Mode 中執行

```bash
# 執行指定任務
claude -p "run tests and summarize failures"

# 處理管道內容
cat logs.txt | claude -p "find the root cause"

# CI/CD 整合
claude -p "review this diff and return JSON"
```

### 其他示例

```bash
# 捕獲輸出
claude -p "analyze this code" > report.txt

# 結構化輸出
claude -p --output-format json "list files"

# 標準輸入
echo "hello" | claude -p "translate to Chinese"
```

### CI/CD 示例

```yaml
# .github/workflows/code-review.yml
```

### Print Mode 配置

- 限制自主回合數
- 使用結構化 JSON 輸出
- 啟用 schema 驗證
- 禁用會話持久化

<a id="session-management"></a>
## 會話管理

會話管理用於在多個會話之間恢復、重新命名、分叉和持續工作。

### 常用命令

- `/resume`(v2.1.116 在大型會話速度提升 67%)
- `/rename`
- `/fork`(v2.1.77 已更名為 `/branch`,別名仍可用)
- `claude -c`
- `claude -r`

### 恢復會話

```bash
# 繼續最後一次對話
claude -c

# 恢復指定名稱或 ID 的會話
claude -r "feature-auth"
```

### 分叉會話

```bash
# 恢復並分叉，適合實驗
claude -r "feature-auth" --fork
```

### 會話持久化

你可以選擇保留、繼續或重建會話上下文。

<a id="interactive-features"></a>
## 互動功能

互動功能包括快捷鍵、多行輸入、歷史記錄、Vim 模式和 Bash 模式等。

### 快捷鍵

常用快捷鍵用於切換模式、編輯輸入、檢視歷史和控制輸出。

### 自定義快捷鍵

你可以在設定中繫結自己的按鍵組合。

### 可用上下文

不同上下文會影響快捷鍵的可用性和行為。

### Chord 支援

支援組合按鍵觸發複雜操作。

### 保留鍵與衝突鍵

某些按鍵會被系統或終端佔用，需要避免衝突。

### Tab 補全

輸入命令、引數或路徑時會提供補全建議。

### 命令歷史

Claude 會保留歷史記錄，方便你快速重用之前的輸入。

### 多行輸入

```text
第一行
第二行
第三行
```

### 行內編輯

支援在輸入過程中編輯中間內容。

### Vim 模式

如果你熟悉 Vim，可以啟用類似的編輯體驗。

### Bash 模式

可在會話中直接進入更偏 shell 風格的操作方式。

<a id="voice-dictation"></a>
## 語音輸入

語音輸入支援按住說話和多語言識別，適合快速記錄想法或在不方便鍵盤輸入時使用。

### 如何啟用

在設定中開啟語音輸入即可。

### 特性

- 支援按住說話
- 支援 20 種語言的語音識別
- 適合快速口述任務

### 配置

可在 settings 中控制靈敏度、語言和輸入行為。

<a id="channels"></a>
## 訊息通道（Channels）

訊息通道允許 MCP servers 向正在執行的會話推送訊息。

### 訂閱 channels

```bash
# 啟動時訂閱 channel 外掛
```

### 支援的整合

- Discord
- Telegram
- 其他 channel 型整合

### 配置

透過 MCP 配置或 settings 訂閱相應 channel。

### 工作方式

外部系統可以把訊息推送進 Claude 的當前會話裡，讓你在一個會話中接收通知或事件。

<a id="chrome-integration"></a>
## Chrome 整合

Chrome 整合讓 Claude 可以協助瀏覽器自動化和站點級操作。

### 啟用 Chrome 整合

開啟相關開關後，Claude 就可以與瀏覽器協作。

### 能力

- 瀏覽器自動化
- 頁面操作
- 網站級互動

### 站點級許可權

你可以對站點設定更細粒度的訪問許可權。

### 工作方式

Claude 透過瀏覽器連線與頁面互動。

### 已知限制

- 某些站點會限制自動化
- 複雜頁面可能需要更多許可權

<a id="remote-control"></a>
## 遠端控制

Remote Control 可以讓你從 Claude.ai 或 Claude app 遠端控制本地 Claude Code 會話。

### 啟動 Remote Control

```bash
# 使用預設會話名啟動
claude --remote-control

# 使用自定義名稱
claude --remote-control --name my-session
```

### 連線到會話

你可以在遠端端連線到已經啟動的本地會話，繼續同一個上下文。

### 安全性

遠端控制會牽涉到會話和許可權，因此建議只在可信環境中使用。

### Remote Control vs Claude Code on the web

Remote Control 是控制本地會話；web 版則是在瀏覽器中直接執行。

### 侷限

- 網路可用性依賴更高
- 某些本地資源可能不可遠端訪問

### 使用場景

- 在手機上檢視並接管會話
- 遠端繼續長期任務

<a id="web-sessions"></a>
## 網頁會話

Web Sessions 允許你直接在瀏覽器裡建立和恢復 Claude Code 會話。

### 建立 Web Session

```bash
# 從 CLI 建立新的 web session
claude --remote "implement API"
```

### 在本地恢復 Web Session

```bash
# 在本地終端恢復 web session
claude --teleport
```

### 使用場景

- 瀏覽器裡快速開始任務
- 在本地和網頁之間切換
- 遠端繼續工作

<a id="desktop-app"></a>
## 桌面應用

桌面應用提供獨立介面，適合視覺化 diff 審查和多會話管理。

### 安裝

按照桌面應用的安裝說明安裝即可。

### 從 CLI 接力

你可以把 CLI 會話交給桌面應用繼續處理。

### 核心功能

- 視覺化 diff
- 多會話
- 更直觀的審查體驗

### App 預覽配置

可以調整預覽、佈局和視覺呈現方式。

### Connectors

桌面應用可以連線到不同的工作環境。

### 遠端和 SSH 會話

桌面應用也支援遠端或 SSH 場景下的接力。

### Desktop 中的許可權模式

Desktop 裡同樣可以使用不同的許可權模式。

### 企業功能

企業部署下可使用更多受管和合規相關能力。

<a id="push-notification"></a>
## Push Notification Tool（v2.1.110）

Claude Code 提供 `PushNotification` 工具,用於從 hook、subagent 或 long-running task 觸發桌面 / 行動端推送通知。

### 用途

- 長任務完成後通知(測試跑完、build 完成、deploy 結束)
- Hook 在關鍵事件時推送(failed test、安全掃描發現問題)
- Subagent 等待你回應時主動提醒

### 接收端

- macOS Notification Center
- Windows Toast
- Claude.ai mobile app(若已綁定遠端控制)

### 範例:在 hook 中使用

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash(npm test:*)",
        "hooks": [
          {
            "type": "command",
            "command": "claude --notify 'Tests done'"
          }
        ]
      }
    ]
  }
}
```

<a id="task-list"></a>
## 任務列表

Task List 用於跨 context compaction 持久儲存任務進度。

### 切換任務列表

在設定裡開啟或關閉任務列表顯示。

### 持久任務

任務會保留，即使上下文被壓縮，也能繼續跟蹤。

### 命名任務目錄

你可以給任務分組或目錄命名，方便管理。

<a id="prompt-suggestions"></a>
## 提示詞建議

Prompt suggestions 會根據當前上下文智慧推薦下一步命令或提示詞。

### 工作方式

系統會結合你的歷史、當前任務和上下文給出建議。

### 禁用建議

如果你不想看到建議，可以在設定裡關閉。

<a id="git-worktrees"></a>
## Git 工作樹（Git Worktrees）

Git worktrees 讓你在隔離的目錄裡並行工作，非常適合實驗和分支開發。

### 從 worktree 啟動

```bash
# 在隔離 worktree 中啟動 Claude Code
claude -w
```

### Worktree 位置

你可以控制 worktree 的放置位置，保持主工作區整潔。

### Monorepo 的 sparse checkout

在大型 monorepo 中，可配合 sparse checkout 只載入需要的部分。

### Worktree 工具和 Hooks

worktree 場景下依然可以配合工具和 hooks 自動化流程。

### 自動清理

不需要的 worktree 可以自動清理。

### 使用場景

- 並行嘗試多個方案
- 隔離高風險重構
- 保持主分支整潔

<a id="sandboxing"></a>
## 沙盒

沙盒提供檔案系統和網路層面的隔離，能顯著降低自動化操作的風險。

### 啟用沙盒

開啟後，Claude 的很多操作都會在受控環境裡執行。

### 配置項

你可以配置允許訪問的目錄、網路行為和例外列表。

### 示例配置

```json
{
  "sandbox": {
    "enabled": true
  }
}
```

### 工作方式

沙盒會限制 Claude 可觸達的檔案和網路邊界。

### 使用場景

- 低風險自動化
- 在不完全信任的輸入上執行任務
- 受控環境中的批處理

<a id="managed-settings-enterprise"></a>
## 企業受管設定

企業可以透過受管設定統一分發 Claude Code 配置。

### 部署方式

- plist
- Registry
- 受管檔案

### 受管配置片段

透過受管配置片段，可以給團隊統一推送策略。

### 可用的受管設定

- 許可權模式
- 功能開關
- MCP / hooks / 外掛約束

### macOS plist 示例

```xml
<!-- 示例略 -->
```

<a id="configuration-and-settings"></a>
## 配置與設定

### 配置檔案位置

- `~/.claude/settings.json`
- `.claude/settings.json`
- `.claude/settings.local.json`

### 完整配置示例

```json
{
  "permissions": {
    "defaultMode": "plan"
  },
  "thinking": {
    "enabled": true,
    "effort": "high"
  },
  "autoCheckpoint": true
}
```

### 環境變數

#### 模型選擇

- `ANTHROPIC_API_KEY`
- `CLAUDE_MODEL`

#### API 配置

- `ANTHROPIC_API_KEY`

#### Thinking 配置

- `CLAUDE_EFFORT`

#### 功能開關

- `CLAUDE_ENABLE_AUTO_MODE`

#### MCP 配置

- `CLAUDE_MCP_CONFIG`

#### 任務管理

- `CLAUDE_TASK_LIST`

#### Agent team（實驗性）

- `CLAUDE_TEAM_MODE`

#### Subagent 和 plugin 配置

- `CLAUDE_PLUGIN_DATA`

#### 子程式和流式處理

- `CLAUDE_OUTPUT_FORMAT`

### 配置管理命令

你可以用命令檢視或更新當前配置，並把它們落到 settings 中。

### 每個專案的配置

推薦把專案專用設定放進專案內的 `.claude/settings.json`，這樣能跟程式碼一起版本化。

<a id="platform-improvements"></a>
## 平台改進

### macOS / Linux 原生二進位檔（v2.1.113）

Claude Code 自 v2.1.113 起改為**原生二進位檔**,不再是捆綁 JavaScript runtime。優點:

- 啟動更快(冷啟動從 ~1.5s 降至 ~300ms)
- 記憶體佔用降低
- 不再依賴系統 Node.js 版本
- 安裝體積略增,但首次啟動體驗顯著改善

### 嵌入式 `bfs` 與 `ugrep`（v2.1.117）

原生二進位內嵌兩個高效能搜尋工具,不再依賴系統工具:

- **`bfs`** — 廣度優先檔案搜尋(取代 `find`,效能更好)
- **`ugrep`** — Unicode 正則搜尋(取代 `grep` 部分用途)

對於大型 monorepo 的程式碼搜尋與檔案探索任務,效能提升顯著。

### PowerShell 工具(Windows，v2.1.111 / v2.1.119）

Windows 平台漸進推出 PowerShell 工具,取代部分需要 cmd / WSL bash 的場景。

```powershell
$env:CLAUDE_CODE_USE_POWERSHELL_TOOL = "1"
claude
```

v2.1.119 起,PowerShell 工具指令可在 `--permission-mode` 中**自動批准**(同 Bash 工具的處理方式)。

### WSL 繼承 Windows 受管設定（v2.1.118）

WSL 環境會自動繼承 Windows 主機的受管設定(plist / Registry / 受管檔案),不需在 WSL 內重複配置。受管設定優先級最高。

### Plugin 依賴自動安裝（v2.1.117）

Plugin manifest 可宣告依賴項(其他 plugins、MCP servers),`/plugin install` 會自動從市集解析並安裝相依的 plugins。

```json
{
  "name": "my-plugin",
  "dependencies": {
    "plugins": ["base-plugin@marketplace"],
    "mcpServers": ["github"]
  }
}
```

### `/doctor` 可在回應時開啟（v2.1.116）

過去 `/doctor` 只能在 idle 狀態使用,v2.1.116 起即使 Claude 正在生成回應也可開啟,方便即時檢查健康狀態。

<a id="best-practices"></a>
## 最佳實踐

### Planning Mode

- 複雜任務先規劃，再實現
- 計劃要可審閱

### Extended Thinking

- 留給複雜問題
- 不要在簡單任務上過度使用

### Background Tasks

- 用於長時間、可並行的任務
- 記得設定超時和可見性

### Permissions

- 預設最小許可權
- 只在必要時放寬

### Sessions

- 為重要會話命名
- 必要時分叉，而不是硬改

<a id="additional-resources"></a>
## 更多資源

- [根目錄中文指南](../README.md)
- [Slash Commands 中文參考](../01-slash-commands/README.md)
- [Checkpoints 中文指南](../08-checkpoints/README.md)
- [Plugins 中文指南](../07-plugins/README.md)
- [CLI 中文參考](../10-cli/README.md)
