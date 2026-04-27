# UI、主題與鍵盤自訂

> 涵蓋 v2.1.109 ~ v2.1.119 的 UI / 主題 / Vim / 快捷鍵變更。其他 UI 相關功能參見 [11-output-styles](../11-output-styles/README.md) 與 [12-status-line](../12-status-line/README.md)。

---

## 自訂主題（v2.1.118）

`/theme` 命令現在可從 `~/.claude/themes/` 目錄載入使用者自訂的 JSON 主題,而不再僅限於內建選項。

### 自訂主題檔案結構

```text
~/.claude/themes/
├── solarized-dark.json
├── monokai-pro.json
└── company-brand.json
```

### 主題 JSON 格式

```json
{
  "name": "solarized-dark",
  "displayName": "Solarized Dark",
  "colors": {
    "background": "#002b36",
    "foreground": "#839496",
    "accent": "#268bd2",
    "warning": "#cb4b16",
    "error": "#dc322f",
    "success": "#859900",
    "muted": "#586e75"
  },
  "syntax": {
    "keyword": "#268bd2",
    "string": "#2aa198",
    "comment": "#586e75"
  }
}
```

### 套用方式

```bash
/theme                      # 開啟主題選單,自訂主題會列在內建主題之後
/theme solarized-dark       # 直接套用
```

### 「Auto (match terminal)」主題（v2.1.111）

新增的 `auto` 主題會自動跟隨終端機 dark / light mode,不需手動切換。適合終端機本身已經有主題切換機制(macOS Terminal、iTerm2、Windows Terminal)的使用者。

```bash
/theme auto
```

---

## Vim 模式增強（v2.1.118 / v2.1.119）

### Visual mode 與 Visual-line mode（v2.1.118）

Vim 模式新增兩個選取模式:

| 按鍵 | 模式 | 用途 |
|------|------|------|
| `v` | Visual | 字元層級選取 |
| `V` | Visual-line | 整行選取 |
| `Esc` | 退出回 Normal | — |

選取後可用標準 Vim 動作(`y` 複製、`d` 剪下、`c` 取代等)。

### INSERT 模式 Esc 行為修正（v2.1.119）

過去在 INSERT 模式按 Esc 會意外**拉取佇列訊息**(把背景已產生但尚未顯示的訊息一次出來),導致 UI 跳動。v2.1.119 修正後 Esc 只負責退出 INSERT 模式,不會干擾訊息流。

> Vim 模式啟用方式:`/config → Editor mode`(`/vim` 命令已自 v2.1.92 起移除)。

---

## 控制台快捷鍵

### Ctrl+U 清空輸入緩衝區（v2.1.111）

過去 Ctrl+U 行為不一致(有時清到行首、有時清整段)。v2.1.111 統一為**清空整個輸入緩衝區**,行為與多數 readline / shell 一致。

### Ctrl+O 切換 verbose transcript（v2.1.110，行為變更）

舊版 Ctrl+O 是「儲存對話到檔案」;v2.1.110 改為**切換 verbose transcript 檢視**,可在精簡與完整輸出之間快速切換,方便除錯。

> 若你習慣舊行為:儲存對話請改用 `/export [filename]`。

### Fullscreen Shift+↑/↓ 捲動 viewport（v2.1.113）

進入 fullscreen / TUI 模式(`/tui`)時,使用 Shift+↑/↓ 可捲動 viewport,不會誤觸 history navigation。

| 快捷鍵 | 動作 |
|--------|------|
| Shift+↑ / Shift+↓ | 捲動 viewport |
| ↑ / ↓ | 上一筆 / 下一筆 prompt history |
| Page Up / Page Down | 整頁捲動 |

---

## Thinking spinner 內嵌進度（v2.1.109 / v2.1.116）

Extended thinking 的 spinner 過去只顯示靜態 "Thinking..." 字樣,v2.1.109 起會顯示**內嵌進度提示**(token 數、推理階段、近期決策摘要),v2.1.116 進一步改善動畫流暢度。

不需特別設定,預設啟用。若 transcript 顯示過於擁擠可在 `/config → UI` 關閉。

---

## 平滑捲動改善（v2.1.116）

VS Code、Cursor、Windsurf 全螢幕模式的捲動表現大幅改善(主因是底層 terminal renderer 改寫)。Linux + ghostty 使用者可獲得最大收益。

---

## 快速參考表

| 功能 | 版本 | 觸發方式 |
|------|------|---------|
| 自訂主題 JSON | v2.1.118 | `~/.claude/themes/*.json` + `/theme` |
| Auto match terminal 主題 | v2.1.111 | `/theme auto` |
| Vim visual mode | v2.1.118 | `v` / `V`(Vim 模式啟用後) |
| Vim Esc 修正 | v2.1.119 | 自動套用 |
| Ctrl+U 清空緩衝區 | v2.1.111 | INSERT 模式 |
| Ctrl+O 切 verbose transcript | v2.1.110 | 全模式 |
| Shift+↑/↓ 捲動 viewport | v2.1.113 | `/tui` 模式 |
| Thinking spinner 進度 | v2.1.109 / v2.1.116 | 自動 |

---

## 相關文件

- [Output Styles](../11-output-styles/README.md) — 切換回應風格
- [Status Line](../12-status-line/README.md) — 底部狀態列
- [Settings 與環境變數](../10-cli/config-and-env-reference.md) — `autoScrollEnabled` 等設定
- [CLI Flags 完整對照表](../10-cli/flags-reference.md)
- [官方 Interactive Mode](https://code.claude.com/docs/en/interactive-mode)
