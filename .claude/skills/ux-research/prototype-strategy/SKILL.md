---
name: prototype-strategy
description: Plan prototyping approach - fidelity levels, tool selection, prototype types, testing strategies, and design-to-development handoff.
allowed-tools: Read, Glob, Grep, Task
---

# Prototype Strategy

规划并执行从纸面草图到高保真交互原型的原型设计方法。

## 何时使用此 Skill

在以下情况使用此 skill：

- **Prototype Strategy 任务** - 致力于规划原型设计方法 - 保真度级别、工具选择、原型类型、测试策略以及设计到开发的交接
- **规划或设计阶段** - 需要关于 Prototype Strategy 方法的指导
- **最佳实践** - 希望遵循既定的模式和标准

## 强制要求：首先加载 Skill

在回答任何原型设计问题之前：

2. 使用已建立的原型设计方法论来源
3. 基于经过验证的设计实践提供所有指导

## 保真度谱系

### 保真度级别

| 级别 | 特征 | 适用场景 |
|-------|-----------------|----------|
| **低（Lo-Fi）** | 草图风格、灰度、静态 | 早期探索、概念测试 |
| **中（Mid-Fi）** | 结构化、占位符内容、有限交互 | 信息架构、流程验证 |
| **高（Hi-Fi）** | 精致、真实内容、完整交互 | 可用性测试、利益相关者确认 |

### 保真度维度

| 维度 | 低 | 中 | 高 |
|-----------|-----|--------|------|
| **视觉** | 草图、方框 | 线框图、灰色调 | 像素级完美、品牌化 |
| **内容** | Lorem ipsum | 代表性内容 | 真实内容 |
| **交互** | 静态/纸质 | 点击跳转 | 微交互 |
| **广度** | 关键页面 | 核心流程 | 完整体验 |

## 原型类型

### 按用途分类

| 类型 | 用途 | 保真度 | 工具 |
|------|---------|----------|-------|
| **概念草图** | 生成想法 | 非常低 | 纸张、白板 |
| **纸质原型** | 快速流程测试 | 低 | 纸张、便利贴 |
| **线框图** | 布局验证 | 低-中 | Figma、Balsamiq |
| **可点击模型** | 流程测试 | 中 | Figma、Sketch |
| **交互原型** | 可用性测试 | 高 | Figma、Principle |
| **代码原型** | 技术可行性 | 高 | HTML/CSS、React |
| **绿野仙踪** | 模拟复杂行为 | 可变 | 混合 |

### 决策矩阵

```text
What are you testing?
├── General concept/direction
│   └── Paper prototypes, sketches (Low)
├── Information architecture / navigation
│   └── Wireframes, tree tests (Low-Medium)
├── Specific interactions / usability
│   └── Interactive prototype (Medium-High)
├── Visual design / brand
│   └── High-fidelity mockups (High)
├── Technical feasibility
│   └── Coded prototype (High)
└── AI/complex behaviors
    └── Wizard of Oz prototype (Varies)
```

## 原型设计方法

### 纸质原型

**适用场景：** 早期概念探索、资源有限、快速迭代

```markdown
## Paper Prototype Kit

### Materials
- Index cards or paper (one per screen)
- Markers/pens
- Sticky notes (for overlays, error states)
- Scissors (for buttons that "move")
- Clear sheet (for tracing overlays)

### Running a Paper Prototype Test
1. Explain: "This is rough - we're testing the concept, not the drawing"
2. Lay out starting screen
3. Give user a task
4. User "clicks" by pointing
5. Swap/update papers to show response
6. Observe, take notes
7. Ask follow-up questions
```

### 数字低保真

**适用场景：** 远程测试、需要稍多精致度、版本控制

```csharp
// Configuration for low-fidelity prototype
public class LoFiPrototypeSpec
{
    public required string ProjectName { get; init; }
    public required List<ScreenSpec> Screens { get; init; }
    public required List<FlowSpec> Flows { get; init; }

    public LoFiGuidelines Style { get; init; } = new()
    {
        UseGrayscale = true,
        ShowPlaceholderContent = true,
        IncludeInteractions = false,
        AnnotateDecisions = true
    };
}

public class ScreenSpec
{
    public required string Name { get; init; }
    public required string Purpose { get; init; }
    public required List<string> KeyElements { get; init; }
    public string? Notes { get; init; }
}

public class FlowSpec
{
    public required string Name { get; init; }
    public required List<string> ScreenSequence { get; init; }
    public required string UserGoal { get; init; }
}
```

