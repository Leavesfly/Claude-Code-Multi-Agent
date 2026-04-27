---
name: service-blueprinting
description: 创建 service blueprints - frontstage/backstage 可视化、touchpoints、support processes、evidence 和 service design methodology。
allowed-tools: Read, Glob, Grep, Task
---

# Service Blueprinting（服务蓝图）

本 skill 用于设计和可视化端到端的服务体验，把 customer 的操作、员工的操作、以及背后支撑服务运转的系统与流程以分层的方式呈现出来，从而让跨职能团队能够看清整个服务的全貌，并据此定位问题、发现机会。

## 何时使用此 Skill

遇到以下场景时，应当主动启用本 skill：

- **Service Blueprinting 任务** —— 需要创建 service blueprint，绘制 frontstage/backstage 可视化、梳理 touchpoint、支持流程、evidence 以及落地 service design methodology
- **规划或设计** —— 需要在项目中获得关于 service blueprinting 方法、流程与交付物的系统指导
- **最佳实践** —— 希望在实施过程中遵循业界既定的模式、模板和评估手段，避免从零摸索

## 强制要求：优先加载 Skill

在回答任何与 service design 相关的问题之前，请先确认以下原则：

1. 本 skill 是该领域问题的权威参考入口，应优先查阅并依据其内容作答
2. 使用成熟的 service design methodology，例如 Lynn Shostack 的原始方法论以及 Nielsen Norman Group 的实践指引
3. 所有建议都建立在经过验证的 service blueprinting 实践之上，而不是主观经验

## Service Blueprint 结构

一张完整的 service blueprint 通常由五条核心 lane 和若干条扩展 lane 组成，它们共同回答「客户看到什么、做什么、感受如何」以及「员工做什么、系统做什么」这两类问题。

### 五个核心 Lane

下面这张结构示意图从上到下展示了 service blueprint 的五个经典 lane，以及把它们分隔开的三条重要「line」：

```text
┌─────────────────────────────────────────────────────────────────┐
│                    PHYSICAL EVIDENCE                             │
│  (What customer sees, receives, interacts with)                  │
├─────────────────────────────────────────────────────────────────┤
│                    CUSTOMER ACTIONS                              │
│  (Steps the customer takes)                                      │
├─────────────────────────── LINE OF INTERACTION ─────────────────┤
│                    FRONTSTAGE ACTIONS                            │
│  (Employee actions visible to customer)                          │
├─────────────────────────── LINE OF VISIBILITY ──────────────────┤
│                    BACKSTAGE ACTIONS                             │
│  (Employee actions hidden from customer)                         │
├─────────────────────────── LINE OF INTERNAL INTERACTION ────────┤
│                    SUPPORT PROCESSES                             │
│  (Systems, partners, policies that enable service)               │
└─────────────────────────────────────────────────────────────────┘
```

### 额外 Lane（扩展 Blueprint）

在五条核心 lane 的基础上，还可以叠加若干扩展 lane 来丰富对服务的描述，尤其是在需要量化评估与绩效管理时：

| Lane | 描述 |
|------|-------------|
| **Time** | 每个步骤的持续时间 |
| **Emotional Journey** | 客户在整个过程中的感受 |
| **Metrics** | 每个 touchpoint 的 KPI |
| **Fail Points** | 可能出现问题的地方 |
| **Wait Points** | 发生延迟的地方 |
| **Ownership** | 负责人 |

## Blueprint 组件

下面按 lane 依次展开每一类组件的数据模型，帮助你在工程实现或研究文档中对 blueprint 进行结构化表达。

### Physical Evidence

Physical evidence 指的是客户在服务过程中真实接触到的一切可见或可感知的元素，无论是数字界面还是实物材料都算在内：

```csharp
public class PhysicalEvidence
{
    public required string Name { get; init; }
    public required EvidenceType Type { get; init; }
    public required string Description { get; init; }
    public string? DesignConsiderations { get; init; }
}

public enum EvidenceType
{
    Digital,        // 网站、应用、邮件
    Physical,       // 门店、包装、收据
    Environmental,  // 标识、氛围
    Documentation,  // 表格、合同
    Communication   // 通知、确认
}
```

### Customer Actions

Customer actions 是整张 blueprint 的主轴，用来记录客户为达成目标所执行的每一个具体步骤，以及对应的 channel、耗时和痛点：

```csharp
public class CustomerAction
{
    public int Step { get; init; }
    public required string Action { get; init; }
    public required string Intent { get; init; } // 他们试图实现的目标
    public required CustomerChannel Channel { get; init; }
    public TimeSpan? ExpectedDuration { get; init; }
    public string? PainPoint { get; init; }
    public string? Opportunity { get; init; }
}

public enum CustomerChannel
{
    Web,
    Mobile,
    Phone,
    InPerson,
    Email,
    Chat,
    Social
}
```

