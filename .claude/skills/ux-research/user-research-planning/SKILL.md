---
name: user-research-planning
description: 规划用户研究 —— 面向 generative 和 evaluative 研究的方法选择、参与者招募、研究设计和研究问题。
allowed-tools: Read, Glob, Grep, Task
---

# User Research Planning（用户研究规划）

规划并设计用户研究，用于收集关于用户、需求、行为和痛点的洞察。

## 何时使用本 skill

在以下场景使用：

- **用户研究规划任务** —— 需要规划用户研究：方法选择、参与者招募、研究设计，以及 generative 和 evaluative 研究的研究问题
- **规划或设计** —— 需要在用户研究规划方面得到指引
- **最佳实践** —— 希望遵循成熟的模式和标准

## 强制：先加载 skill

在回答任何用户研究相关问题前：

2. 使用官方的 UX 研究方法论来源
3. 所有建议都基于成熟的研究实践

## 研究方法选择

### 方法分类

| 类别 | 目的 | 何时使用 |
|----------|---------|-------------|
| **Generative/Discovery** | 理解问题空间、用户需求 | 产品早期开发、问题框定 |
| **Evaluative/Formative** | 测试方案、收集反馈 | 设计过程中、迭代期 |
| **Summative** | 衡量成果、验证 | 发布后、做基准 |

### 研究方法矩阵

| 方法 | 类型 | 样本量 | 时长 | 最适合 |
|--------|------|-------------|------|----------|
| **Contextual Inquiry** | Generative | 5-10 | 每人 2-3 小时 | 在情境中理解 workflow |
| **User Interviews** | Generative | 8-12 | 每人 45-60 分钟 | 深入理解需求/动机 |
| **Diary Studies** | Generative | 10-20 | 1-4 周 | 纵向行为模式 |
| **Ethnographic Observation** | Generative | 5-15 | 整天 | 环境中的自然行为 |
| **Focus Groups** | Generative | 每组 6-10 | 90-120 分钟 | 团体互动、创意产出 |
| **Surveys** | Evaluative | 100+ | 视情况 | 定量验证、覆盖广 |
| **Card Sorting** | Evaluative | 15-30 | 30-60 分钟 | 信息架构 |
| **Tree Testing** | Evaluative | 50+ | 10-20 分钟 | 导航验证 |
| **Usability Testing** | Evaluative | 5-8 | 45-60 分钟 | 界面评估 |
| **A/B Testing** | Summative | 1000+ | 数天-数周 | 大规模对比方案 |

### 方法选型决策树

```text
你想了解什么？
├── 关于用户、他们的需求和情境
│   ├── 在自然环境中 → 情境访谈 / 民族志研究
│   ├── 他们的动机和态度 → 用户访谈
│   └── 随时间变化的行为 → 日记研究
├── 关于你的方案的有效性
│   ├── 他们能完成任务吗？ → 可用性测试
│   ├── 他们能找到内容吗？ → 树测试 / 卡片分类
│   └── 哪个版本表现更好？ → A/B 测试
└── 关于广泛的模式和偏好
    └── 大规模验证 → 问卷调查
```

## 研究设计框架

### 研究方案结构

```markdown
# Research Plan: [Study Name]

## Background
- Business context
- What we know already
- Knowledge gaps

## Research Objectives
1. [Primary objective - what must we learn?]
2. [Secondary objective]
3. [Secondary objective]

## Research Questions
- RQ1: [Specific, answerable question]
- RQ2: [Specific, answerable question]
- RQ3: [Specific, answerable question]

## Methodology
- **Method:** [Selected method with justification]
- **Participants:** [Number and characteristics]
- **Duration:** [Session length, overall timeline]
- **Location:** [Remote/in-person, specific locations]

## Participant Criteria
### Must Have
- [Criterion 1]
- [Criterion 2]

### Nice to Have
- [Criterion 1]

### Exclusions
- [People who should not participate]

## Recruitment
- **Source:** [How we'll find participants]
- **Screener:** [Key screening questions]
- **Incentive:** [Compensation details]

## Materials
- [ ] Screener questionnaire
- [ ] Discussion guide / Protocol
- [ ] Consent forms
- [ ] Recording setup
- [ ] Note-taking templates

## Timeline
| Phase | Dates | Activities |
|-------|-------|------------|
| Setup | Week 1 | Finalize protocol, recruit |
| Fieldwork | Week 2-3 | Conduct sessions |
| Analysis | Week 4 | Synthesize findings |
| Report | Week 5 | Present recommendations |

## Team
- **Lead Researcher:** [Name]
- **Note Taker:** [Name]
- **Stakeholder Observer:** [Name]

## Deliverables
- [ ] Raw notes and recordings
- [ ] Affinity diagram / Themes
- [ ] Research report
- [ ] Presentation to stakeholders
```

## 参与者招募

### 样本量指南