### 高保真交互原型

**适用场景：** 可用性测试、利益相关者演示、开发者交接

```csharp
public class HiFiPrototypeSpec
{
    public required string ProjectName { get; init; }
    public required string DesignSystemLink { get; init; }
    public required List<ScreenSpec> Screens { get; init; }
    public required List<InteractionSpec> Interactions { get; init; }
    public required List<StateSpec> States { get; init; }

    public HiFiGuidelines Style { get; init; } = new()
    {
        UseRealContent = true,
        IncludeAllStates = true,
        AddMicroInteractions = true,
        AnnotateForDev = true
    };
}

public class InteractionSpec
{
    public required string Trigger { get; init; }
    public required string Action { get; init; }
    public required string Target { get; init; }
    public AnimationType? Animation { get; init; }
    public TimeSpan? Delay { get; init; }
}

public enum AnimationType
{
    None,
    Fade,
    Slide,
    Scale,
    SmartAnimate,
    Custom
}

public class StateSpec
{
    public required string Component { get; init; }
    public required List<string> States { get; init; } // Default, Hover, Active, Disabled, etc.
}
```

### 代码原型

**适用场景：** 复杂交互、数据驱动 UI、技术验证

```csharp
// Blazor prototype component
@page "/prototype/checkout"
@inject PrototypeDataService Data

<div class="prototype-container">
    <PrototypeBanner>
        This is a prototype - not the real product
    </PrototypeBanner>

    @switch (CurrentStep)
    {
        case CheckoutStep.Cart:
            <CartView
                Items="@cartItems"
                OnProceed="() => CurrentStep = CheckoutStep.Shipping" />
            break;

        case CheckoutStep.Shipping:
            <ShippingForm
                OnBack="() => CurrentStep = CheckoutStep.Cart"
                OnProceed="HandleShippingComplete" />
            break;

        case CheckoutStep.Payment:
            <PaymentForm
                OnBack="() => CurrentStep = CheckoutStep.Shipping"
                OnProceed="HandlePaymentComplete" />
            break;

        case CheckoutStep.Confirmation:
            <OrderConfirmation Order="@confirmedOrder" />
            break;
    }

    <PrototypeControls>
        <button @onclick="ResetPrototype">Reset</button>
        <button @onclick="() => ShowNotes = !ShowNotes">
            @(ShowNotes ? "Hide" : "Show") Notes
        </button>
    </PrototypeControls>

    @if (ShowNotes)
    {
        <PrototypeNotes Step="@CurrentStep" />
    }
</div>

@code {
    private CheckoutStep CurrentStep { get; set; } = CheckoutStep.Cart;
    private List<CartItem> cartItems = [];
    private Order? confirmedOrder;
    private bool ShowNotes { get; set; }

    protected override async Task OnInitializedAsync()
    {
        cartItems = await Data.GetSampleCartItems();
    }

    private void ResetPrototype()
    {
        CurrentStep = CheckoutStep.Cart;
        confirmedOrder = null;
    }
}
```

### 绿野仙踪原型设计

**适用场景：** AI 功能、语音界面、复杂个性化

```csharp
public class WizardOfOzSetup
{
    public required string FeatureDescription { get; init; }
    public required List<ScenarioScript> Scenarios { get; init; }
    public required WizardControls Controls { get; init; }

    public class ScenarioScript
    {
        public required string UserAction { get; init; }
        public required List<WizardResponse> PossibleResponses { get; init; }
        public string? DefaultResponse { get; init; }
    }

    public class WizardResponse
    {
        public required string Label { get; init; }
        public required string Response { get; init; }
        public TimeSpan SimulatedDelay { get; init; } = TimeSpan.FromSeconds(1);
    }

    public class WizardControls
    {
        public required string TriggerHotkey { get; init; }
        public required string ResponsePanel { get; init; }
        public bool SimulateTyping { get; init; } = true;
        public bool AddRandomDelay { get; init; } = true;
    }
}
```

## 工具选择

### 工具对比