### Frontstage Actions

Frontstage actions 是员工或系统在客户眼前进行的操作，交互方式可能是同步的、异步的、自助的，也可能是员工协助完成的：

```csharp
public class FrontstageAction
{
    public int Step { get; init; }
    public required string Action { get; init; }
    public required string Actor { get; init; } // 角色/系统
    public required InteractionType Type { get; init; }
    public bool IsAutomated { get; init; }
    public List<string> Dependencies { get; init; } = [];
}

public enum InteractionType
{
    Synchronous,    // 实时交互
    Asynchronous,   // 邮件、通知
    SelfService,    // 客户驱动，系统支持
    Assisted        // 员工帮助客户
}
```

### Backstage Actions

Backstage actions 指客户看不见、但又是服务得以实现的必要操作，每一项都应有明确的 owner、触发方式、SLA 以及可能的失败模式：

```csharp
public class BackstageAction
{
    public int Step { get; init; }
    public required string Action { get; init; }
    public required string Owner { get; init; }
    public required string TriggeredBy { get; init; }
    public TimeSpan? SLA { get; init; }
    public List<string> Systems { get; init; } = [];
    public string? FailureMode { get; init; }
}
```

### Support Processes

Support processes 涵盖了支撑整套服务运行的系统、政策、合作伙伴、培训与基础设施，它们一旦缺失，frontstage 与 backstage 的动作都会失效：

```csharp
public class SupportProcess
{
    public required string Name { get; init; }
    public required SupportType Type { get; init; }
    public required string Description { get; init; }
    public List<string> DependentActions { get; init; } = [];
}

public enum SupportType
{
    Technology,     // CRM、数据库、API
    Policy,         // 业务规则、合规
    Partner,        // 第三方服务
    Training,       // 员工知识
    Infrastructure  // 物理或云资源
}
```

## Blueprint 模板

下面这份模板把一张 service blueprint 所需的全部要素组织成可直接复用的文档结构，实际使用时按服务阶段逐段填写即可：

````markdown
# Service Blueprint: [服务名称]

## 服务概述
- **服务：** [名称]
- **范围：** [起点] 到 [终点]
- **主要 Persona：** [目标用户]
- **服务承诺：** [价值主张]

---

## Blueprint

### 阶段 1: [阶段名称]

| 元素 | 详情 |
|---------|---------|
| **Physical Evidence** | [客户看到的内容] |
| **Customer Action** | [客户执行的操作] |
| **Frontstage** | [可见的员工/系统操作] |
| **Backstage** | [隐藏的操作] |
| **Support** | [支持系统/流程] |
| **Duration** | [时间] |
| **Emotion** | [客户感受] |
| **Fail Point** | [可能出错的地方] |

### 阶段 2: [阶段名称]
[继续模式...]

---

## 可视化 Blueprint

```text

Time →     [5 分钟]        [2 分钟]         [24 小时]       [5 分钟]
           ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
Evidence   │ Website │    │ Form    │    │ Email   │    │ Product │
           └─────────┘    └─────────┘    └─────────┘    └─────────┘

           ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
Customer   │ 浏览    │───►│ 提交    │───►│ 接收    │───►│ 开箱    │
           │ 目录    │    │ 订单    │    │ 确认    │    │ 产品    │
           └─────────┘    └─────────┘    └─────────┘    └─────────┘
           ═══════════════════════════════════════════════════════════
           Line of Interaction
           ═══════════════════════════════════════════════════════════
           ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
Frontstage │ 展示    │    │ 确认    │    │ 发送    │    │ 交付    │
           │ 产品    │    │ 支付    │    │ 更新    │    │ 包裹    │
           └─────────┘    └─────────┘    └─────────┘    └─────────┘
           ═══════════════════════════════════════════════════════════
           Line of Visibility
           ═══════════════════════════════════════════════════════════
           ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
Backstage  │ 目录    │    │ 处理    │    │ 拣货 &  │    │ 运输    │
           │ 管理    │    │ 支付    │    │ 包装    │    │ 交接    │
           └─────────┘    └─────────┘    └─────────┘    └─────────┘
           ═══════════════════════════════════════════════════════════
           Line of Internal Interaction
           ═══════════════════════════════════════════════════════════
           ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
Support    │ 产品    │    │ 支付    │    │ WMS     │    │ 承运商  │
           │ 数据库  │    │ 网关    │    │         │    │ API     │
           └─────────┘    └─────────┘    └─────────┘    └─────────┘

Emotion    😊 好奇      😟 焦虑        😌 安心        🎉 兴奋
           ─────────────────────────────────────────────────────────
                        ▼ Fail Point: 支付失败

```

