<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 為 Claude How To 貢獻內容

感謝你願意為這個專案做出貢獻。這份指南會幫助你更高效地參與協作。

## 關於本專案

Claude How To 是一份面向 Claude Code 的視覺化、以示例驅動的學習指南。我們提供：
- **Mermaid 圖表**，解釋各項功能的工作方式
- **可直接投入使用的模板**，你可以馬上覆制到專案中
- **帶上下文和最佳實踐的真實案例**
- **從入門到進階的漸進式學習路徑**

## 貢獻型別

### 1. 新示例或新模板
為已有功能補充示例，例如 slash commands、skills、hooks 等：
- 可直接複製貼上的程式碼
- 清楚說明工作原理
- 使用場景與收益
- 排障提示

### 2. 檔案改進
- 澄清容易混淆的內容
- 修復拼寫和語法問題
- 補充缺失資訊
- 最佳化程式碼示例

### 3. 功能指南
為新的 Claude Code 功能編寫指南：
- 分步驟教程
- 架構圖
- 常見模式和反模式
- 真實工作流

### 4. Bug 報告
報告你遇到的問題：
- 你原本期望發生什麼
- 實際發生了什麼
- 復現步驟
- 相關的 Claude Code 版本和作業系統

### 5. 反饋與建議
幫助我們改進這份指南：
- 建議更好的解釋方式
- 指出覆蓋不足的地方
- 推薦新章節或重組結構

## 快速開始

### 1. Fork 並克隆
```bash
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto
```

### 2. 建立分支
使用能說明用途的分支名：
```bash
git checkout -b add/feature-name
git checkout -b fix/issue-description
git checkout -b docs/improvement-area
```

### 3. 配置開發環境

pre-commit hooks 會在本地、每次提交前執行與 CI 相同的檢查。PR 被接受前，下面四項檢查都必須透過。

**必需依賴：**

```bash
# Python 工具鏈（uv 是本專案的包管理器）
pip install uv
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# Markdown lint 工具（Node.js）
npm install -g markdownlint-cli

# Mermaid 圖表校驗器（Node.js）
npm install -g @mermaid-js/mermaid-cli

# 安裝 pre-commit 並啟用 hooks
uv pip install pre-commit
pre-commit install
```

**驗證配置：**

```bash
pre-commit run --all-files
```

每次提交都會執行這些 hooks：

| Hook | 檢查內容 |
|------|----------|
| `markdown-lint` | Markdown 格式和結構 |
| `cross-references` | 相對連結、錨點、程式碼圍欄 |
| `mermaid-syntax` | 所有 ` ```mermaid ` 程式碼塊是否可正確解析 |
| `link-check` | 外部 URL 是否可訪問 |
| `build-epub` | `.md` 變更時 EPUB 是否能無錯誤生成 |

## 目錄結構

```
├── 01-slash-commands/      # 使用者手動觸發的快捷命令
├── 02-memory/              # 持久上下文示例
├── 03-skills/              # 可複用能力
├── 04-subagents/           # 專門化 AI 助手
├── 05-mcp/                 # Model Context Protocol 示例
├── 06-hooks/               # 事件驅動自動化
├── 07-plugins/             # 打包功能集合
├── 08-checkpoints/         # 會話快照
├── 09-advanced-features/   # 規劃、思考、後臺任務
├── 10-cli/                 # CLI 參考
├── scripts/                # 構建與工具指令碼
└── README.md            # 主指南
```

## 如何貢獻示例

### 新增 Slash Command
1. 在 `01-slash-commands/` 中建立一個 `.md` 檔案
2. 包含：
   - 清晰說明用途
   - 使用場景
   - 安裝說明
   - 使用示例
   - 自定義技巧
3. 更新 `01-slash-commands/README.md`

### 新增 Skill
1. 在 `03-skills/` 中建立一個目錄
2. 包含：
   - `SKILL.md` - 主檔案
   - `scripts/` - 如有需要可放輔助指令碼
   - `templates/` - 提示詞模板
   - README 中的示例用法
3. 更新 `03-skills/README.md`

### 新增 Subagent
1. 在 `04-subagents/` 中建立一個 `.md` 檔案
2. 包含：
   - agent 的目標和能力
   - 系統提示詞結構
   - 示例使用場景
   - 整合示例
3. 更新 `04-subagents/README.md`

### 新增 MCP 配置
1. 在 `05-mcp/` 中建立一個 `.json` 檔案
2. 包含：
   - 配置說明
   - 所需環境變數
   - 配置步驟
   - 使用示例
3. 更新 `05-mcp/README.md`

### 新增 Hook
1. 在 `06-hooks/` 中建立一個 `.sh` 檔案
2. 包含：
   - shebang 和說明
   - 邏輯清晰的註釋
   - 錯誤處理
   - 安全考慮
3. 更新 `06-hooks/README.md`

## 編寫規範

### Markdown 風格
- 使用清晰的標題層級（章節用 H2，小節用 H3）
- 段落保持簡短且聚焦
- 列表用專案符號
- 程式碼塊要標明語言
- 各章節之間保留空行

### 程式碼示例
- 示例要能直接複製貼上
- 為不明顯的邏輯新增註釋
- 同時提供簡單版和進階版
- 展示真實使用場景
- 標出可能的坑

### 檔案寫作
- 解釋“為什麼”，而不只是“是什麼”
- 包含前置條件
- 新增排障章節
- 連結相關主題
- 保持對初學者友好

### JSON/YAML
- 統一縮排風格（2 或 4 個空格）
- 為配置新增註釋
- 包含驗證示例

### 圖表
- 儘量使用 Mermaid
- 保持圖表簡潔易讀
- 在圖表下方補充說明
- 連結到相關章節

## Commit 規範

使用 conventional commit 格式：
```
type(scope): description

