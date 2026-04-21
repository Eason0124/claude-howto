<picture>
  <source media="(prefers-color-scheme: dark)" srcset="logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="logos/claude-howto-logo.svg">
</picture>

# Claude How To - 品牌資源

這是 Claude How To 專案的完整品牌資源集合，包括 logo、圖示和 favicon。所有資源都採用 V3.0 設計：一個帶有程式碼括號（`>`）符號的指南針，象徵在程式碼中引導前進，整體使用黑/白/灰配色，並以亮綠色（`#22C55E`）作為強調色。

## 目錄結構

```
resources/
├── logos/
│   ├── claude-howto-logo.svg       # 主 logo - 淺色模式 (520×120px)
│   └── claude-howto-logo-dark.svg  # 主 logo - 深色模式 (520×120px)
├── icons/
│   ├── claude-howto-icon.svg       # 應用圖示 - 淺色模式 (256×256px)
│   └── claude-howto-icon-dark.svg  # 應用圖示 - 深色模式 (256×256px)
└── favicons/
    ├── favicon-16.svg              # Favicon - 16×16px
    ├── favicon-32.svg              # Favicon - 32×32px（主用）
    ├── favicon-64.svg              # Favicon - 64×64px
    ├── favicon-128.svg             # Favicon - 128×128px
    └── favicon-256.svg             # Favicon - 256×256px
```

`assets/logo/` 下還有額外資源：
```
assets/logo/
├── logo-full.svg       # 圖形標記 + 品牌字樣（橫向）
├── logo-mark.svg       # 僅指南針符號 (120×120px)
├── logo-wordmark.svg   # 僅文字
├── logo-icon.svg       # 應用圖示 (512×512，圓角)
├── favicon.svg         # 16×16 最佳化版
├── logo-white.svg      # 適用於深色背景的白色版本
└── logo-black.svg      # 黑色單色版本
```

## 資源概覽

### 設計概念（V3.0）

**指南針 + 程式碼括號**，代表“導航遇見程式碼”：
- **指南針環** = 導航、找到方向
- **北針（綠色）** = 方向、學習路徑上的進展
- **南針（黑色）** = 紮實基礎、穩固根基
- **`>` 括號** = 終端提示符、程式碼、CLI 場景
- **刻度線** = 精準、結構化學習

### Logo

**檔案**：
- `logos/claude-howto-logo.svg`（淺色模式）
- `logos/claude-howto-logo-dark.svg`（深色模式）

**規格**：
- **尺寸**：520×120 px
- **用途**：主標題/品牌 logo，帶品牌字樣
- **使用場景**：
  - 網站頁首
  - README 徽章
  - 營銷素材
  - 印刷物料
- **格式**：SVG（可無限縮放）
- **模式**：淺色（白色背景）與深色（#0A0A0A 背景）

### 圖示

**檔案**：
- `icons/claude-howto-icon.svg`（淺色模式）
- `icons/claude-howto-icon-dark.svg`（深色模式）

**規格**：
- **尺寸**：256×256 px
- **用途**：應用圖示、頭像、縮圖
- **使用場景**：
  - App 圖示
  - 個人頭像
  - 社交媒體縮圖
  - 檔案頁首
- **格式**：SVG（可無限縮放）
- **模式**：淺色（白色背景）與深色（#0A0A0A 背景）

**設計元素**：
- 帶方位和中間方位刻度的指南針環
- 綠色北針（方向/引導）
- 黑色南針（基礎）
- 中央 `>` 程式碼括號（終端/CLI）
- 綠色中心點強調

### Favicon

為網頁使用提供了多種尺寸的最佳化版本：

| 檔案 | 尺寸 | DPI | 用途 |
|------|------|-----|------|
| `favicon-16.svg` | 16×16 px | 1x | 瀏覽器標籤頁（舊瀏覽器） |
| `favicon-32.svg` | 32×32 px | 1x | 標準瀏覽器 favicon |
| `favicon-64.svg` | 64×64 px | 1x-2x | 高 DPI 顯示器 |
| `favicon-128.svg` | 128×128 px | 2x | Apple touch icon、書籤 |
| `favicon-256.svg` | 256×256 px | 4x | 現代瀏覽器、PWA 圖示 |