---

## Pain Points & Opportunities

### 已识别的 Pain Points

| 阶段 | Pain Point | 影响 | 根本原因 |
|-------|------------|--------|------------|
| [阶段] | [问题] | [高/中/低] | [发生原因] |

### 改进机会

| 阶段 | 机会 | 预期影响 | 工作量 |
|-------|-------------|-----------------|--------|
| [阶段] | [想法] | [收益] | [高/中/低] |

---

## Metrics

| Touchpoint | Metric | 当前值 | 目标值 |
|------------|--------|---------|--------|
| [阶段] | [度量] | [值] | [目标] |

---

## 依赖与风险

### 系统依赖

| 系统 | 使用方 | 风险等级 | 缓解措施 |
|--------|---------|------------|------------|
| [系统] | [阶段] | [高/中/低] | [计划] |

### Fail Points

| 阶段 | 失败模式 | 概率 | 影响 | 恢复措施 |
|-------|--------------|-------------|--------|----------|
| [阶段] | [失败内容] | [高/中/低] | [效果] | [如何恢复] |

````

## Service Moments

Service moment 是 blueprint 分析中的重要视角，它关注服务过程中对客户感知影响最大的那些时刻，而不是一视同仁地对待每一步。

### Moment 类型

下表列出了最常见的四种 service moment 类型，识别它们是服务改进排序的前提：

| Moment 类型 | 定义 | 示例 |
|-------------|------------|---------|
| **Moment of Truth** | 塑造认知的关键交互 | 首次接触、支付、交付 |
| **Moment of Waiting** | 客户经历延迟 | 处理中、运输中 |
| **Moment of Failure** | 出现问题 | 错误、缺货 |
| **Moment of Delight** | 超出预期 | 惊喜升级 |

### Moment 分析

下面的数据模型演示了如何对每个 service moment 做结构化分析——通过「重要性」与「当前表现」的差距打分，自动筛选出需要优先改进的项：

```csharp
public class ServiceMoment
{
    public int Stage { get; init; }
    public required string Name { get; init; }
    public required MomentType Type { get; init; }
    public required int ImportanceScore { get; init; } // 1-10
    public required int CurrentPerformance { get; init; } // 1-10
    public decimal GapScore => ImportanceScore - CurrentPerformance;
    public string? Opportunity { get; init; }
}

public enum MomentType
{
    Truth,
    Waiting,
    Failure,
    Delight,
    Routine
}

// 优先级排序：关注高重要性 + 低性能
public static IEnumerable<ServiceMoment> PrioritizeImprovements(
    IEnumerable<ServiceMoment> moments) =>
    moments
        .Where(m => m.ImportanceScore >= 7)
        .OrderByDescending(m => m.GapScore)
        .ThenByDescending(m => m.ImportanceScore);
```

## 跨渠道 Blueprint

在全渠道服务中，客户可能会在 web、mobile、phone、门店之间反复切换，这时就需要把跨渠道的旅程显式绘制出来，重点关注渠道切换时的连续性：

```text
           Web          Mobile        Phone         Store
           ┌────────────────────────────────────────────────┐
阶段 1    │ Research   │ Research   │     -       │   -    │
           ├────────────────────────────────────────────────┤
阶段 2    │ Compare    │ Compare    │     -       │ Browse │
           ├────────────────────────────────────────────────┤
阶段 3    │ Order      │ Order      │     -       │ Order  │
           ├────────────────────────────────────────────────┤
阶段 4    │     -      │ Track      │ Support     │ Pickup │
           ├────────────────────────────────────────────────┤
阶段 5    │ Review     │ Review     │     -       │   -    │
           └────────────────────────────────────────────────┘

渠道转换：
- Web → Mobile: 保存购物车同步
- Mobile → Store: 门店库存检查
- Phone ↔ Any: 案例连续性
```

## .NET Service Blueprint 模型

下面这份 .NET 参考模型把前面介绍过的所有概念聚合到一起，既可以作为文档工具的底层数据结构，也可以直接驱动可视化渲染：

