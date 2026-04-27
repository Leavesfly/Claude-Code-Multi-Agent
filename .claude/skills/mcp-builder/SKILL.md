---
name: mcp-builder
description: 创建高质量 MCP（模型上下文协议）服务器的指南，使 LLM 能够通过精心设计的工具与外部服务交互。在构建 MCP 服务器以集成外部 API 或服务时使用，无论是 Python (FastMCP) 还是 Node/TypeScript (MCP SDK)。
license: Complete terms in LICENSE.txt
---

# MCP Server 开发指南

## 概述

使用本 skill 创建高质量的 MCP（Model Context Protocol）server，让 LLM 可以有效地与外部服务交互。MCP server 提供工具，使 LLM 能够访问外部服务和 API。一个 MCP server 的质量，取决于它在多大程度上能让 LLM 借助所提供的工具完成真实任务。

---

# 流程

## 🚀 整体 workflow

创建一个高质量的 MCP server 涉及四个主要阶段：

### 阶段 1：深入调研与规划

#### 1.1 理解面向 agent 的设计原则

动手实现之前，先回顾这些为 AI agent 设计工具的原则：

**围绕 workflow 构建，而不是只包裹 API endpoint：**
- 不要简单地封装现有 API endpoint —— 要构建经过深思熟虑、影响力大的 workflow 型工具
- 合并相关操作（例如 `schedule_event` 同时检查可用时段并创建事件）
- 关注能完成完整任务的工具，而不仅是单个 API 调用
- 思考 agent 实际需要完成哪些 workflow

**为有限的 context 做优化：**
- agent 的 context window 受限 —— 每一个 token 都要用好
- 返回高信噪比的信息，不要 data dump
- 提供 "concise" 与 "detailed" 两种响应格式
- 默认使用可读性强的标识（名字）而非技术代码（ID）
- 把 agent 的 context 预算视为稀缺资源

**设计可行动的错误信息：**
- 错误信息应该引导 agent 走向正确用法
- 给出具体下一步建议："试试 filter='active_only' 来减少结果"
- 让错误具有教育性，而不仅是诊断性
- 通过清晰反馈帮 agent 学会正确使用工具

**遵循自然的任务划分：**
- 工具名应该反映人类对任务的思维方式
- 相关工具用统一前缀归类，便于发现
- 围绕自然 workflow 设计工具，而不是完全按 API 结构设计

**采用评估驱动的开发：**
- 尽早创建真实的评估场景
- 让 agent 反馈驱动工具改进
- 快速原型，并基于 agent 实际表现迭代

#### 1.3 研究 MCP 协议文档

**拉取最新的 MCP 协议文档：**

使用 WebFetch 加载：`https://modelcontextprotocol.io/llms-full.txt`

这份文档包含完整的 MCP 规范和指南。

#### 1.4 研究框架文档

**加载并阅读以下 reference 文件：**

- **MCP Best Practices**：[📋 View Best Practices](./reference/mcp_best_practices.md) —— 所有 MCP server 的核心指引

**如果是 Python 实现，还需加载：**
- **Python SDK 文档**：用 WebFetch 加载 `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
- [🐍 Python Implementation Guide](./reference/python_mcp_server.md) —— Python 专属最佳实践和示例

**如果是 Node/TypeScript 实现，还需加载：**
- **TypeScript SDK 文档**：用 WebFetch 加载 `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
- [⚡ TypeScript Implementation Guide](./reference/node_mcp_server.md) —— Node/TypeScript 专属最佳实践和示例

#### 1.5 彻底研究 API 文档

要集成一个服务，请通读**所有**可用的 API 文档：
- 官方 API reference 文档
- 身份认证与授权要求
- rate limit 和分页模式
- 错误响应与状态码
- 可用 endpoint 及其参数
- 数据模型与 schema

**为了获取完整信息，按需使用 web search 和 WebFetch 工具。**

#### 1.6 制定完整的实现方案

基于你的调研，制定一份详细方案，包括：

**工具选择：**
- 列出最有价值的 endpoint/操作
- 优先选能支撑最常见、最重要用例的工具
- 思考哪些工具搭配使用能支持复杂 workflow

**共享工具与 helper：**
- 识别常见的 API 请求模式
- 规划分页 helper
- 设计过滤与格式化工具
- 规划错误处理策略