| 工具 | 保真度 | 协作 | 开发交接 | 适用场景 |
|------|----------|---------------|-------------|----------|
| **Figma** | 全部 | 优秀 | 优秀 | 团队设计 |
| **Sketch** | 中-高 | 良好（需插件） | 良好 | Mac 团队 |
| **Adobe XD** | 中-高 | 良好 | 良好 | Adobe 生态 |
| **Framer** | 高 | 良好 | 有限 | 复杂交互 |
| **InVision** | 中 | 优秀 | 良好 | 利益相关者反馈 |
| **Balsamiq** | 低 | 基础 | 不适用 | 快速线框图 |
| **Axure** | 全部 | 良好 | 详细 | 复杂规范 |
| **Principle** | 高 | 有限 | 有限 | 微交互 |
| **ProtoPie** | 高 | 良好 | 有限 | 多设备 |

### 选择标准

```csharp
public class ToolEvaluation
{
    public required string ToolName { get; init; }
    public required Dictionary<string, int> Scores { get; init; } // 1-5

    public static readonly string[] Criteria =
    [
        "FidelityRange",      // Can handle low to high
        "Collaboration",      // Real-time co-editing
        "ComponentSystem",    // Reusable components
        "InteractionDepth",   // Animation, logic
        "DevHandoff",         // Specs, assets export
        "LearningCurve",      // Ease of adoption
        "TeamExisting",       // Team already knows it
        "Cost",               // Budget fit
        "Integration"         // Works with other tools
    ];

    public decimal OverallScore => Scores.Values.Average();
}
```

## 保真度递进

### 典型递进流程

```text
Week 1-2: Discovery
├── Paper sketches
├── Concept exploration
└── Stakeholder sketches

Week 3-4: Definition
├── Wireframes
├── Information architecture
└── Content strategy

Week 5-6: Design
├── Visual design
├── Component creation
└── Style guide

Week 7-8: Prototype
├── Interactive flows
├── Micro-interactions
└── Usability testing

Week 9+: Handoff
├── Final specs
├── Asset export
└── Developer collaboration
```

### 何时跳过保真度级别

| 跳过 Lo-Fi 的情况 | 跳过 Mid-Fi 的情况 | 直接进入代码的情况 |
|-----------------|------------------|--------------------------|
| 需求明确 | 设计系统已存在 | 技术不确定性 |
| 已有模式 | 流程已知 | 性能关键 |
| 时间压力 | 团队已对齐 | 高度数据驱动 |
| 小改动 | 需要视觉验证 | 开发者偏好 |

## 使用原型进行测试

### 将保真度与测试目标匹配

| 测试目标 | 推荐保真度 |
|-----------|---------------------|
| 概念验证 | 低 |
| 导航/信息架构 | 低-中 |
| 任务完成 | 中 |
| 美学反应 | 高 |
| 微交互 | 高 |
| 无障碍性 | 中-高 |
| 情感反应 | 高 |

### 原型测试注意事项

```csharp
public class PrototypeTestPlan
{
    public required string PrototypeUrl { get; init; }
    public required PrototypeFidelity Fidelity { get; init; }
    public required List<TestTask> Tasks { get; init; }
    public required List<PrototypeLimitation> Limitations { get; init; }

    public string IntroductionScript => Fidelity switch
    {
        PrototypeFidelity.Low => """
            This is an early concept - it's rough on purpose.
            We're testing ideas, not visual design.
            Some things won't work yet - just tell me what you'd expect.
            """,

        PrototypeFidelity.Medium => """
            This is a work-in-progress design.
            It's clickable but not everything will work.
            Focus on whether this flow makes sense to you.
            """,

        PrototypeFidelity.High => """
            This is close to what the final product will look like.
            Try to use it as if it were real.
            Let me know if anything seems off or confusing.
            """,

        _ => throw new ArgumentOutOfRangeException()
    };
}

public record PrototypeLimitation(
    string What,
    string Workaround,
    string ExplainToUser
);
```

### 向参与者说明

**始终澄清：**

- 哪些功能可用，哪些不可用
- 我们正在测试设计，而不是测试他们
- 允许大声思考
- 粗糙的外观是有意为之（如果是低保真）

## 设计到开发的交接

### 交接清单

