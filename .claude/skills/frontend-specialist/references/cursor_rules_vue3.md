---
description: Definitive guidelines for building maintainable, performant, and type-safe Vue 3 applications using modern best practices, Composition API, and TypeScript.
---

# Vue 3 最佳实践

本指南给出开发高质量 Vue 3 应用的核心规则。遵循这些标准能保证代码库的一致性、可读性和长期可维护性。

## 1. 基础环境：Linting 与格式化

**始终使用 ESLint 与 Prettier。** 两者配合能强制统一代码风格并自动捕获常见错误，加速开发与 code review。将它们集成到 IDE 中以获得实时反馈和保存时自动格式化。

## 2. 代码组织与命名

### 2.1. 单文件组件（SFC）
每个组件**必须**独立存放在自己的 `.vue` 文件中。

❌ 差
```js
// app.component('TodoList', { /* ... */ })
// app.component('TodoItem', { /* ... */ })
```

✅ 好
```
components/
├── TodoList.vue
└── TodoItem.vue
```

### 2.2. SFC 文件名大小写
所有 SFC 文件名使用 **PascalCase**。这与在 template 和 JS(X) 中引用组件的方式一致，也有利于编辑器的自动补全。

❌ 差
```
components/
├── my-component.vue
└── myComponent.vue
```