**输入/输出设计：**
- 定义输入校验模型（Python 用 Pydantic，TypeScript 用 Zod）
- 设计一致的响应格式（例如 JSON 或 Markdown），以及可配置的详略级别（如 Detailed 或 Concise）
- 为大规模使用做规划（数千用户/资源）
- 实现字符上限和截断策略（例如 25,000 token）

**错误处理策略：**
- 规划优雅失败模式
- 设计清晰、可行动、LLM 友好、以自然语言促使进一步行动的错误信息
- 考虑 rate limit 和 timeout 场景
- 处理认证与授权错误

---

### 阶段 2：实现

拿到完整方案后，按语言对应的最佳实践开始实现。

#### 2.1 搭建项目结构

**Python：**
- 创建单个 `.py` 文件；如果比较复杂则拆成多个模块（参见 [🐍 Python Guide](./reference/python_mcp_server.md)）
- 使用 MCP Python SDK 注册工具
- 定义 Pydantic 模型用于输入校验

**Node/TypeScript：**
- 创建合适的项目结构（参见 [⚡ TypeScript Guide](./reference/node_mcp_server.md)）
- 配置 `package.json` 和 `tsconfig.json`
- 使用 MCP TypeScript SDK
- 定义 Zod schema 用于输入校验

#### 2.2 先实现核心基础设施

**开始实现时，先于工具创建共享 utility：**
- API 请求 helper 函数
- 错误处理 utility
- 响应格式化函数（JSON 和 Markdown）
- 分页 helper
- 认证/token 管理

#### 2.3 系统地实现工具

对方案中每个工具：

**定义输入 schema：**
- 使用 Pydantic（Python）或 Zod（TypeScript）做校验
- 添加合适的约束（min/max 长度、regex、min/max 值、范围等）
- 提供清晰、具有描述性的字段描述
- 在字段描述中加入多样化示例

**编写完整的 docstring/description：**
- 一句话总结该工具做什么
- 对目的与功能的详细说明
- 显式的参数类型及示例
- 完整的返回类型 schema
- 使用示例（何时用、何时不用）
- 错误处理文档，说明遇到具体错误时如何继续

**实现工具逻辑：**
- 使用共享 utility，避免代码重复
- 所有 I/O 遵循 async/await 模式
- 做好错误处理
- 支持多种响应格式（JSON 和 Markdown）
- 尊重分页参数
- 检查字符上限并适当截断

**加上工具标注：**
- `readOnlyHint`：true（只读操作）
- `destructiveHint`：false（非破坏性操作）
- `idempotentHint`：true（重复调用效果相同）
- `openWorldHint`：true（与外部系统交互）

#### 2.4 遵循语言对应的最佳实践

**此时，加载对应语言的指南：**

**Python：加载 [🐍 Python Implementation Guide](./reference/python_mcp_server.md) 并确保以下：**
- 使用 MCP Python SDK 正确注册工具
- 使用带 `model_config` 的 Pydantic v2 模型
- 全部代码都有类型注解
- 所有 I/O 操作使用 async/await
- 合理组织 import
- 模块级常量（CHARACTER_LIMIT、API_BASE_URL）

**Node/TypeScript：加载 [⚡ TypeScript Implementation Guide](./reference/node_mcp_server.md) 并确保以下：**
- 正确使用 `server.registerTool`
- Zod schema 使用 `.strict()`
- TypeScript strict 模式开启
- 不使用 `any` —— 使用正确类型
- 显式 `Promise<T>` 返回类型
- 配置好构建流程（`npm run build`）

---

### 阶段 3：审查与打磨

初步实现后：

#### 3.1 代码质量审查

为确保质量，从以下方面审查代码：
- **DRY 原则**：工具之间没有重复代码
- **可组合性**：把共享逻辑抽为函数
- **一致性**：相似操作返回相似格式
- **错误处理**：所有外部调用都有错误处理
- **类型安全**：全量类型覆盖（Python 类型注解、TypeScript 类型）
- **文档**：每个工具都有完整的 docstring/description

#### 3.2 测试与构建

**重要：** MCP server 是长时运行的进程，通过 stdio/stdin 或 sse/http 等待请求。直接在你的主进程里运行它们（例如 `python server.py` 或 `node dist/index.js`）会导致进程无限挂起。

