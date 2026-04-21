---
description: 根據原始碼生成完整的 API 檔案
---

# API 檔案生成器

按以下步驟生成 API 檔案：

1. 掃描 `/src/api/` 下的所有檔案
2. 提取函式簽名和 JSDoc 註釋
3. 按端點/模組進行組織
4. 生成帶示例的 Markdown
5. 包含請求/響應 Schema
6. 新增錯誤檔案

輸出格式：
- 輸出為 `/docs/api.md` 中的 Markdown 檔案
- 為所有端點加入 curl 示例
- 新增 TypeScript 型別
