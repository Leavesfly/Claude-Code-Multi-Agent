---
description: Definitive guidelines for developing, deploying, and operating applications on Azure, focusing on Well-Architected Framework principles, secure coding, and efficient resource management.
---

# Azure 最佳实践

本指南给出在 Azure 上开发和运维解决方案的、有明确倾向的可执行最佳实践。它强调 **Azure Well-Architected Framework**（成本优化、安全性、可靠性、性能效率、卓越运营）和用于治理的 **Microsoft Cloud Adoption Framework**（CAF）。遵循这些标准以构建稳健、安全、成本可控的云原生应用。

## 1. 代码组织与结构

**始终使用 Infrastructure-as-Code（IaC）部署 Azure 资源。** 优先使用模块化、可复用的组件。

### 1.1. 使用 Bicep 或 Terraform 进行模块化 IaC
将基础设施拆分为逻辑清晰、可复用的模块。这能提升可维护性、可复用性，并强制一致性。

❌ 差：单体 ARM 模板或单个大 Bicep/Terraform 文件。
```bicep
// main.bicep - too many resources, hard to manage
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  // ... all storage config
}
resource appServicePlan 'Microsoft.Web/serverfarms@2022-09-01' = {
  // ... all app service plan config
}
// ... many more resources
```

✅ 好：模块化 Bicep（或 Terraform modules）。
```bicep
// main.bicep
module storage '