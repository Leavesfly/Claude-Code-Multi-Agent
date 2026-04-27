---
name: design-system-planning
description: 规划 design system - component libraries、design tokens、文档策略、版本控制、治理和采用框架。
allowed-tools: Read, Glob, Grep, Task
---

# Design System 规划

本 skill 用于系统地规划和架构 design system，以便在多产品、多团队的环境中实现一致、可扩展、可持续演进的用户界面体验。它覆盖了从基础概念、token 体系到 component 文档、版本策略、治理模型、采用度量的全流程。

## 强制要求：首先加载 skill

在回答任何与 design system 相关的问题之前，请务必先遵循以下原则：

1. 首先完整加载本 skill，并依据其内容作答
2. 使用业界成熟的 design system 方法论，例如 Nathan Curtis 的组织实践与 Brad Frost 的 atomic design 理论
3. 所有建议都应建立在经过验证的 design system 实践之上，而不是主观经验

## Design System 基础

下面是理解一个 design system 所必需掌握的两个核心概念：它由哪几层组成，以及这些层之间如何组织。

### Design System 的组成

一个完整的 design system 通常由多层内容共同构成，下表按照从底层到上层的顺序，展示了每一层的目的与典型示例：

| 层级 | 目的 | 示例 |
|-------|---------|----------|
| **Design Tokens** | 原始值 | Colors、spacing、typography |
| **Core Components** | 构建块 | Button、Input、Card |
| **Patterns** | Component 组合 | Form、Navigation、Modal |
| **Templates** | 页面布局 | Dashboard、Detail page |
| **Guidelines** | 使用文档 | 何时使用、accessibility |
| **Tooling** | 开发支持 | Storybook、linting、testing |

### Atomic Design 方法论

Atomic design 由 Brad Frost 提出，它把界面拆解为五个递进的层次——从最小的 atom 出发，逐层组装到实际可交付的 page：

```text
Atoms → Molecules → Organisms → Templates → Pages

Atoms:       Button, Input, Icon, Label
Molecules:   Form Field (Label + Input + Error)
Organisms:   Login Form (Fields + Button + Links)
Templates:   Auth Page Layout
Pages:       Login Page (Template + Real Content)
```

## Design Tokens

Design tokens 是 design system 的「原子语言」，它们把颜色、间距、字号、圆角等视觉决策抽象成可复用的命名常量，从而解耦设计与实现。

### Token 架构

一个健壮的 token 体系通常分为三层，自上而下逐层具体化——最底层是与业务无关的原始值，中间层赋予语义，最上层为每个 component 提供定制：

```text
├── Global Tokens (Primitives)
│   ├── colors.blue.500: "#3B82F6"
│   ├── spacing.4: "16px"
│   └── font.size.base: "16px"
│
├── Semantic Tokens (Aliases)
│   ├── color.text.primary: colors.gray.900
│   ├── color.background.surface: colors.white
│   └── spacing.component.padding: spacing.4
│
└── Component Tokens (Specific)
    ├── button.background.default: color.primary.main
    ├── button.padding.horizontal: spacing.4
    └── card.border.radius: radius.medium
```

### Token 实现（.NET/CSS）

下面展示了同一套 token 在 .NET 项目与 CSS 中的典型实现方式。.NET 版本适合在 Blazor 或 WPF 等场景直接引用，CSS 版本则通过自定义属性（CSS custom property）暴露给样式层使用。两套产物理想情况下应当由同一份 token 源自动生成，以避免人工同步带来的漂移：

- .NET 版本把 token 组织成嵌套静态类，方便在强类型环境中做 IDE 补全与重构
- CSS 版本把 token 暴露为 `--color-*`、`--spacing-*` 等自定义属性，供样式表、组件样式、主题切换直接消费
- Primitives（原始值）与 semantic（语义值）务必分层声明，以便在同一套基础色板上派生不同的语义角色
- Component-level token（如 `--button-padding-x`）应当始终引用 semantic token，而不是直接引用 primitive，这样就能做到「改主题不改组件」

