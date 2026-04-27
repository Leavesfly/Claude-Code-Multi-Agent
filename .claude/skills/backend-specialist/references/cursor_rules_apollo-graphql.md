---
description: Definitive guidelines for building robust, performant, and maintainable apollo-graphql applications using modern best practices and tooling.
---

# apollo-graphql 最佳实践

本指南给出使用 Apollo GraphQL 开发的权威最佳实践，涵盖服务端（Apollo Server 5）和客户端（Apollo Client）两部分。遵循这些规则能保证我们 GraphQL 生态的一致性、性能与可维护性。

## 1. Schema 设计与组织

GraphQL schema 就是契约。精心设计、完整记录、按逻辑组织它。

### 1.1 基于 SDL 的 Schema-First 开发

始终先用 Schema Definition Language（SDL）定义 schema。这能在实现之前就强制你进行清晰的 API 设计。

❌ 差
```javascript
// Implicit schema definition through resolvers
const resolvers = {
  Query: {
    users: () => [{ id: '1', name: 'Alice' }],
  },
};
// Schema is inferred or built programmatically, harder to review
```

✅ 好
```graphql
# schema/user.graphql
"""Represents a user in the system."""
type User {
  id: ID!
  name: String!
  email: String
}

type Query {
  """Fetches a list of all users."""
  users: [User!]!
  """Fetches a single user by their ID."""
  user(id: ID