```markdown
## Prototype Handoff Checklist

### Design Specs
- [ ] All screens exported
- [ ] Component specs documented
- [ ] Spacing and sizing annotated
- [ ] Typography specs included
- [ ] Color values specified
- [ ] Breakpoints defined

### Interactions
- [ ] State changes documented
- [ ] Animations specified (timing, easing)
- [ ] Error states included
- [ ] Loading states defined
- [ ] Empty states designed

### Assets
- [ ] Icons exported (SVG)
- [ ] Images optimized
- [ ] Fonts specified/included
- [ ] Design tokens exported

### Documentation
- [ ] User flows documented
- [ ] Edge cases noted
- [ ] Accessibility requirements
- [ ] Copy/content finalized
- [ ] Component behavior specs

### Collaboration
- [ ] Figma/design file shared
- [ ] Questions channel established
- [ ] Review meetings scheduled
- [ ] Iteration process defined
```

### 交接标注模式

```csharp
public class HandoffAnnotation
{
    public required AnnotationType Type { get; init; }
    public required string Description { get; init; }
    public Position? ScreenPosition { get; init; }
    public string? LinkedComponent { get; init; }
    public Dictionary<string, string>? Properties { get; init; }
}

public enum AnnotationType
{
    Spacing,
    Behavior,
    Animation,
    Condition,
    EdgeCase,
    Accessibility,
    ResponsiveBreakpoint,
    DeveloperNote
}

// Example annotations
var annotations = new List<HandoffAnnotation>
{
    new()
    {
        Type = AnnotationType.Behavior,
        Description = "Button disabled until all required fields valid",
        LinkedComponent = "SubmitButton"
    },
    new()
    {
        Type = AnnotationType.Animation,
        Description = "Slide in from right, 300ms ease-out",
        Properties = new()
        {
            ["duration"] = "300ms",
            ["easing"] = "ease-out",
            ["direction"] = "right"
        }
    },
    new()
    {
        Type = AnnotationType.EdgeCase,
        Description = "If list > 100 items, show 'Load More' button"
    }
};
```

## 原型文档

### 原型简报模板

```markdown
# Prototype Brief: [Feature Name]

## Objective
What are we trying to learn or validate?

## Scope
- **Flows included:** [List of user flows]
- **Flows excluded:** [What's not in prototype]
- **Fidelity level:** [Low/Medium/High]

## Target Users
Who will test this? [Persona or criteria]

## Key Scenarios

### Scenario 1: [Name]
**Setup:** [Starting conditions]
**Task:** [What user tries to accomplish]
**Success:** [How we know it worked]

### Scenario 2: [Name]
[Continue...]

## Prototype Links
- **Main prototype:** [URL]
- **Design file:** [URL]
- **Assets:** [URL]

## Known Limitations
| What | Why | Workaround |
|------|-----|------------|
| [Feature] | [Reason] | [How to handle] |

## Test Plan
- **Method:** [Moderated/unmoderated]
- **Participants:** [Number]
- **Duration:** [Per session]
- **Schedule:** [Dates]

## Questions to Answer
1. [Research question 1]
2. [Research question 2]
3. [Research question 3]

## Success Criteria
- [Metric/observation that indicates success]
- [Metric/observation that indicates success]
```

## 清单：原型规划

### 策略

- [ ] 目标已定义（要学习什么）
- [ ] 保真度级别已选择
- [ ] 工具已选择
- [ ] 范围已确定
- [ ] 时间线已设定

### 创建

- [ ] 关键页面已识别
- [ ] 核心流程已映射
- [ ] 状态已定义（错误、空、加载）
- [ ] 交互已指定
- [ ] 边界情况已考虑

### 测试准备

- [ ] 测试任务已编写
- [ ] 限制已记录
- [ ] 参与者介绍脚本已就绪
- [ ] 录制设置已测试
- [ ] 成功标准已定义

### 交接

- [ ] 规范已标注
- [ ] 资产已导出
- [ ] 设计令牌已就绪
- [ ] 开发者 walkthrough 已安排
- [ ] 反馈流程已定义

## 相关 Skills

- `usability-testing` - 测试原型
- `design-system-planning` - 可复用组件
- `information-architecture` - 导航原型设计
- `accessibility-planning` - 无障碍原型