```csharp
// Token generation for .NET projects
public class DesignTokens
{
    public static class Colors
    {
        public static class Primary
        {
            public const string Main = "#3B82F6";
            public const string Light = "#60A5FA";
            public const string Dark = "#1D4ED8";
            public const string Contrast = "#FFFFFF";
        }

        public static class Semantic
        {
            public const string TextPrimary = "#111827";
            public const string TextSecondary = "#6B7280";
            public const string BackgroundSurface = "#FFFFFF";
            public const string BackgroundPage = "#F3F4F6";
            public const string BorderDefault = "#E5E7EB";
        }

        public static class Status
        {
            public const string Success = "#10B981";
            public const string Warning = "#F59E0B";
            public const string Error = "#EF4444";
            public const string Info = "#3B82F6";
        }
    }

    public static class Spacing
    {
        public const string Xs = "4px";
        public const string Sm = "8px";
        public const string Md = "16px";
        public const string Lg = "24px";
        public const string Xl = "32px";
        public const string Xxl = "48px";
    }

    public static class Typography
    {
        public static class FontSize
        {
            public const string Xs = "12px";
            public const string Sm = "14px";
            public const string Base = "16px";
            public const string Lg = "18px";
            public const string Xl = "20px";
            public const string Xxl = "24px";
            public const string Xxxl = "30px";
        }

        public static class FontWeight
        {
            public const string Normal = "400";
            public const string Medium = "500";
            public const string Semibold = "600";
            public const string Bold = "700";
        }

        public static class LineHeight
        {
            public const string Tight = "1.25";
            public const string Normal = "1.5";
            public const string Relaxed = "1.75";
        }
    }

    public static class Radius
    {
        public const string None = "0";
        public const string Sm = "2px";
        public const string Default = "4px";
        public const string Md = "6px";
        public const string Lg = "8px";
        public const string Xl = "12px";
        public const string Full = "9999px";
    }

    public static class Shadow
    {
        public const string Sm = "0 1px 2px 0 rgb(0 0 0 / 0.05)";
        public const string Default = "0 1px 3px 0 rgb(0 0 0 / 0.1)";
        public const string Md = "0 4px 6px -1px rgb(0 0 0 / 0.1)";
        public const string Lg = "0 10px 15px -3px rgb(0 0 0 / 0.1)";
    }
}
```

```css
/* CSS Custom Properties from tokens */
:root {
    /* Colors - Primitives */
    --color-blue-50: #EFF6FF;
    --color-blue-500: #3B82F6;
    --color-blue-600: #2563EB;
    --color-blue-700: #1D4ED8;

    /* Colors - Semantic */
    --color-primary-main: var(--color-blue-500);
    --color-primary-hover: var(--color-blue-600);
    --color-text-primary: var(--color-gray-900);
    --color-background-surface: var(--color-white);

    /* Spacing */
    --spacing-1: 4px;
    --spacing-2: 8px;
    --spacing-4: 16px;
    --spacing-6: 24px;
    --spacing-8: 32px;

    /* Typography */
    --font-size-sm: 14px;
    --font-size-base: 16px;
    --font-size-lg: 18px;
    --font-weight-medium: 500;
    --font-weight-bold: 700;
    --line-height-normal: 1.5;

    /* Component Tokens */
    --button-padding-x: var(--spacing-4);
    --button-padding-y: var(--spacing-2);
    --button-border-radius: var(--radius-md);
    --card-padding: var(--spacing-4);
    --input-height: 40px;
}
```

## Component 文档

Component 文档是 design system 最重要的「人机接口」——它决定了使用者能否快速找到正确的 component、理解其用法、遵循其规范。缺少文档的 component 几乎等同于不存在。

### Component 规范模板

下面的模板涵盖了一个 component 文档中通常应包含的关键段落，实际落地时可按项目需要裁剪：

````markdown
# Button 组件

## 概览
Button 是用户交互过程中触发主要操作的核心控件，例如提交表单、确认对话框、跳转到关键流程。它的视觉权重决定了用户对操作重要性的感知。

## 组件结构（Anatomy）
下面的示意图展示了 button 的内部布局，包括可选的左右 icon 以及用于控制留白的 padding：
```

┌─────────────────────────────────────┐
│  [Icon]  Label Text  [Icon]         │
│                                     │
│  ← padding-x →     ← padding-x →    │
└─────────────────────────────────────┘

```

## 变体（Variants）
Button 通过不同的视觉变体来表达操作的优先级与危险性，下表列出了常用的四种变体及其典型使用场景：