| 方法 | 最低 | 推荐 | 说明 |
|--------|---------|-------------|-------|
| Interviews | 5 | 8-12 | 直到饱和 |
| Usability Tests | 5 | 5-8 | 5 人能发现 85% 的问题 |
| Card Sorts（开放式） | 15 | 30+ | 越多模式越稳定 |
| Card Sorts（封闭式） | 15 | 30+ | 获得统计显著性 |
| Tree Tests | 50 | 100+ | 定量有效性 |
| Surveys | 100 | 300+ | 取决于分析方式 |
| Diary Studies | 10 | 15-20 | 考虑流失率 |

### Screener 问题类型

```csharp
// Example: Screener model for .NET research recruitment system
public class ScreenerResponse
{
    public Guid RespondentId { get; init; }
    public DateTimeOffset CompletedAt { get; init; }
    public required List<ScreenerAnswer> Answers { get; init; }
    public ScreenerResult Result { get; private set; }

    public void Evaluate(ScreenerCriteria criteria)
    {
        var score = 0;
        var disqualified = false;

        foreach (var answer in Answers)
        {
            if (criteria.Disqualifiers.Contains(answer))
            {
                disqualified = true;
                break;
            }

            if (criteria.MustHave.Contains(answer))
                score += 10;
            else if (criteria.NiceToHave.Contains(answer))
                score += 5;
        }

        Result = disqualified
            ? ScreenerResult.Disqualified
            : score >= criteria.MinimumScore
                ? ScreenerResult.Qualified
                : ScreenerResult.NotQualified;
    }
}

public record ScreenerCriteria
{
    public required HashSet<ScreenerAnswer> MustHave { get; init; }
    public required HashSet<ScreenerAnswer> NiceToHave { get; init; }
    public required HashSet<ScreenerAnswer> Disqualifiers { get; init; }
    public required int MinimumScore { get; init; }
}
```

### Screener 最佳实践

**Do：**

- 从泛问题开始，后续再具体
- 纳入行为类问题（过去行为 > 假设情境）
- 加入注意力检查题
- 筛查表达能力（开放题作答）
- 确认参与时间和技术条件

**Don't：**

- 把筛选题放最前面（容易被作答者投其所好）
- 用 double-barreled 问题
- 让筛选标准一眼就看出来
- 只问未来意向

### Screener 示例结构

```markdown
## Screener: [Study Name]

### Section 1: Demographics (General)
1. What is your age range?
2. What is your occupation?

### Section 2: Behavioral Screening
3. How often do you [relevant behavior]?
   - Daily [QUALIFY]
   - Weekly [QUALIFY]
   - Monthly [CONSIDER]
   - Rarely/Never [DISQUALIFY]

4. In the past 3 months, have you [specific action]?
   - Yes, multiple times [QUALIFY]
   - Yes, once [CONSIDER]
   - No [DISQUALIFY]

### Section 3: Experience Level
5. How would you describe your experience with [tool/product]?
   - Expert (3+ years) [SEGMENT A]
   - Intermediate (1-3 years) [SEGMENT B]
   - Beginner (< 1 year) [SEGMENT C]

### Section 4: Articulation Check
6. Please describe a recent time when [relevant scenario].
   What happened and what did you do?
   [Open-ended - evaluate quality of response]

### Section 5: Logistics
7. Are you available for a 60-minute session during [date range]?
8. Do you have access to [required technology]?
9. Are you comfortable with [video recording/screen sharing]?
```

## 研究问题

### SMART 研究问题

研究问题应当：

- **S**pecific：聚焦单一概念
- **M**easurable：可用数据回答
- **A**chievable：在范围和资源内可达
- **R**elevant：与业务/用户需求相关
- **T**ime-bound：控制在研究周期内

### 问题层级

```text
研究目标（高层）
└── 研究问题（研究层面）
    └── 访谈问题（会话层面）
        └── 追问（即时层面）
```

### 示例转换

**业务问题：** 为什么用户没完成结账？

**研究问题：**

- RQ1：用户对结账流程的 mental model 是什么？
- RQ2：在哪些点上用户会感到摩擦或困惑？
- RQ3：用户需要什么信息才有信心完成购买？

**访谈问题：**

- "带我回顾一下你最近一次在线购物的经历。"
- "当你到达结账页面时，你在想什么？"
- "什么情况下你会在结账时放弃购买？"

**追问（Probes）：**

- "多跟我说说那个..."
- "你当时期望发生什么？"
- "那让你有什么感受？"

## 访谈指南结构

### 模板：半结构化访谈

