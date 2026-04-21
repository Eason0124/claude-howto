---
name: 檔案生成外掛
description: 為檔案編寫、同步和校驗提供完整工作流
tags: plugins, documentation, automation
---

# 檔案生成外掛

這個外掛把檔案相關的命令、subagents、模板和 MCP 伺服器打包在一起，幫助你生成、同步和校驗檔案。

## 特性

✅ 生成 API 檔案
✅ 建立和更新 README
✅ 同步檔案
✅ 改程式式碼註釋
✅ 生成示例

## 包含內容

### 命令
- [commands/generate-api-docs.md](commands/generate-api-docs.md) - 生成 API 檔案
- [commands/generate-readme.md](commands/generate-readme.md) - 生成 README
- [commands/sync-docs.md](commands/sync-docs.md) - 同步檔案
- [commands/validate-docs.md](commands/validate-docs.md) - 校驗檔案

### Subagents
- [agents/api-documenter.md](agents/api-documenter.md) - API 檔案 subagent
- [agents/code-commentator.md](agents/code-commentator.md) - 程式碼註釋 subagent
- [agents/example-generator.md](agents/example-generator.md) - 示例生成 subagent

### 模板
- [templates/adr-template.md](templates/adr-template.md) - ADR 模板
- [templates/api-endpoint.md](templates/api-endpoint.md) - API 端點模板
- [templates/function-docs.md](templates/function-docs.md) - 函式檔案模板

### MCP 伺服器
- GitHub 整合 - 用於檔案同步

## 安裝

```bash
/plugin install documentation
```

## 使用方式

### 生成 API 檔案
```bash
/generate-api-docs
```

### 建立 README
```bash
/generate-readme
```

### 同步檔案
```bash
/sync-docs
```

### 校驗檔案
```bash
/validate-docs
```

## 適用場景

- 想標準化專案檔案產出
- 想自動生成 README、API 檔案和示例
- 想同步多個檔案之間的一致性
- 想維護程式碼註釋和示例質量

## 需求

- Claude Code 1.0+
- GitHub 訪問許可權（可選）

## 示例工作流

```text
使用者：/generate-api-docs

Claude：
1. 掃描 /src/api/ 下的所有 API 端點
2. 委派給 api-documenter subagent
3. 提取函式簽名和 JSDoc
4. 按模組 / 端點組織內容
5. 使用 api-endpoint.md 模板
6. 生成完整的 Markdown 檔案
7. 包含 curl、JavaScript 和 Python 示例

結果：
✅ API 檔案已生成
📄 已建立檔案：
   - docs/api/users.md
   - docs/api/auth.md
   - docs/api/products.md
📊 覆蓋率：23/23 個端點已檔案化
```

## 模板用途

### API 端點模板
用於編寫帶完整示例的 REST API 檔案。

### 函式檔案模板
用於編寫單個函式或方法的說明檔案。

### ADR 模板
用於記錄架構決策。

## 配置

為檔案同步設定 GitHub token：
```bash
export GITHUB_TOKEN="your_github_token"
```

## 最佳實踐

- 檔案儘量貼近程式碼
- 隨著程式碼變化同步更新檔案
- 提供可直接執行的示例
- 定期校驗檔案有效性
- 使用模板保持一致性
