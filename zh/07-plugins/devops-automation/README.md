---
name: DevOps 自動化外掛
description: 為部署、監控和故障處理提供自動化工作流
tags: plugins, devops, automation
---

# DevOps 自動化外掛

完整的 DevOps 自動化工作流，覆蓋部署、監控和事故響應。

## 功能

✅ 自動化部署
✅ 回滾流程
✅ 系統健康監控
✅ 事故響應工作流
✅ Kubernetes 整合

## 安裝

```bash
/plugin install devops-automation
```

## 包含內容

### Slash 命令
- `/deploy` - 部署到生產或預發環境
- `/rollback` - 回滾到上一個版本
- `/status` - 檢查系統健康
- `/incident` - 處理生產事故

### 子 agents
- `deployment-specialist` - 部署操作
- `incident-commander` - 事故協調
- `alert-analyzer` - 系統健康分析

### MCP 伺服器
- Kubernetes 整合

### 指令碼
- `deploy.sh` - 部署自動化
- `rollback.sh` - 回滾自動化
- `health-check.sh` - 健康檢查工具

### Hooks
- `pre-deploy.js` - 部署前驗證
- `post-deploy.js` - 部署後任務

## 使用

### 部署到預發環境

```
/deploy staging
```

### 部署到生產環境

```
/deploy production
```

### 回滾

```
/rollback production
```

### 檢查狀態

```
/status
```

### 處理事故

```
/incident
```

## 要求

- Claude Code 1.0+
- Kubernetes CLI（kubectl）
- 已配置叢集訪問

## 配置

設定 Kubernetes 配置：

```bash
export KUBECONFIG=~/.kube/config
```

## 示例工作流

```
使用者：/deploy production

Claude:
1. 執行 pre-deploy hook（驗證 kubectl 和叢集連線）
2. 將部署委派給 deployment-specialist subagent
3. 執行 deploy.sh 指令碼
4. 透過 Kubernetes MCP 監控部署進度
5. 執行 post-deploy hook（等待 pods 就緒，執行 smoke tests）
6. 提供部署總結

結果：
✅ 部署完成
📦 版本：v2.1.0
🚀 Pods：3/3 已就緒
⏱️  用時：2m 34s
```