```markdown
# Interview Guide: [Study Name]

## Before Session (5 min)
- [ ] Test recording equipment
- [ ] Review participant background
- [ ] Prepare materials

## Introduction (5 min)
"Hi [Name], thank you for joining us today. I'm [Researcher] and I'll be
facilitating our conversation. [Note-taker] will be taking notes.

We're interested in learning about [topic]. There are no right or wrong
answers—we want to understand your genuine experiences and perspectives.

This session will last about [X] minutes. We'll record for our notes,
but the recording won't be shared outside the research team.

Do you have any questions before we begin?"

## Warm-up (5 min)
- Tell me a bit about yourself and your role.
- How long have you been [relevant context]?

## Core Questions (30-40 min)

### Topic 1: [Theme]
- Question 1 [Primary]
  - Probe: [Follow-up if needed]
  - Probe: [Alternative angle]
- Question 2 [Secondary]

### Topic 2: [Theme]
- Question 3 [Primary]
- Question 4 [Primary]

### Topic 3: [Theme]
- Question 5 [Primary]

## Concept/Prototype Feedback (if applicable, 10 min)
"I'd like to show you something we're considering..."
- What's your first impression?
- What would you expect [feature] to do?
- What questions do you have?

## Wrap-up (5 min)
- Is there anything else you'd like to share?
- Do you have any questions for me?

"Thank you so much for your time today. Your insights are incredibly
valuable. [Incentive details]"

## After Session
- [ ] Quick debrief with note-taker
- [ ] Note immediate impressions
- [ ] Flag key quotes
```

## 研究产出

### 交付物类型

| 产出 | 目的 | 受众 |
|--------|---------|----------|
| **Research Report** | 完整结论 | 整个团队 |
| **Executive Summary** | 关键洞察，1-2 页 | 管理层 |
| **Affinity Diagram** | 整理好的主题 | Research/Design |
| **Journey Map** | 用户体验流程 | Product/Design |
| **Personas** | 用户原型 | 所有团队 |
| **Video Highlights** | 证据与共情 | stakeholder |
| **Recommendations** | 可行动的下一步 | 产品团队 |

### Findings 框架

```markdown
## Finding: [Descriptive Title]

**Theme:** [Category this belongs to]

**Observation:** What we saw/heard
[Factual description of behavior or statement]

**Evidence:**
- P3: "Direct quote supporting observation"
- P7: "Another supporting quote"
- [X/N participants exhibited this]

**Insight:** What it means
[Interpretation - why this matters, what it reveals]

**Implication:** What to do about it
[Recommendation or design opportunity]

**Severity/Opportunity:** [High/Medium/Low]
```

## .NET 研究数据模型

```csharp
// Research study management
public class ResearchStudy
{
    public Guid Id { get; init; }
    public required string Name { get; init; }
    public required ResearchType Type { get; init; }
    public required string Objective { get; init; }
    public required List<string> ResearchQuestions { get; init; }
    public required ParticipantCriteria Criteria { get; init; }
    public required int TargetParticipants { get; init; }
    public required DateRange FieldworkPeriod { get; init; }
    public StudyStatus Status { get; private set; }

    public List<ResearchSession> Sessions { get; } = [];
    public List<Finding> Findings { get; } = [];
}

public enum ResearchType
{
    ContextualInquiry,
    UserInterview,
    DiaryStudy,
    UsabilityTest,
    CardSort,
    TreeTest,
    Survey,
    FocusGroup
}

public record ParticipantCriteria
{
    public required List<string> MustHave { get; init; }
    public required List<string> NiceToHave { get; init; }
    public required List<string> Exclusions { get; init; }
}

public class Finding
{
    public Guid Id { get; init; }
    public required string Theme { get; init; }
    public required string Observation { get; init; }
    public required List<Evidence> Evidence { get; init; }
    public required string Insight { get; init; }
    public required string Implication { get; init; }
    public required Severity Severity { get; init; }
}

public record Evidence(
    Guid ParticipantId,
    string Quote,
    TimeSpan? Timestamp,
    string? Context
);
```

## Checklist：研究规划

### 启动前

- [ ] 明确业务问题
- [ ] stakeholder 对目标达成一致
- [ ] 时间与资源确认
- [ ] 如有必要，走伦理/法务审查

### 研究设计

- [ ] 已选定方法并说明理由
- [ ] 研究问题已记录
- [ ] 样本量已确定
- [ ] 参与者标准已定义
- [ ] screener 已完成

### 材料

- [ ] 讨论指南/protocol
- [ ] 知情同意书
- [ ] 录制设置已测试
- [ ] 笔记模板
- [ ] 刺激物材料（如有）

### 招募

- [ ] 招募渠道已确定
- [ ] screener 已投放
- [ ] 参与者已排期
- [ ] 报酬已安排
- [ ] 确认邮件已发送

### 实地研究中

- [ ] 每场前的 checklist
- [ ] 各场次 protocol 一致
- [ ] 每日复盘已安排
- [ ] 有问题上报机制

### 实地研究后

- [ ] 所有场次已完成
- [ ] 笔记已整理
- [ ] 已定义分析方式
- [ ] 综合分析会已安排
- [ ] 报告时间表已定

## 相关 skill

- `usability-testing` —— 测试设计与执行
- `information-architecture` —— card sorting、tree testing
- `service-blueprinting` —— 服务设计研究
- `journey-mapping`（business-analysis）—— journey research 综合
