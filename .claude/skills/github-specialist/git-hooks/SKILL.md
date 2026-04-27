---
name: git-hooks
description: 面向 .NET/C#、JavaScript/TypeScript、Python 和多语言代码库的 git hook 实现、现代最佳实践与工具的中央参考。涵盖框架选型（Husky.Net、lefthook、Husky、pre-commit）、设置工作流、Conventional Commits、语义化版本控制、密钥扫描（gitleaks、TruffleHog）、性能优化、CI/CD 集成、测试策略和团队协作模式。适应从个人开发者到企业团队的各种项目规模。适用于设置 git hook、配置 pre-commit/commit-msg/pre-push hook、集成 dotnet format/dotnet test、ESLint/Prettier、Black/Ruff/mypy、commitlint、在多种框架间选择、优化 hook 性能、强制代码质量、自动化测试，以及排查 hook 问题。
allowed-tools: Read, Bash, Glob, Grep
---

# Git Hooks（Git 钩子）

## 概述

本 skill 提供覆盖多种生态的现代 git hook 实现指引，主要聚焦 .NET/C# 生态，同时对 JavaScript/TypeScript 生态和 Python 生态都有强力支持。内容涵盖框架选型、设置工作流、性能优化、安全集成、测试策略和团队协作模式。通过合理配置 git hook，可以在代码提交或推送阶段自动执行格式化、代码检查、测试、密钥扫描等质量把关动作，从而在问题流入远端仓库前就被拦截下来。

**核心能力：**

1. **适应性框架推荐** —— 基于生态、规模和需求
2. **全生命周期支持** —— 从工具选择 → 设置 → 配置 → 测试 → 维护
3. **性能优先的方法** —— 并行执行与增量处理模式
4. **默认安全** —— 强制集成密钥扫描
5. **持续集成/持续部署集成** —— 纵深防御验证（本地 + 远程）
6. **真实示例** —— 可直接复制粘贴的配置

## 何时使用本 skill

在以下场景使用：

- **设置 git hook** —— 新项目或已有项目
- **选择 git hook 框架**（Husky.Net vs lefthook vs Husky vs pre-commit）
- **配置 pre-commit hook** —— 代码格式化、代码检查、安全扫描
- **配置 commit-msg hook** —— Conventional Commits 校验
- **配置 pre-push hook** —— 运行测试或构建校验
- **集成生态工具**（dotnet format、ESLint/Prettier、Black/Ruff/mypy）
- **用 commitlint 强制 Conventional Commits**
- **提交前扫描密钥**（gitleaks、TruffleHog）
- **优化 hook 性能**（慢 hook 阻塞开发者）
- **测试 git hook**（单元测试、集成测试）
- **把 hook 部署给团队**（自动安装、持续集成/持续部署强制）
- **排查 hook 问题**（激活问题、性能问题）
- **处理 monorepo** hook 模式

**触发场景：**

- "为我的 .NET 项目设置 git hook"
- "怎么在提交前强制代码格式化？"
- "配置 Conventional Commits 校验"
- "我的 pre-commit hook 太慢了"
- "C# 应该用 Husky.Net 还是 lefthook？"
- "把密钥扫描集成到 git hook 里"

## 快速决策树

面对众多 git hook 框架时，可以先回答下面几个问题，再按决策结果挑选最合适的方案。

### 该选哪个框架？

**回答以下问题，便可快速收敛出适合你的框架组合：**

1. **你的主要生态是什么？**
   - **仅 .NET/C#** → 考虑 **Husky.Net**（原生集成）或 **lefthook**（性能）
   - **仅 JavaScript/TypeScript** → 使用 **Husky** + lint-staged + commitlint
   - **仅 Python** → 使用 **pre-commit** 框架
   - **多语言（polyglot）** → 使用 **lefthook** 或 **pre-commit**

2. **团队规模有多大？**
   - **个人开发者或小团队** → 任意框架都可以；按生态选
   - **中等团队（5-20 人）** → 优先考虑易安装和持续集成/持续部署集成
   - **大型团队/企业（20 人以上）** → 优先性能（lefthook）和标准化