| 变体 | 使用场景 |
|---------|----------|
| Primary | 主要操作，典型的 CTA |
| Secondary | 次要操作，与 primary 并列出现 |
| Tertiary | 低强调操作，例如辅助链接 |
| Destructive | 危险或不可逆的操作，例如删除 |

## 状态（States）
Button 需要为以下交互状态提供清晰的视觉反馈，缺少任一状态都会影响可用性：
- 默认（Default）
- 悬停（Hover）
- 激活/按下（Active/Pressed）
- 聚焦（Focus）
- 禁用（Disabled）
- 加载中（Loading）

## 尺寸（Sizes）
下表给出了三种标准尺寸的关键度量值，设计与实现侧都应严格遵循：

| 尺寸 | 高度 | 字号 | 图标尺寸 |
|------|--------|-----------|-----------|
| Small | 32px | 14px | 16px |
| Medium | 40px | 16px | 20px |
| Large | 48px | 18px | 24px |

## Props / API

下表列出了 button component 对外暴露的全部 prop，以及它们的类型、默认值与作用：

| Prop | 类型 | 默认值 | 说明 |
|------|------|---------|-------------|
| variant | enum | primary | 视觉样式 |
| size | enum | medium | Button 尺寸 |
| disabled | boolean | false | 禁用状态 |
| loading | boolean | false | 加载状态 |
| iconLeft | ReactNode | - | 左侧图标 |
| iconRight | ReactNode | - | 右侧图标 |
| fullWidth | boolean | false | 是否占满容器宽度 |

## 无障碍（Accessibility）
为了让 button 对所有用户都可用，实现时必须遵守下列规范：
- 使用原生 `<button>` 元素承载操作，而不是 `<div>` 或 `<span>`
- 确保焦点状态明显可见，方便键盘用户定位
- 最小触摸目标不得小于 44x44px，避免移动端误触
- 仅含图标的按钮必须通过 aria-label 提供文本描述
- 加载中状态须同时设置 aria-busy，让读屏软件识别

## 应当与不应当（Do's and Don'ts）

### 应当
✓ 使用动词开头的标签，例如 "Save"、"Submit"
✓ 每个视图原则上只保留一个 primary button
✓ 保证前景与背景之间有足够对比度

### 不应当
✗ 使用含糊的标签，例如 "Click here"
✗ 在没有任何解释的情况下禁用按钮
✗ 用 button 来做导航，这类场景应改用 Link

## 示例（Examples）

### 基础用法
```razor
<Button variant="primary">Save Changes</Button>
```

### 带图标

```razor
<Button variant="secondary" iconLeft="@Icons.Plus">
    Add Item
</Button>
```

### 加载中状态

```razor
<Button loading="@isSubmitting" disabled="@isSubmitting">
    @(isSubmitting ? "Saving..." : "Save")
</Button>
```

## 相关组件

下面这些组件常常与 Button 协同出现，构成一组完整的操作语汇：

- ButtonGroup —— 把多个相关的按钮作为一组展示
- IconButton —— 仅包含图标的按钮，用于紧凑布局
- Link —— 用于导航场景的超链接

````

### Blazor Component 实现

基于前述规范，下面给出一个符合 design system 约束的 Blazor Button component 的参考实现，展示了如何把 variant、size、loading、accessibility 等属性落实到代码层面：