[optional body]
```

型別包括：
- `feat`：新功能或新示例
- `fix`：修復或更正
- `docs`：檔案變更
- `refactor`：程式碼重構
- `style`：格式調整
- `test`：新增或修改測試
- `chore`：構建、依賴等

示例：
```
feat(slash-commands): Add API documentation generator
docs(memory): Improve personal preferences example
fix(README): Correct table of contents link
docs(skills): Add comprehensive code review skill
```

## 提交前檢查

### 清單
- [ ] 程式碼符合專案風格與約定
- [ ] 新示例附帶清晰檔案
- [ ] README 已更新（本地和根目錄）
- [ ] 不包含敏感資訊（API key、憑據）
- [ ] 示例經過測試並可執行
- [ ] 連結已驗證且正確
- [ ] 檔案許可權正確（指令碼可執行）
- [ ] Commit 資訊清晰且有描述性

### 本地測試
```bash
# 執行所有 pre-commit 檢查（與 CI 相同）
pre-commit run --all-files

# 檢視改動
git diff
```

## Pull Request 流程

1. **建立帶有清晰說明的 PR**：
   - 這次提交做了什麼？
   - 為什麼需要它？
   - 是否有關聯 issue？

2. **補充相關細節**：
   - 新功能？請說明使用場景
   - 檔案？請說明改進點
   - 示例？展示前後對比

3. **關聯 issue**：
   - 使用 `Closes #123` 自動關閉相關 issue

4. **耐心等待審查**：
   - 維護者可能會提出改進建議
   - 根據反饋迭代
   - 最終決定由維護者負責

## Code Review 流程

審查者會檢查：
- **準確性**：描述是否與實際一致？
- **質量**：是否達到生產可用？
- **一致性**：是否符合專案模式？
- **檔案**：是否清晰完整？
- **安全性**：是否存在漏洞？

## 報告問題

### Bug 報告
請包含：
- Claude Code 版本
- 作業系統
- 復現步驟
- 預期行為
- 實際行為
- 如有必要，附上截圖

### 功能請求
請包含：
- 使用場景或要解決的問題
- 建議方案
- 你考慮過的替代方案
- 額外上下文

### 檔案問題
請包含：
- 哪裡令人困惑或缺失
- 建議如何改進
- 示例或參考資料

## 專案政策

### 敏感資訊
- 永遠不要提交 API key、token 或憑據
- 示例中使用佔位符
- 為配置檔案提供 `.env.example`
- 檔案中說明所需環境變數

### 程式碼質量
- 示例要聚焦且易讀
- 避免過度設計
- 對不明顯的邏輯新增註釋
- 提交前充分測試

### 智慧財產權
- 原創內容歸作者所有
- 專案採用教育用途許可證
- 尊重已有版權
- 需要時註明來源

## 獲取幫助

- **提問**：在 GitHub Issues 中發起 discussion
- **通用幫助**：先檢視現有檔案
- **開發幫助**：參考類似示例
- **Code Review**：在 PR 中 @ 維護者

## 認可

貢獻者會被收錄在：
- `README.md` 的 Contributors 部分
- GitHub contributors 頁面
- Commit 歷史

## 安全

在貢獻示例和檔案時，請遵守安全編碼實踐：

- **絕不要硬編碼金鑰或 API key** - 使用環境變數
- **提示安全影響** - 標出潛在風險
- **使用安全預設值** - 預設啟用安全功能
- **驗證輸入** - 展示正確的輸入校驗和清理方式
- **加入安全提示** - 記錄安全注意事項

如遇安全問題，請參見 [SECURITY.md](SECURITY.md) 獲取漏洞報告流程。

## 行為準則

我們承諾打造一個熱情且包容的社群。請閱讀 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 檢視完整社群標準。

簡要來說：
- 保持尊重和包容
- 愉快地接受反饋
- 幫助他人成長和學習
- 避免騷擾或歧視
- 向維護者報告問題

所有貢獻者都應遵守這份準則，並以善意和尊重對待彼此。

## 許可證

為本專案貢獻內容，即表示你同意你的貢獻將按 MIT License 許可。詳情見 [LICENSE](LICENSE) 檔案。

## 有問題？

- 檢視 [README.md](README.md)
- 閱讀 [LEARNING-ROADMAP.md](LEARNING-ROADMAP.md)
- 檢視已有示例
- 發起 issue 討論

感謝你的貢獻！
