# 快速開始 - 品牌資源

## 複製資源到你的專案

```bash
# 將所有資源複製到你的 Web 專案中
cp -r resources/ /path/to/your/website/

# 或者只複製網頁所需的 favicon
cp resources/favicons/* /path/to/your/website/public/
```

## 新增到 HTML（直接複製貼上）

```html
<!-- Favicons -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
<meta name="theme-color" content="#000000">
```

## 在 Markdown/檔案中使用

```markdown
# Claude How To

![Claude How To Logo](resources/logos/claude-howto-logo.svg)

![Icon](resources/icons/claude-howto-icon.svg)
```

## 推薦尺寸

| 用途 | 尺寸 | 檔案 |
|---------|------|------|
| 網站頁首 | 520×120 | `logos/claude-howto-logo.svg` |
| 應用圖示 | 256×256 | `icons/claude-howto-icon.svg` |
| 瀏覽器標籤頁 | 32×32 | `favicons/favicon-32.svg` |
| 移動端桌面圖示 | 128×128 | `favicons/favicon-128.svg` |
| 桌面應用 | 256×256 | `favicons/favicon-256.svg` |
| 小頭像 | 64×64 | `favicons/favicon-64.svg` |

## 顏色值

```css
/* 在 CSS 中使用這些值 */
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```

## 圖示含義

**指南針 + 程式碼括號**：
- 指南針環 = 導航、結構化學習路徑
- 綠色北針 = 方向、進展、引導
- 黑色南針 = 基礎、根基
- `>` 括號 = 終端提示符、程式碼、CLI 場景
- 刻度線 = 精準、結構化步驟

這象徵著“在清晰引導下穿行於程式碼世界”。

## 在哪裡用什麼

### 網站
- **頁首**：Logo（`logos/claude-howto-logo.svg`）
- **Favicon**：32px（`favicons/favicon-32.svg`）
- **社交預覽**：圖示（`icons/claude-howto-icon.svg`）

### GitHub
- **README 徽章**：圖示（`icons/claude-howto-icon.svg`），建議 64-128px
- **倉庫頭像**：圖示（`icons/claude-howto-icon.svg`）

### 社交媒體
- **頭像**：圖示（`icons/claude-howto-icon.svg`）
- **橫幅**：Logo（`logos/claude-howto-logo.svg`）
- **縮圖**：256×256px 圖示

### 檔案
- **章節標題**：Logo 或圖示（按需縮放）
- **導航圖示**：Favicon（32-64px）

---

完整檔案見 [README.md](README.md)。
