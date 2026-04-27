---
name: skill-creator
description: 创建有效技能的指南。当用户想要创建新技能（或更新现有技能）以通过专业知识、工作流或工具集成扩展 Claude 的功能时，应使用此技能。
license: Complete terms in LICENSE.txt
---

# Skill Creator（skill 创作助手）

本 skill 为创建有效的 skill 提供指导。

## 关于 skill

skill 是模块化、自包含的包，通过提供专业知识、workflow 和工具来扩展 Claude 的能力。可以把它们看作某个领域或任务的"入职指南"—— 它们把 Claude 从通用 agent 转变为配备了任何模型都无法完全拥有的过程性知识的专业 agent。

### skill 提供什么

1. 专业 workflow —— 针对特定领域的多步骤流程
2. 工具集成 —— 处理特定文件格式或 API 的说明
3. 领域专长 —— 公司专属知识、schema、业务逻辑
4. 打包资源 —— 用于复杂和重复任务的 script、reference 和 asset

### 一个 skill 的构成

每个 skill 由一个必需的 SKILL.md 文件和可选的打包资源组成：

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter metadata (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── Markdown instructions (required)
└── Bundled Resources (optional)
    ├── scripts/          - Executable code (Python/Bash/etc.)
    ├── references/       - Documentation intended to be loaded into context as needed
    └── assets/           - Files used in output (templates, icons, fonts, etc.)
```

#### SKILL.md（必需）

**元数据质量：** YAML frontmatter 中的 `name` 和 `description` 决定 Claude 何时使用该 skill。要具体说明 skill 做什么、何时使用。用第三人称（例如 "This skill should be used when..." 而非 "Use this skill when..."）。

#### 打包资源（可选）

##### Scripts（`scripts/`）

可执行代码（Python/Bash 等），用于需要确定性可靠或被反复重写的任务。

- **何时纳入**：当同一段代码被反复重写，或需要确定性可靠时
- **示例**：`scripts/rotate_pdf.py` 用于 PDF 旋转任务
- **好处**：token 高效、确定性、可在不加载进上下文的情况下执行
- **备注**：script 仍可能需要 Claude 读取，以做 patch 或环境相关调整

##### References（`references/`）

按需加载进上下文的文档与参考资料，用于辅助 Claude 的流程和思考。

- **何时纳入**：Claude 工作时需要参考的文档
- **示例**：`references/finance.md` 存金融 schema、`references/mnda.md` 存公司 NDA 模板、`references/policies.md` 存公司政策、`references/api_docs.md` 存 API 规格
- **用例**：数据库 schema、API 文档、领域知识、公司政策、详细 workflow 指南
- **好处**：让 SKILL.md 保持精简，仅在 Claude 判断需要时才加载
- **最佳实践**：如果文件较大（>10k 字），在 SKILL.md 中加入 grep 搜索模式
- **避免重复**：信息应只存在于 SKILL.md 或 reference 文件之一，而非两者。除非信息真正是 skill 的核心，否则优先放到 reference 文件 —— 这样 SKILL.md 保持精简，同时信息仍可发现而不占用上下文窗口。SKILL.md 只保留必需的过程性指令和 workflow 指引；把详细参考材料、schema 和示例移到 reference 文件。

##### Assets（`assets/`）

不打算被加载进上下文、而是要在 Claude 产出的内容中被使用的文件。

- **何时纳入**：当 skill 需要在最终输出里使用的文件
- **示例**：`assets/logo.png` 存品牌资产、`assets/slides.pptx` 存 PowerPoint 模板、`assets/frontend-template/` 存 HTML/React boilerplate、`assets/font.ttf` 存字体
- **用例**：模板、图片、图标、boilerplate 代码、字体、会被复制或修改的样例文档
- **好处**：把输出用的资源与文档分开，让 Claude 可以不把文件加载进上下文就使用它们

### Progressive Disclosure 设计原则

skill 使用三级加载系统来高效管理上下文：

1. **元数据（name + description）** —— 始终在上下文里（约 100 词）
2. **SKILL.md 正文** —— skill 被触发时（<5k 词）
3. **打包资源** —— 由 Claude 按需加载（无限*）

*无限是因为 script 可以在不读入上下文窗口的情况下执行。

## skill 创作流程

创建 skill 时，按顺序执行 "skill 创作流程"，只有在明确不适用的情况下才跳过某一步。

### Step 1：通过具体示例理解 skill

只有当 skill 的使用模式已经被清晰理解时才跳过本步。即使面对已有的 skill，这一步仍然有价值。

要创建有效的 skill，需要清楚地理解 skill 将如何被使用的具体示例。这种理解可以来自用户直接提供的示例，也可以来自你生成、再由用户确认的示例。

例如，构建一个 image-editor skill 时，相关问题包括：

- "What functionality should the image-editor skill support? Editing, rotating, anything else?"
- "Can you give some examples of how this skill would be used?"
- "I can imagine users asking for things like 'Remove the red-eye from this image' or 'Rotate this image'. Are there other ways you imagine this skill being used?"
- "What would a user say that should trigger this skill?"

为避免让用户负担过重，不要在一条消息里问太多问题。先问最重要的，必要时再追问。

当你对 skill 应支持的功能有了清晰认识，就可以结束本步。

### Step 2：规划可复用的 skill 内容

要把具体示例转化为有效的 skill，对每个示例做如下分析：

1. 思考如何从零开始执行这个示例
2. 识别出在反复执行这些 workflow 时，有哪些 script、reference 和 asset 会有帮助

示例：在构建 `pdf-editor` skill 处理 "Help me rotate this PDF" 这类 query 时，分析显示：

1. 旋转 PDF 每次都要重写同样的代码
2. 存一个 `scripts/rotate_pdf.py` 会有帮助

示例：在设计 `frontend-webapp-builder` skill 处理 "Build me a todo app" 或 "Build me a dashboard to track my steps" 这类 query 时，分析显示：

1. 写一个前端 webapp 每次都要同样的 HTML/React boilerplate
2. 存一个 `assets/hello-world/` 模板（包含 boilerplate HTML/React 项目文件）会有帮助

示例：在构建 `big-query` skill 处理 "How many users have logged in today?" 这类 query 时，分析显示：

1. 每次查 BigQuery 都要重新发现表的 schema 和关系
2. 存一个 `references/schema.md` 记录表 schema 会有帮助

要确立 skill 的内容，分析每个具体示例，列出要包含的可复用资源：script、reference 和 asset。

### Step 3：初始化 skill

到这一步，该真正创建 skill 了。

只有当要开发的 skill 已经存在、仅需迭代或打包时才跳过本步；这种情况下继续下一步。

从零创建新 skill 时，始终运行 `init_skill.py` 脚本。该脚本会方便地生成一个新的模板 skill 目录，自动包含 skill 需要的一切，使 skill 创作过程更高效、更可靠。

用法：

```bash
scripts/init_skill.py <skill-name> --path <output-directory>
```

该脚本会：

- 在指定路径创建 skill 目录
- 生成带正确 frontmatter 和 TODO 占位符的 SKILL.md 模板
- 创建示例资源目录：`scripts/`、`references/` 和 `assets/`
- 在每个目录下添加可自定义或删除的示例文件

初始化后，按需自定义或移除生成的 SKILL.md 和示例文件。

### Step 4：编辑 skill

编辑（新生成或已有的）skill 时，请记住：该 skill 是给另一个 Claude 实例使用的。重点纳入对 Claude 有用且不显而易见的信息。思考哪些过程性知识、领域细节或可复用资源能帮助另一个 Claude 实例更有效地完成这些任务。

#### 从可复用的 skill 内容入手

开始实现时，先从上面识别出的可复用资源入手：`scripts/`、`references/` 和 `assets/` 文件。注意此步可能需要用户输入。例如，实现 `brand-guidelines` skill 时，用户可能需要提供存入 `assets/` 的品牌资产或模板，或存入 `references/` 的文档。

同时删除 skill 不需要的示例文件和目录。初始化脚本会在 `scripts/`、`references/` 和 `assets/` 中创建示例文件来展示结构，但大多数 skill 不会全部用到。

#### 更新 SKILL.md

**写作风格：** 整个 skill 使用**祈使句/不定式**（动词开头的指令）书写，不用第二人称。使用客观、指令性的语言（如 "To accomplish X, do Y" 而非 "You should do X" 或 "If you need to do X"）。这在给 AI 消费时能保持一致性与清晰度。

要完成 SKILL.md，回答以下问题：

1. skill 的目的是什么，用几句话说明？
2. skill 在什么时候应被使用？
3. 在实践中，Claude 该如何使用这个 skill？上面规划的所有可复用内容都应被引用，这样 Claude 才知道如何使用它们。

### Step 5：打包 skill

skill 准备好后，应被打包成一个可分发的 zip 文件以分享给用户。打包过程会先自动校验 skill，确保它满足所有要求：

```bash
scripts/package_skill.py <path/to/skill-folder>
```

可选地指定输出目录：

```bash
scripts/package_skill.py <path/to/skill-folder> ./dist
```

打包脚本会：

1. **自动校验** skill，检查：
   - YAML frontmatter 格式和必需字段
   - skill 命名规范与目录结构
   - description 的完整性与质量
   - 文件组织与资源引用

2. 若校验通过则**打包** skill，创建一个以 skill 命名的 zip 文件（如 `my-skill.zip`），其中包含所有文件并保持正确的目录结构以便分发。

若校验失败，脚本会报告错误并退出而不创建包。修好校验错误后再次运行打包命令。

### Step 6：迭代

在测试 skill 后，用户可能会请求改进。通常这发生在刚用完 skill、对其表现记忆新鲜时。

**迭代 workflow：**
1. 在真实任务上使用该 skill
2. 留意卡点或低效之处
3. 判断 SKILL.md 或打包资源应如何更新
4. 实施改动并再次测试