```csharp
@namespace DesignSystem.Components

<button
    type="@Type"
    class="@ComputedClass"
    disabled="@(Disabled || Loading)"
    aria-busy="@Loading.ToString().ToLower()"
    @onclick="HandleClick"
    @attributes="AdditionalAttributes">

    @if (Loading)
    {
        <span class="btn-spinner" aria-hidden="true"></span>
        <span class="visually-hidden">Loading</span>
    }

    @if (IconLeft is not null && !Loading)
    {
        <span class="btn-icon btn-icon-left" aria-hidden="true">
            @IconLeft
        </span>
    }

    <span class="btn-label">@ChildContent</span>

    @if (IconRight is not null)
    {
        <span class="btn-icon btn-icon-right" aria-hidden="true">
            @IconRight
        </span>
    }
</button>

@code {
    [Parameter] public RenderFragment? ChildContent { get; set; }
    [Parameter] public ButtonVariant Variant { get; set; } = ButtonVariant.Primary;
    [Parameter] public ButtonSize Size { get; set; } = ButtonSize.Medium;
    [Parameter] public bool Disabled { get; set; }
    [Parameter] public bool Loading { get; set; }
    [Parameter] public bool FullWidth { get; set; }
    [Parameter] public RenderFragment? IconLeft { get; set; }
    [Parameter] public RenderFragment? IconRight { get; set; }
    [Parameter] public string Type { get; set; } = "button";
    [Parameter] public EventCallback<MouseEventArgs> OnClick { get; set; }
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object>? AdditionalAttributes { get; set; }

    private string ComputedClass => new CssBuilder("btn")
        .AddClass($"btn-{Variant.ToString().ToLower()}")
        .AddClass($"btn-{Size.ToString().ToLower()}")
        .AddClass("btn-full-width", FullWidth)
        .AddClass("btn-loading", Loading)
        .AddClass("btn-disabled", Disabled)
        .Build();

    private async Task HandleClick(MouseEventArgs args)
    {
        if (!Disabled && !Loading)
        {
            await OnClick.InvokeAsync(args);
        }
    }
}
```

## Design System 成熟度模型

成熟度模型用于评估 design system 目前所处的阶段，以便团队定位问题、制定投入优先级。

### 成熟度等级

下表给出了从最低到最高五个等级的典型特征与该阶段应聚焦的重点事项：

| 等级 | 典型特征 | 聚焦重点 |
|-------|-----------------|-------|
| **1. Inconsistent** | 没有共享语言，各产品各自为战 | 识别已有模式 |
| **2. Foundation** | 建立了 tokens 与基础 components | 搭建核心库 |
| **3. Documented** | 形成 guidelines 与 Storybook | 文档化模式 |
| **4. Integrated** | 接入 CI/CD 与版本管理 | 扩大采用范围 |
| **5. Evolved** | 具备治理与外部贡献机制 | 持续改进 |

### 成熟度评估

下面的数据模型演示了如何把成熟度评估落到代码，通过六个维度的打分自动推导出整体等级。实际执行时可以把每个维度拆成更细的子项（例如 tooling 维度可以再细分为「CI 集成」「visual regression」「linting」等），再取平均值作为该维度的分数。这种量化评估既可以作为团队回顾的依据，也可以纳入季度或年度的 design system 健康度报告：

```csharp
public class DesignSystemMaturity
{
    public record MaturityScore(
        int Foundations,      // Tokens, primitives
        int Components,       // Component coverage
        int Documentation,    // Guidelines, examples
        int Tooling,          // Dev tools, testing
        int Governance,       // Process, contribution
        int Adoption          // Team usage
    )
    {
        public decimal OverallScore => (Foundations + Components + Documentation +
            Tooling + Governance + Adoption) / 6m;

        public MaturityLevel Level => OverallScore switch
        {
            >= 4.5m => MaturityLevel.Evolved,
            >= 3.5m => MaturityLevel.Integrated,
            >= 2.5m => MaturityLevel.Documented,
            >= 1.5m => MaturityLevel.Foundation,
            _ => MaturityLevel.Inconsistent
        };
    }

    public enum MaturityLevel
    {
        Inconsistent = 1,
        Foundation = 2,
        Documented = 3,
        Integrated = 4,
        Evolved = 5
    }
}
```

## 版本策略

Design system 面向多个消费方，每一次发布都可能波及大量产品，因此需要清晰、可预期的版本策略。

### Design System 的语义化版本

在 design system 场景下，语义化版本号的含义需要根据 token 和 component 的特点稍作收敛。和一般的后端库相比，design system 的「破坏性变更」不仅仅体现在 API 接口层面，还包括 token 的重命名、视觉行为的显著调整、component DOM 结构的变化等——这些变化一旦忽视，下游产品很可能出现肉眼可见的回归：

```text
MAJOR.MINOR.PATCH

MAJOR: Breaking changes（重命名 tokens，移除 components）
MINOR: New features（新 components，新 variants）
PATCH: Bug fixes（视觉修复，docs 更新）
```

### 变更分类

下表把常见变更按照语义化版本中的影响级别进行归类，可以作为每次发布评估版本号跳变的依据：