3. **性能要求如何？**
   - **极度追求速度** → 使用 **lefthook**（并行执行，Go 语言实现）
   - **中等关注速度** → 任意支持增量处理的框架
   - **不太关注速度** → 按生态熟悉度选择

### 快速推荐

下表给出了根据生态和团队规模做出的默认推荐。实际落地时仍可根据性能、可维护性等因素做调整。

| 生态 | 个人/小团队 | 中等团队 | 大型团队/企业 |
| --- | --- | --- | --- |
| **.NET/C#** | Husky.Net | Husky.Net 或 lefthook | lefthook |
| **JavaScript/TypeScript** | Husky + lint-staged | Husky + lint-staged | Husky + lint-staged 或 lefthook |
| **Python** | pre-commit | pre-commit | pre-commit 或 lefthook |
| **多语言** | lefthook 或 pre-commit | lefthook | lefthook |

## 框架速览对比

下表从性能、生态适用范围、配置格式、并行能力、学习曲线和 monorepo 支持等维度对主流框架做了横向对比，方便快速了解它们的差异。

| 特性 | Husky.Net | lefthook | Husky (JS) | pre-commit |
| --- | --- | --- | --- | --- |
| **性能** | 中等 | **优秀**（并行执行） | 良好（配合 lint-staged） | 良好 |
| **生态** | .NET/C# | **任意语言** | JavaScript/TypeScript | **任意语言**（以 Python 为主） |
| **配置格式** | JSON（task-runner.json） | YAML | Shell 脚本 + package.json | YAML |
| **并行执行** | ❌ 不支持 | ✅ 支持 | ❌ 不支持 | 部分支持 |
| **学习曲线** | 低（.NET 开发者） | 中等 | 低（JavaScript 开发者） | 低（Python 开发者） |
| **Monorepo 支持** | 中等 | **优秀** | 优秀 | 良好 |

**📖 详细对比：** 完整的特性分析、优劣比较和选型标准见 [references/framework-comparison.md](references/framework-comparison.md)。

## 核心能力

下面按照常见的使用场景，分别展示如何落地 git hook 方案。

### 1. .NET/C# 工作流

面向 .NET/C# 项目，推荐优先选用 Husky.Net 或 lefthook 两种方案，一个胜在原生集成，一个胜在性能。

**快速开始：** 安装 Husky.Net（`dotnet tool install Husky`）或 lefthook（`scoop install lefthook`），在项目根目录配置 task-runner.json 或 lefthook.yml，然后执行一次提交来验证 hook 是否按预期触发。

**关键模式：**

- 使用 `${staged}` 或 `{staged_files}` 做增量处理，只对本次暂存文件运行检查
- 在 pre-commit 阶段集成 dotnet format，在 pre-push 阶段集成 dotnet test，形成由快到慢的分层校验
- 把 Husky.Net 绑定到 .csproj，使团队成员首次 restore 项目时就能自动完成 hook 的安装

