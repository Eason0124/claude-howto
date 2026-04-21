---
name: 設定 CI/CD 流水線
description: 實現 pre-commit hooks 和 GitHub Actions 質量保障
tags: ci-cd, devops, automation
---

# 設定 CI/CD 流水線

根據專案型別實現完整的 DevOps 質量門禁：

1. **分析專案**：檢測語言、框架、構建系統和現有工具鏈
2. **配置 pre-commit hooks**，使用與語言匹配的工具：
   - 格式化：Prettier/Black/gofmt/rustfmt 等
   - 程式碼檢查：ESLint/Ruff/golangci-lint/Clippy 等
   - 安全：Bandit/gosec/cargo-audit/npm audit 等
   - 型別檢查：TypeScript/mypy/flow（如適用）
   - 測試：執行相關測試套件
3. **建立 GitHub Actions 工作流**（`.github/workflows/`）：
   - 在 push/PR 時映象 pre-commit 檢查
   - 多版本/多平臺矩陣（如適用）
   - 構建和測試驗證
   - 部署步驟（如需要）
4. **驗證流水線**：本地測試、建立測試 PR，確認所有檢查都透過

使用免費/開源工具。尊重已有配置。保持執行快速。