| 类别 | 版本影响 | 示例 |
|----------|----------------|---------|
| New component | Minor | 添加 DatePicker |
| New variant | Minor | 添加 "ghost" button |
| Token rename | Major | `color-primary` → `color-brand` |
| Token value change | Minor or Patch | 调整蓝色色值 |
| API change | Major | 重命名 prop |
| Bug fix | Patch | 修复 focus ring |
| Deprecation | Minor | 弃用 v1 API |
| Removal | Major | 移除已弃用的 API |

### Changelog 模板

下面的 changelog 模板按「破坏性变更 → 新增 → 变更 → 修复 → 文档 → 弃用」的顺序组织，能够让消费方一眼识别出升级时需要重点关注的内容。建议搭配迁移指南（migration guide）一同发布，针对每一项破坏性变更都给出具体的替换方案、代码差异示例以及过渡期策略，确保消费团队能够在有限的时间窗口内完成平滑升级：

```markdown
# Changelog

## [2.0.0] - 2025-01-15

### ⚠️ Breaking Changes
- **tokens**: Renamed `color-primary-*` to `color-brand-*`
- **Button**: Removed `type` prop, use `variant` instead

### 🚀 Added
- **DatePicker**: New date picker component
- **Button**: Added `ghost` variant
- **tokens**: Added dark mode tokens

### 🔧 Changed
- **Card**: Updated padding to use new spacing tokens
- **Input**: Improved focus ring visibility

### 🐛 Fixed
- **Modal**: Fixed focus trap for keyboard navigation
- **Select**: Fixed dropdown positioning in scrollable containers

### 📚 Documentation
- Added migration guide from v1 to v2
- New accessibility guidelines section

### ⚠️ Deprecated
- **Button**: `primary` variant renamed to `brand` (removal in v3)
```

## 治理模型

当 design system 从「单一团队维护」演进到「多团队共建」时，必须建立明确的治理模型，才能兼顾开放与一致性。

### 贡献工作流

一个典型的贡献流程包含六个阶段，它们共同保证了变更在进入主线前经过充分的评估与沟通：

```text
1. Proposal → 2. Review → 3. Design → 4. Build → 5. Document → 6. Release

Proposal:    提交 RFC，包含用例和需求
Review:      Design system 团队评估适配性
Design:      创建设计规范，获得设计批准
Build:       实现并编写测试
Document:    编写指南和示例
Release:     版本化、发布、沟通
```

### RFC 模板

RFC 是贡献流程中的核心输入，它迫使提案人提前把动机、方案、替代选择、未决问题等内容讲清楚：

```markdown
# RFC: [Component/Feature Name]

## Summary
提议变更的简要描述。

## Motivation
为什么需要这个？解决什么问题？

## Use Cases
1. [用例 1]
2. [用例 2]

## Proposed Solution
### Design
[截图，Figma 链接]

### API
[Props, tokens, 使用示例]

### Accessibility
[WCAG 考虑]

## Alternatives Considered
[其他方法以及为何被拒绝]

## Open Questions
[未解决的决策]

## Adoption Strategy
[团队如何迁移/采用]
```

### 决策日志

Decision log 用于沉淀每一次重要的设计决策及其上下文，避免团队在日后反复讨论同样的问题，也为新成员提供历史背景。每条决策都应包含「背景 → 所做的决策 → 决策依据 → 曾经考虑过的替代方案 → 当前状态」五个要素，状态字段可以标记为「提案中」「已接受」「已被新决策取代」或「已废弃」，从而形成可追溯的决策谱系：

```csharp
public class DesignDecision
{
    public required string Id { get; init; }
    public required string Title { get; init; }
    public required DateOnly Date { get; init; }
    public required string Context { get; init; }
    public required string Decision { get; init; }
    public required string Rationale { get; init; }
    public required List<string> Alternatives { get; init; }
    public required DecisionStatus Status { get; init; }
    public List<string> Supersedes { get; init; } = [];
    public string? SupersededBy { get; init; }
}

public enum DecisionStatus
{
    Proposed,
    Accepted,
    Superseded,
    Deprecated
}
```

## 从设计到代码的工作流

Design system 要真正产生价值，必须打通从 Figma 设计到代码实现的管线，让 token 和 component 的变更以可控的方式流动。

### Figma 到代码的管线

下面的管线展示了一次典型的 token 流转路径，从 Figma 的源数据一路走到发布后的包：