**📖 完整设置：** 参见 [references/setup-workflows.md](references/setup-workflows.md#netc-workflows)

**示例：**

- [examples/husky-net-dotnet-format.json](examples/husky-net-dotnet-format.json)
- [examples/lefthook-dotnet.yml](examples/lefthook-dotnet.yml)

---

### 2. JavaScript/TypeScript 工作流

面向 JavaScript 和 TypeScript 项目，Husky 加 lint-staged 是目前使用最广泛的组合，配合 commitlint 还能顺带解决提交信息规范的问题。

**快速开始：** 安装依赖（`npm install --save-dev husky lint-staged @commitlint/cli`），初始化 Husky（`npx husky init`），在 package.json 中为 lint-staged 配置每种文件类型对应的命令，然后添加 pre-commit、commit-msg 等 hook 即可。

**关键模式：**

- 使用 lint-staged 做增量处理，只跑本次暂存文件的检查，实测可带来约 15 倍的加速
- 集成 ESLint 做静态检查、Prettier 做格式化、commitlint 做提交信息校验
- 通过 package.json 中的 prepare 脚本自动完成 Husky 安装，新同学 clone 仓库后无需额外配置

**📖 完整设置：** 参见 [references/setup-workflows.md](references/setup-workflows.md#javascripttypescript-workflows)

**示例：**

- [examples/husky-javascript-package.json](examples/husky-javascript-package.json)

---

### 3. Python 工作流

Python 生态的事实标准是 pre-commit 框架，它既能管理多个 hook，又能自动隔离各个 hook 所需的虚拟环境，避免污染项目自身的依赖。

**快速开始：** 安装 pre-commit（`pip install pre-commit`），在仓库根目录创建包含 Black、Ruff、mypy 等 hook 的 `.pre-commit-config.yaml`，然后运行 `pre-commit install` 完成 hook 注册。

**关键模式：**

- 用 Ruff 做代码检查，相比 Flake8 或 pylint，可以带来 10 到 100 倍的性能提升
- 依托丰富的插件生态，社区已提供数百个预置 hook，大多数场景无需手写脚本
- 框架会自动管理每个 hook 的执行环境，开发者只需在配置文件中声明版本即可

**📖 完整设置：** 参见 [references/setup-workflows.md](references/setup-workflows.md#python-workflows)

**示例：**

- [examples/pre-commit-python.yaml](examples/pre-commit-python.yaml)

---

### 4. 多语言工作流

当一个仓库同时包含多种语言时（例如前端 TypeScript、后端 Python、基础设施 Shell），框架需要同时覆盖这些语言的工具链。

对于多语言代码库，推荐使用 **lefthook**（性能更佳，配置简洁）或 **pre-commit**（生态更丰富，插件齐全），二者都能为不同文件类型挂不同的 hook。

**📖 完整设置：** 参见 [references/setup-workflows.md](references/setup-workflows.md#polyglot-workflows)

**示例：**

- [examples/lefthook-polyglot.yml](examples/lefthook-polyglot.yml)

---

### 5. Conventional Commits 与语义化版本控制

规范化提交信息能让变更历史可读、可检索，并为自动化版本号管理和更新日志生成打下基础。

**工具：** commitlint（负责对提交信息做格式校验）、commitizen（在本地提供交互式提示来写规范提交）、semantic-release（根据提交信息自动推算版本号并生成发布内容）

**快速开始：** 安装 commitlint，在项目中配置校验规则（通常沿用 `@commitlint/config-conventional`），然后添加一个 commit-msg hook 来触发校验。

**📖 完整设置：** 参见 [references/conventional-commits.md](references/conventional-commits.md)

**示例：**

- [examples/commitlint.config.js](examples/commitlint.config.js)

---

### 6. 安全与密钥扫描

密钥一旦进入 Git 历史，即便事后删除也很难保证没有被泄漏或抓取过，因此最佳做法是在提交前就拦截住，而不是事后补救。

**工具：** gitleaks（轻量级密钥检测，配置灵活）、TruffleHog（支持 800 多种密钥类型，并能通过调用对应 API 实时验证密钥是否有效）

**快速开始：** 安装 gitleaks 或 TruffleHog，把它们加入 pre-commit hook，并为已知的误报场景配置允许列表，避免干扰开发者正常提交。

**最佳实践：** 同一套检测规则应同时部署在本地（pre-commit）**和** CI/CD（拉取请求阶段），形成纵深防御，确保绕过本地 hook 的提交仍然会在合并前被拦截。

**📖 完整设置：** 参见 [references/secret-scanning.md](references/secret-scanning.md)

---

### 7. 性能优化

当 hook 耗时过长时，开发者会倾向于使用 `--no-verify` 绕过校验，这等于让整套机制形同虚设。因此性能优化是推广 git hook 过程中绕不开的话题。

**关键策略：**

1. **增量处理** —— 仅处理已暂存文件，而不是整个仓库扫一遍，一般可带来约 15 倍的加速
2. **并行执行** —— 借助 lefthook 的并行能力同时跑多个 hook，在多核环境下可将整体耗时缩减约 90%
3. **快速工具** —— 在功能等价的前提下优先选择更快的实现，例如用 Ruff 替代 Flake8 或 pylint，可获得 10 到 100 倍的性能提升
4. **缓存** —— 善用工具自带的缓存机制，比如 ESLint 的 --cache 选项，或 dotnet 工具的 --no-build 选项，避免重复做已经做过的工作

**📖 详细策略：** 参见 [references/performance-optimization.md](references/performance-optimization.md)

---

### 8. 测试与验证

hook 本身也是代码，随着配置变得复杂，同样需要测试来保证其行为符合预期。建议按照下面的分层思路来组织测试。

**层级：**

- **单元测试** —— 针对单个 hook 脚本或子命令做独立验证，确保核心逻辑正确
- **集成测试** —— 模拟真实的提交、推送流程，验证整条工作流能够串联起来
- **CI/CD 验证** —— 在远端以相同或更严格的配置再跑一次同类检查，形成纵深防御，防止本地被绕过

**📖 示例与模式：** 参见 [references/testing-strategies.md](references/testing-strategies.md)

---

### 9. 故障排查

在 git hook 的日常维护中，最常见的问题无外乎「不生效」「太慢」和跨平台兼容性三类，以下为对应的排查线索。

**常见问题：**

- **hook 没运行** —— 重新执行框架提供的安装命令，并检查 Git 的 core.hooksPath 配置是否指向正确的目录
- **hook 太慢** —— 采用增量处理、并行执行，或替换为更快的等价工具来缓解性能瓶颈
- **Windows 路径问题** —— 路径中如包含空格或特殊字符，务必用引号包裹，并统一使用正斜杠书写

**迁移策略：** 不要一次铺开所有 hook，而是按「格式化 → 代码检查 → 提交信息校验 → 测试/安全扫描」的顺序逐步推广，每阶段都要留给团队足够的适应时间。

**📖 完整排查：** 参见 [references/troubleshooting.md](references/troubleshooting.md)

---

## 配置示例

下面这些示例文件可直接作为起点使用，只需根据实际项目做轻量调整即可落地。

**完整示例位于 `examples/` 目录：**

- [examples/husky-net-dotnet-format.json](examples/husky-net-dotnet-format.json) —— 带 dotnet format 的 Husky.Net
- [examples/lefthook-dotnet.yml](examples/lefthook-dotnet.yml) —— 面向 .NET/C# 的 lefthook
- [examples/husky-javascript-package.json](examples/husky-javascript-package.json) —— 面向 JavaScript/TypeScript 的 Husky + lint-staged
- [examples/pre-commit-python.yaml](examples/pre-commit-python.yaml) —— 面向 Python 的 pre-commit
- [examples/lefthook-polyglot.yml](examples/lefthook-polyglot.yml) —— 面向多语言代码库的 lefthook
- [examples/commitlint.config.js](examples/commitlint.config.js) —— Conventional Commits 配置

---

## 迁移与推广

将 git hook 推广到整个团队时，切忌一次性上齐所有规则，下面的渐进式策略能显著降低团队的抵触情绪。

### 渐进式推广策略

#### 阶段 1：从格式化开始（低风险）

- 添加 dotnet format、Prettier 或 Black 等格式化工具
- 先以警告模式运行，保留提交能力，给团队适应空间
- 持续收集反馈并微调配置，直到格式化结果稳定

#### 阶段 2：加入代码检查

- 添加 ESLint、Ruff 或 StyleCop 等静态检查工具
- 切换到错误模式，违规即阻塞提交，把质量红线立起来
- 集中处理历史遗留的违规，避免旧债持续干扰

#### 阶段 3：加入提交信息校验

- 接入 commitlint，对提交信息进行强约束
- 组织专门的分享，向团队普及 Conventional Commits 的动机与写法
- 推荐使用 commitizen 等交互式工具来辅助生成规范的提交信息

#### 阶段 4：加入测试与安全

- 在 pre-push 阶段加入 dotnet test，确保推送的代码至少通过本地测试
- 在 pre-commit 阶段加入 gitleaks，拦截可能的密钥泄漏
- 同步在 CI/CD 中启用相同的检查，做到双重强制

---

## 测试场景

下面的五个场景用于验证本 skill 在被触发时能否正确识别用户意图并给出匹配的建议，可以作为 skill 自测的参考用例。

### 场景 1：.NET 项目的框架选型

**用户 query：** "我需要为我的 15 人 C# 团队设置 git hook"

**预期行为：**

- 命中关键词激活：git hooks、C#、.NET
- 提供快速决策树引导框架选型
- 考虑团队规模（中等团队）
- 推荐 Husky.Net（原生集成）或 lefthook（性能）
- 链接到框架对比和设置工作流

---

### 场景 2：性能优化请求

**用户 query：** "我的 pre-commit hook 需要 35 秒，阻碍了开发者提交代码"

**预期行为：**

- 命中关键词激活：pre-commit hook、慢速、性能
- 识别性能问题
- 立即提供优化策略
- 链接到 performance-optimization.md 以深入了解

---

### 场景 3：框架对比请求

**用户 query：** "我们的 .NET 团队应该用 Husky.Net 还是 lefthook？"

**预期行为：**

- 命中框架名激活：Husky.Net、lefthook
- 提供框架对比矩阵
- 问相关的后续问题（团队规模、性能要求）
- 根据上下文给出选型建议

---

### 场景 4：Conventional Commits 实施

**用户 query：** "如何在 JavaScript 项目中强制使用 Conventional Commits？"

**预期行为：**

- 命中关键词激活：Conventional Commits、JavaScript、强制
- 识别提交校验需求
- 提供 commitlint 设置步骤
- 链接到 conventional-commits.md 完整指南

---

### 场景 5：密钥扫描集成

**用户 query：** "我想防止开发者意外将密钥提交到我们的代码库中"

**预期行为：**

- 命中关键词激活：密钥、防止、提交
- 识别安全需求
- 推荐 gitleaks 或 TruffleHog
- 链接到 secret-scanning.md
- 提及本地（pre-commit）和 CI/CD 两处强制

---

## 参考

本 skill 的指引来自多方资料与实践经验，下面列出了主要的官方文档与内部参考文件。

**官方文档（可通过 MCP 服务器访问）：**

- **Husky.Net：** <https://alirezanet.github.io/Husky.Net/>
- **lefthook：** <https://github.com/evilmartians/lefthook>
- **Husky (JS)：** <https://typicode.github.io/husky/>
- **pre-commit：** <https://pre-commit.com/>
- **commitlint：** <https://commitlint.js.org/>

**详细参考资料：**

- [references/framework-comparison.md](references/framework-comparison.md) —— 框架对比与选型标准
- [references/setup-workflows.md](references/setup-workflows.md) —— 各生态的设置工作流
- [references/conventional-commits.md](references/conventional-commits.md) —— Conventional Commits 配置
- [references/secret-scanning.md](references/secret-scanning.md) —— 密钥扫描集成
- [references/performance-optimization.md](references/performance-optimization.md) —— 性能优化策略
- [references/testing-strategies.md](references/testing-strategies.md) —— 测试与验证模式
- [references/troubleshooting.md](references/troubleshooting.md) —— 排查指南

## 版本历史

- **v1.0.0**（2025-12-26）：首次发布

---

## 最后更新

**日期：** 2025-11-28
**模型：** claude-opus-4-5-20251101

**调研来源：** Microsoft Learn MCP、Context7 MCP（Husky.Net、lefthook、Husky、pre-commit 仓库）、Perplexity MCP（2024-2025 最佳实践）、Firecrawl MCP（官方文档站点）、Ref MCP（commitlint、semantic-release）。
