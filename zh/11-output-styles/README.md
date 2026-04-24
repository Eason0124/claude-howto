<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Output Styles

Output Styles 改變 Claude 回應的**風格**而不改變能力 — 比如加上教學註釋、加上學習任務,或去除話術回到純工程模式。

## 預設 style

| Style | 用途 |
|-------|------|
| `Default` | 標準軟體工程模式,簡潔直接 |
| `Explanatory` | 在寫程式時額外提供「Insights」教學註釋 |
| `Learning` | 協作式學習 — Claude 會在關鍵點放 `TODO(human)` 讓你接手寫 |

## 如何切換

### 方式 1:用 `/config`

1. 在互動模式輸入 `/config`
2. 找 **Output style** 選項
3. 選一個

### 方式 2:設定檔

```json
// ~/.claude/settings.json 或 .claude/settings.json
{
  "outputStyle": "Explanatory"
}
```

變更在**新會話**生效(保持既有會話的 system prompt 穩定,確保 prompt cache 命中)。

## 自訂 style

建立 markdown 檔:

- `~/.claude/output-styles/<name>.md` — 個人(所有專案)
- `.claude/output-styles/<name>.md` — 專案(可共享)

### Frontmatter

```md
---
name: Game Backend Style
description: 聚焦遊戲伺服器後端工程,避免閒聊
keep-coding-instructions: true
---

# 指令

- 回答前先想清楚:是 matchmaking / 排行榜 / 經濟系統 / 認證 哪類?
- 所有程式碼範例使用 Node.js TypeScript 或 Go
- 涉及 Redis / MongoDB 時標明 key 命名規則
- 不做 pep talk,不總結「我剛做了什麼」
```

| Frontmatter 欄位 | 說明 |
|-----------------|------|
| `name` | 顯示名(預設用檔名) |
| `description` | 在 `/config` 選擇器中顯示的簡述 |
| `keep-coding-instructions` | 是否保留內建的 coding 相關指引,預設 `false` |

## 與 system prompt 覆寫的差異

| 方式 | 保留內建能力 | 使用場景 |
|------|-----------|---------|
| Output Style | ✅ 調整語氣 / 風格 | 長期切換的「工作模式」 |
| `--append-system-prompt` | ✅ 添加指引 | 單次會話注入 |
| `--system-prompt` | ❌ 完全替換 | 極端自訂人格(小心) |

## 常見需求對照

| 需求 | 建議做法 |
|------|---------|
| 希望 Claude 解釋思路 | 用 `Explanatory` |
| 想自己學著寫 | 用 `Learning` |
| 公司標準化語氣 | 自訂 style 放 `.claude/output-styles/` 共享 |
| 個人快速切換 | 自訂 style 放 `~/.claude/output-styles/` |

## 相關概念

- [Memory](../02-memory/README.md) — 放長期專案規則
- [Skills](../03-skills/README.md) — 放可觸發的工作流
- [Status Line](../12-status-line/README.md) — 顯示目前狀態

## 更多資源

- [官方 Output Styles 文件](https://code.claude.com/docs/en/output-styles)
