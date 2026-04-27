---
description: This guide provides definitive, opinionated best practices for building secure, performant, and maintainable applications with Firebase, focusing on modern patterns and avoiding common pitfalls.
---

# firebase 最佳实践

Firebase 是一款强大的 Backend-as-a-Service（BaaS），能显著加速开发。要有效利用它，你必须在安全性、性能和可维护性方面严格遵守最佳实践。本指南给出我们团队的权威做法。

## 1. 安全最佳实践

安全至关重要。假设所有客户端代码都已被攻破，访问控制完全通过 Firebase Security Rules 和 App Check 来强制执行。

### 1.1 启用 Firebase App Check

始终为所有支持的 Firebase 服务启用 App Check。它能验证请求来源于你的正版 app 且设备未被篡改，阻止未授权客户端访问你的后端。

**✅ 好：启用 App Check**
在 Firebase Console 中为 Firestore、Realtime Database、Storage 和 Functions 配置 App Check。在客户端尽早初始化它。

```typescript
// client/src/firebase.ts
import { initializeApp } from 'firebase/app';
import { initializeAppCheck, ReCaptchaV3Provider } from 'firebase/app-check';
// For mobile, use platform-specific providers (e.g., ReactNativeFirebaseAppCheckProvider)

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_