```text
Figma Design
    ↓
Token Export（Figma Tokens plugin）
    ↓
Token Transformation（Style Dictionary）
    ↓
Generated CSS/Code
    ↓
Component Implementation
    ↓
Storybook Stories
    ↓
Visual Regression Tests
    ↓
Published Package
```

### Token 转换配置

下面是一份 Style Dictionary 的示例配置，展示了如何从一份 token 源生成 CSS、SCSS 以及 .NET 三套产物。实际项目中还可以继续扩展：增加 iOS（Swift）、Android（XML/Kotlin）、React Native（JavaScript）等输出目标，保证各端视觉的一致性；同时通过 CI 在每次提交时自动构建这些产物并进行视觉回归测试，从而在 token 发生意外变更时及时拦截问题：

```json
// style-dictionary.config.json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "dist/css/",
      "files": [{
        "destination": "tokens.css",
        "format": "css/variables"
      }]
    },
    "scss": {
      "transformGroup": "scss",
      "buildPath": "dist/scss/",
      "files": [{
        "destination": "_tokens.scss",
        "format": "scss/variables"
      }]
    },
    "csharp": {
      "transformGroup": "csharp",
      "buildPath": "dist/csharp/",
      "files": [{
        "destination": "DesignTokens.cs",
        "format": "csharp/class"
      }]
    }
  }
}
```

## 采用度指标

采用度指标回答「design system 到底被用起来了没有」这一根本问题，是评估投入产出的关键手段。

### 跟踪采用度

下表列出了五个最具代表性的采用度指标，每项都需要结合工具自动收集或定期审计得到：

| 指标 | 衡量内容 | 目标 |
|--------|-----------------|--------|
| **Coverage** | 使用 DS components 的 UI 百分比 | >80% |
| **Adoption** | 积极使用 DS 的团队数量占比 | 100% |
| **Contribution** | 来自消费团队的 PRs 数量 | 持续增长 |
| **Support** | 待处理问题、新提交的问题 | 持续减少 |
| **Consistency** | Design audit 得分 | >90% |

```csharp
public class AdoptionMetrics
{
    public required decimal ComponentCoverage { get; init; }
    public required int TeamsAdopted { get; init; }
    public required int TotalTeams { get; init; }
    public required int ContributionsPastQuarter { get; init; }
    public required int OpenIssues { get; init; }
    public required decimal ConsistencyScore { get; init; }

    public decimal AdoptionRate => TeamsAdopted / (decimal)TotalTeams * 100;
}
```

## 检查清单：Design System 规划

在启动或重启一个 design system 项目时，可以用下面这张覆盖六大维度的检查清单来自检规划的完整度。

### 策略

- [ ] 定义目标和成功指标
- [ ] 识别目标受众
- [ ] 确定范围（哪些产品）
- [ ] 建立团队和治理机制
- [ ] 确定技术栈

### 基础

- [ ] 定义 Design tokens
- [ ] 建立 Color system
- [ ] 设置 Typography scale
- [ ] 创建 Spacing system
- [ ] 定义 Grid system

### Components

- [ ] 完成 Component audit
- [ ] 识别 Priority components
- [ ] 创建 Component 规范
- [ ] 定义 Accessibility requirements
- [ ] 确定 Implementation 方式

### 文档

- [ ] 选择 Documentation 平台
- [ ] Component documentation template
- [ ] 编写 Usage guidelines
- [ ] 提供示例代码
- [ ] 创建 Contribution guide

### 工具链

- [ ] Design tool integration（Figma）
- [ ] Storybook 或等效工具
- [ ] Visual regression testing
- [ ] Linting rules
- [ ] CI/CD pipeline

### 治理

- [ ] 版本控制策略
- [ ] 发布流程
- [ ] RFC/proposal 流程
- [ ] 支持模型
- [ ] 反馈渠道

## 相关 skill

下面这些相关 skill 常常与 design-system-planning 协同使用，共同支撑完整的产品体验建设工作：

- `accessibility-planning` —— 确保所有 components 满足无障碍标准
- `prototype-strategy` —— 基于 design system 更快地搭建 prototype
- `heuristic-evaluation` —— 对产品的一致性做启发式评估
- `information-architecture` —— 设计与 design system 协同的导航模式