```csharp
public class ServiceBlueprint
{
    public Guid Id { get; init; }
    public required string ServiceName { get; init; }
    public required string Scope { get; init; }
    public required Persona PrimaryPersona { get; init; }
    public required List<BlueprintStage> Stages { get; init; }

    public IEnumerable<FailPoint> GetFailPoints() =>
        Stages.SelectMany(s => s.FailPoints);

    public IEnumerable<BlueprintStage> GetCriticalMoments() =>
        Stages.Where(s => s.IsMomentOfTruth);

    public TimeSpan TotalDuration =>
        TimeSpan.FromTicks(Stages.Sum(s => s.Duration?.Ticks ?? 0));
}

public class BlueprintStage
{
    public int Order { get; init; }
    public required string Name { get; init; }

    // 五个 lane
    public List<PhysicalEvidence> Evidence { get; init; } = [];
    public required CustomerAction CustomerAction { get; init; }
    public required FrontstageAction FrontstageAction { get; init; }
    public List<BackstageAction> BackstageActions { get; init; } = [];
    public List<SupportProcess> SupportProcesses { get; init; } = [];

    // 扩展 lane
    public TimeSpan? Duration { get; init; }
    public EmotionalState? CustomerEmotion { get; init; }
    public List<FailPoint> FailPoints { get; init; } = [];
    public List<string> Metrics { get; init; } = [];
    public bool IsMomentOfTruth { get; init; }
    public string? Owner { get; init; }
}

public class FailPoint
{
    public required string Description { get; init; }
    public required FailureProbability Probability { get; init; }
    public required FailureImpact Impact { get; init; }
    public required string RecoveryProcedure { get; init; }
    public string? PreventionMeasure { get; init; }
}

public enum FailureProbability { 罕见, 偶尔, 频繁 }
public enum FailureImpact { 低, 中, 高, 严重 }

public enum EmotionalState
{
    沮丧,
    焦虑,
    中性,
    满意,
    欣喜
}
```

## Blueprinting Workshop

一份高质量的 blueprint 往往不是某个人闷头写出来的，而是在跨职能 workshop 中通过结构化讨论共同构建的。

### Workshop 议程

下面这份议程模板按 3-4 小时单场 workshop 安排，覆盖从前期准备到最终排序的六个阶段：

```markdown
## Service Blueprinting Workshop

**时长：** 3-4 小时
**参与者：** 跨职能团队（design、product、ops、support）

### Workshop 前
- [ ] 定义 service scope
- [ ] 识别关键 personas
- [ ] 收集现有的 journey maps
- [ ] 准备材料（便利贴、马克笔、模板）

### Workshop 流程

**第一部分：设定背景（30 分钟）**
1. 回顾 service scope 和 persona
2. 分享现有研究/数据
3. 对齐目标

**第二部分：Customer Journey（45 分钟）**
1. 映射 customer actions（便利贴）
2. 排序和优化
3. 识别每个步骤的 channels

**第三部分：Lines of Interaction（30 分钟）**
1. 添加 frontstage actions
2. 绘制 line of visibility
3. 添加 backstage actions

**第四部分：Support Systems（30 分钟）**
1. 识别 technology dependencies
2. 映射 policies 和 partners
3. 记录 training needs

**第五部分：分析（45 分钟）**
1. 标记 fail points
2. 识别 pain points
3. 发现机会
4. 添加 emotional journey
5. 记录 metrics

**第六部分：优先级排序（30 分钟）**
1. 投票确定优先改进项
2. 分配负责人
3. 定义下一步

### 产出物
- [ ] 完成的 service blueprint
- [ ] 优先改进 backlog
- [ ] 带负责人的行动项
```

## 检查清单：Service Blueprint

在产出或评审一张 service blueprint 时，可以用下面这张覆盖准备、要素、分析、验证四个阶段的清单来自检完整度。

### 准备阶段

- [ ] 定义 service scope
- [ ] 识别主要 persona
- [ ] 利益相关者对齐
- [ ] 收集现有数据

### Blueprint 元素

- [ ] 映射所有 customer actions
- [ ] 识别 physical evidence
- [ ] 记录 frontstage actions
- [ ] 记录 backstage actions
- [ ] 识别 support processes
- [ ] 添加时间估算
- [ ] 映射 emotional journey

### 分析

- [ ] 识别 fail points
- [ ] 记录 wait points
- [ ] 记录 pain points
- [ ] 发现机会
- [ ] 定义 metrics

### 验证

- [ ] 跨职能团队审查
- [ ] 与运营团队进行现实检查
- [ ] 客户验证（如果可能）

## 相关 Skills

下面这些相关 skill 经常与 service-blueprinting 协同使用，共同覆盖从客户旅程到流程建模、再到原型落地的完整链路：

- `journey-mapping`（business-analysis）—— 从客户视角绘制 journey map，为 blueprint 提供输入
- `user-research-planning` —— 规划为 blueprinting 提供证据的研究活动
- `prototype-strategy` —— 基于 blueprint 识别的 touchpoint 进行原型迭代
- `process-modeling`（business-analysis）—— 把 backstage 流程进一步用 BPMN 等方法精细建模