**测试 server 的安全方式：**
- 使用评估 harness（见阶段 4）—— 推荐方式
- 在 tmux 中运行 server，使其脱离主进程
- 测试时加 timeout：`timeout 5s python server.py`

**Python：**
- 验证 Python 语法：`python -m py_compile your_server.py`
- 通过阅读文件检查 import 是否正常
- 手动测试：在 tmux 中运行 server，然后在主进程中用评估 harness 测试
- 或直接使用评估 harness（它会为 stdio 传输自行管理 server）

**Node/TypeScript：**
- 运行 `npm run build` 并确保无错误
- 确认生成了 dist/index.js
- 手动测试：在 tmux 中运行 server，然后在主进程中用评估 harness 测试
- 或直接使用评估 harness（它会为 stdio 传输自行管理 server）

#### 3.3 使用质量检查清单

要验证实现质量，加载对应语言指南中的 checklist：
- Python：见 [🐍 Python Guide](./reference/python_mcp_server.md) 的 "Quality Checklist"
- Node/TypeScript：见 [⚡ TypeScript Guide](./reference/node_mcp_server.md) 的 "Quality Checklist"

---

### 阶段 4：创建评估

实现完 MCP server 后，创建全面的评估以检验其有效性。

**加载 [✅ Evaluation Guide](./reference/evaluation.md) 获取完整评估指南。**

#### 4.1 理解评估的目的

评估测试 LLM 是否能有效地用你的 MCP server 回答真实、复杂的问题。

#### 4.2 创建 10 个评估问题

要创建有效的评估，按评估指南中的流程操作：

1. **工具盘点**：列出可用工具，了解其能力
2. **内容探索**：使用只读操作来探索可用数据
3. **问题生成**：创建 10 个复杂、真实的问题
4. **答案验证**：自己解答每个问题来验证答案

#### 4.3 评估要求

每个问题都必须：
- **独立**：不依赖其他问题
- **只读**：仅需非破坏性操作
- **复杂**：需要多次工具调用和深入探索
- **真实**：基于人们实际关心的用例
- **可验证**：答案单一、明确、可用字符串比较验证
- **稳定**：答案不会随时间变化

#### 4.4 输出格式

创建如下结构的 XML 文件：

```xml
<evaluation>
  <qa_pair>
    <question>Find discussions about AI model launches with animal codenames. One model needed a specific safety designation that uses the format ASL-X. What number X was being determined for the model named after a spotted wild cat?</question>
    <answer>3</answer>
  </qa_pair>
<!-- More qa_pairs... -->
</evaluation>
```

---

# Reference 文件

## 📚 文档库

开发过程中按需加载以下资源：

### 核心 MCP 文档（最先加载）
- **MCP 协议**：从 `https://modelcontextprotocol.io/llms-full.txt` 拉取 —— 完整 MCP 规范
- [📋 MCP Best Practices](./reference/mcp_best_practices.md) —— 通用 MCP 指南，包括：
  - server 和工具命名规范
  - 响应格式指引（JSON vs Markdown）
  - 分页最佳实践
  - 字符上限和截断策略
  - 工具开发指南
  - 安全与错误处理标准

### SDK 文档（阶段 1/2 期间加载）
- **Python SDK**：从 `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md` 拉取
- **TypeScript SDK**：从 `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md` 拉取

### 语言专属实现指南（阶段 2 期间加载）
- [🐍 Python Implementation Guide](./reference/python_mcp_server.md) —— 完整 Python/FastMCP 指南，包括：
  - server 初始化模式
  - Pydantic 模型示例
  - 用 `@mcp.tool` 注册工具
  - 完整可用示例
  - 质量检查清单

- [⚡ TypeScript Implementation Guide](./reference/node_mcp_server.md) —— 完整 TypeScript 指南，包括：
  - 项目结构
  - Zod schema 模式
  - 用 `server.registerTool` 注册工具
  - 完整可用示例
  - 质量检查清单

### 评估指南（阶段 4 期间加载）
- [✅ Evaluation Guide](./reference/evaluation.md) —— 完整评估创建指南，包括：
  - 问题创建指引
  - 答案验证策略
  - XML 格式规范
  - 示例问题与答案
  - 使用提供的脚本运行评估