**最佳化說明**：
- 16px：最小几何元素，僅保留環、指標和箭頭
- 32px：增加方位刻度
- 64px 以上：完整細節與中間方位刻度
- 全部版本都保持與主圖示一致的視覺效果
- SVG 格式保證任意尺寸下都清晰銳利

## HTML 整合

### 基礎 Favicon 配置

```html
<!-- 瀏覽器 favicon -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

<!-- Apple touch icon（移動端桌面圖示） -->
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

<!-- PWA 與現代瀏覽器 -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
```

### 完整配置

```html
<head>
  <!-- 主 favicon -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

  <!-- Apple touch icon -->
  <link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

  <!-- PWA 圖示 -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">

  <!-- Android -->
  <link rel="shortcut icon" href="/resources/favicons/favicon-256.svg">

  <!-- PWA manifest 引用（如果使用 manifest.json） -->
  <meta name="theme-color" content="#000000">
</head>
```

## 顏色方案

### 主色
- **黑色**：`#000000`（主文字、線條、南針）
- **白色**：`#FFFFFF`（淺色背景）
- **灰色**：`#6B7280`（次級文字、次要刻度）

### 強調色
- **亮綠色**：`#22C55E`（北針、中心點、強調線條，只用於高亮，絕不作為背景）

### 深色模式
- **背景**：`#0A0A0A`（接近黑色）

### CSS 變數
```css
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```

### Tailwind 配置
```js
colors: {
  brand: {
    primary: '#000000',
    secondary: '#6B7280',
    accent: '#22C55E',
  }
}
```

### 使用規範
- 主文字和結構元素使用黑色
- 次級/輔助元素使用灰色
- 綠色**僅**用於高亮，例如指標、點和強調線
- 不要把綠色用作背景色
- 保持 WCAG AA 對比度（最低 4.5:1）

## 設計規範

### Logo 使用
- 用於白色或深色（#0A0A0A）背景
- 按比例縮放
- Logo 四周保留足夠留白（最小值：logo 高度 / 2）
- 根據背景選擇對應的淺色/深色版本

### 圖示使用
- 適用於標準尺寸：16、32、64、128、256px
- 保持指南針比例
- 按比例縮放

### Favicon 使用
- 根據場景選用合適尺寸
- 16-32px：瀏覽器標籤頁、書籤
- 64px：站點圖示
- 128px 以上：Apple/Android 桌面圖示

## SVG 最佳化

所有 SVG 檔案都採用扁平設計，沒有漸變和濾鏡：
- 乾淨的基於描邊的幾何結構
- 不嵌入點陣圖
- 路徑已最佳化
- 使用響應式 `viewBox`

網頁最佳化示例：
```bash
# 壓縮 SVG，同時保持質量
svgo --config='{
  "js2svg": {
    "indent": 2
  },
  "plugins": [
    "convertStyleToAttrs",
    "removeRasterImages"
  ]
}' input.svg -o output.svg
```

## PNG 轉換

如果你需要為舊瀏覽器支援轉換成 PNG：

```bash
# 使用 ImageMagick
convert -density 300 -background none favicon-256.svg favicon-256.png

# 使用 Inkscape
inkscape -D -z --file=favicon-256.svg --export-png=favicon-256.png
```

## 無障礙性

- 高對比度顏色比例（符合 WCAG AA，最低 4.5:1）
- 幾何形狀清晰，在各種尺寸下都容易識別
- 向量格式，可無限縮放
- 圖示中不使用文字（文字由單獨的 wordmark 承擔）
- 不依賴紅綠配色表達含義

## 署名

這些資源屬於 Claude How To 專案。

**許可證**：MIT（見專案 LICENSE 檔案）

## 版本歷史

- **v3.0**（2026 年 2 月）：指南針 + 括號設計，黑/白/灰 + 綠色強調色
- **v2.0**（2026 年 1 月）：受 Claude 啟發的 12 芒星爆發式設計，祖母綠配色
- **v1.0**（2026 年 1 月）：最初基於六邊形的進度圖示設計

---

**最後更新**：2026 年 2 月
**當前版本**：3.0（指南針 + 括號）
**全部資源**：可直接用於生產環境的 SVG，支援無限縮放，符合 WCAG AA 無障礙標準
