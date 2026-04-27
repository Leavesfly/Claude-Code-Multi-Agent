---
name: design-thinking
description: Design Thinking methodology for human-centered innovation. Covers the 5-phase IDEO/Stanford d.school approach (Empathize, Define, Ideate, Prototype, Test) with workshop facilitation and exercise templates.
allowed-tools: Read, Glob, Grep, Task, Skill
---

# Design Thinking（设计思维）

## 何时使用本 skill

在以下场景使用：

- **Design Thinking 任务** — 使用以人为中心的创新方法；遵循 IDEO/Stanford d.school 的五阶段流程（Empathize、Define、Ideate、Prototype、Test），含工作坊引导与练习模板
- **规划或设计** — 需要设计思维方法的指引
- **最佳实践** — 想遵循成熟的模式与标准

## 概览

一种以人为中心的创新方法，把"人的需求"、"技术的可能性"与"商业的成功要求"整合在一起。

## 什么是 Design Thinking？

Design Thinking 是由 IDEO 提出、并在 Stanford d.school 传授的问题解决方法。它强调理解用户、挑战假设、重构问题，并通过迭代的原型与测试来创造创新方案。

### 核心原则

| 原则 | 描述 |
|-----------|-------------|
| **Human-Centered（以人为中心）** | 从人出发，而不是从技术或流程出发 |
| **Embrace Ambiguity（拥抱模糊）** | 在早期对不确定性保持从容 |
| **Bias to Action（行动偏好）** | 靠做来学，而不只是想 |
| **Radical Collaboration（深度协作）** | 跨职能、视角多元 |
| **Iterate Rapidly（快速迭代）** | 快速失败、快速学习、快速改进 |

## 五个阶段

```text
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│         │  │         │  │         │  │         │  │         │
│EMPATHIZE│→│ DEFINE  │→│ IDEATE  │→│PROTOTYPE│→│  TEST   │
│         │  │         │  │         │  │         │  │         │
│Understand│  │Frame the│  │Generate │  │Build to │  │Learn &  │
│the user │  │problem  │  │ideas    │  │learn    │  │refine   │
└─────────┘  └─────────┘  └─────────┘  └─────────┘  └─────────┘
     ▲                                                    │
     └────────────────── Iterate ─────────────────────────┘
```

### 阶段特征

| 阶段 | 模式 | 关键问题 | 产出 |
|-------|------|--------------|--------|
| **Empathize（共情）** | Diverge（发散） | 用户在经历什么？ | 洞察、观察 |
| **Define（定义）** | Converge（收敛） | 核心问题是什么？ | POV statement |
| **Ideate（构想）** | Diverge（发散） | 可能的方案有哪些？ | 创意、概念 |
| **Prototype（原型）** | Converge（收敛） | 如何让它变得具体？ | 原型 |
| **Test（测试）** | Learn（学习） | 哪些管用、哪些不行？ | 经过验证的学习 |

## Phase 1：Empathize（共情）

对用户及其体验建立深入理解。

### 共情方法

| 方法 | 时长 | 参与者 | 适用场景 |
|--------|----------|--------------|----------|
| **User Interviews** | 30-60 分钟 | 1:1 | 深入的个人洞察 |
| **Observation** | 数小时到数天 | 现场 | 自然行为 |
| **Empathy Mapping** | 15-30 分钟 | 工作坊 | 综合调研结果 |
| **Journey Mapping** | 1-2 小时 | 工作坊 | 可视化体验 |
| **Shadowing（跟班观察）** | 半天 | 现场 | 理解工作流 |

### Empathy Map 模板

```markdown
## Empathy Map: [User/Persona Name]

### Says
[Direct quotes from interviews/observation]
- "[Quote 1]"
- "[Quote 2]"

### Thinks
[What might they be thinking? Hopes/fears?]
- [Thought 1]
- [Thought 2]

### Does
[Actions and behaviors observed]
- [Behavior 1]
- [Behavior 2]

### Feels
[Emotional state, what worries/excites them]
- [Emotion 1]
- [Emotion 2]

### Pains
[Frustrations, obstacles, risks]
- [Pain 1]
- [Pain 2]

### Gains
[Wants, needs, measures of success]
- [Gain 1]
- [Gain 2]
```

### 访谈提纲结构

```markdown
## User Interview Guide

**Duration:** 45-60 minutes
**Objective:** [What we want to learn]

### Opening (5 min)
- Introduce yourself and purpose
- Explain confidentiality, recording consent
- "There are no right or wrong answers"

### Context (10 min)
- Tell me about your role/day-to-day
- How long have you been doing [X]?
- Walk me through a typical [scenario]

### Deep Dive (25 min)
- Tell me about a time when [specific experience]
- What was the most challenging part?
- What did you try? What happened?
- How did that make you feel?

### Needs & Desires (10 min)
- If you could wave a magic wand, what would change?
- What would that enable you to do?
- What does success look like for you?

### Closing (5 min)
- Is there anything else you'd like to share?
- Can we follow up if we have questions?
```

## Phase 2：Define（定义）

综合观察结果，界定正确的问题。

### Point of View（POV）陈述

```markdown
## POV Statement

**User:** [Specific user type/persona]

**Need:** [Verb - what they need to do]

**Insight:** [Because - surprising insight about why]

---

**Full Statement:**
[User] needs to [need] because [insight].

**Example:**
A busy working parent needs to quickly prepare healthy meals
because they want to model good eating habits for their children
but feel guilty about the time-cost tradeoff.
```

### How Might We（HMW）问题

把 POV 转化为可行动的机会陈述：

```markdown
## How Might We Questions

**POV:** [Your POV statement]

### HMW Brainstorm

Generate HMW questions by asking:

| Lens | Question | HMW |
|------|----------|-----|
| **Amplify the good** | What's working? | HMW do more of [good thing]? |
| **Remove the bad** | What's painful? | HMW eliminate [pain point]? |
| **Explore the opposite** | What if we flipped it? | HMW [opposite approach]? |
| **Question assumptions** | What's assumed? | HMW if [assumption] wasn't true? |
| **Change status quo** | What's expected? | HMW change when/where/who? |
| **Break it into parts** | What are components? | HMW address just [component]? |

### Selected HMW Questions
1. HMW [question 1]?
2. HMW [question 2]?
3. HMW [question 3]?

**Priority HMW:** [The one to ideate on]
```

## Phase 3：Ideate（构想）

产生尽可能多的创造性方案。

### 构想规则

| 规则 | 描述 |
|------|-------------|
| **Defer judgment** | brainstorm 期间不批评 |
| **Go for quantity** | 数量多 = 质量更好 |
| **Build on others' ideas** | "Yes, and..." |
| **Encourage wild ideas** | 大胆想，不设限 |
| **Be visual** | 动手画，不要只说 |
| **One conversation** | 一次一个对话：倾听、承接、延展 |
| **Stay focused** | 让 HMW 始终可见 |

### Brainstorm 会议模板

```markdown
## Ideation Session

**HMW:** [The How Might We question]
**Duration:** 30-45 minutes
**Participants:** [Names]

### Warm-Up (5 min)
[Quick creative exercise to loosen up]

### Silent Brainstorm (10 min)
- Write one idea per sticky note
- Work individually
- Aim for 10+ ideas each

### Share & Build (15 min)
- Share ideas one by one
- Build on others' ideas
- No criticism, only "Yes, and..."

### Cluster & Theme (10 min)
- Group similar ideas
- Name each cluster
- Identify promising directions

### Ideas Generated
| Cluster | Ideas | Count |
|---------|-------|-------|
| [Theme 1] | [List] | [N] |
| [Theme 2] | [List] | [N] |

### Top Ideas to Prototype
1. [Idea 1]
2. [Idea 2]
3. [Idea 3]
```

### 构想技法

| 技法 | 描述 | 何时使用 |
|-----------|-------------|-------------|
| **Classic Brainstorm** | 自由联想 | 作为起点 |
| **Brainwriting** | 静默写想法 | 偏安静/远程团队 |
| **SCAMPER** | Substitute、Combine、Adapt、Modify、Put to other use、Eliminate、Reverse | 改进现有 |
| **Analogies** | "X 会怎么解决这个？" | 打破心智模型 |
| **Worst Idea** | 故意想坏主意 | 陷入卡壳时 |
| **Mash-up** | 把不相关概念揉在一起 | 激进式创新 |

## Phase 4：Prototype（原型）

做快速、低成本的表达，用于学习。

### 原型哲学

> "If a picture is worth 1,000 words, a prototype is worth 1,000 meetings."
> — IDEO

### 原型类型

| 类型 | 保真度 | 时间 | 适用场景 |
|------|----------|------|----------|
| **Paper Sketch** | 极低 | 分钟级 | 最早期的探索 |
| **Storyboard** | 低 | 30 分钟 | 用户旅程、场景 |
| **Paper Prototype** | 低 | 1-2 小时 | 界面流、UI |
| **Role Play** | 低 | 30 分钟 | 服务体验 |
| **Wizard of Oz** | 中 | 数小时 | 复杂交互 |
| **Clickable Mockup** | 中 | 数天 | 数字产品 |
| **Physical Model** | 视情况 | 视情况 | 实体产品 |

### 原型规划模板

```markdown
## Prototype Plan

**Concept:** [What we're prototyping]
**Goal:** [What we want to learn]
**Audience:** [Who will test it]

### What to Prototype
- **Include:** [Essential elements to test]
- **Exclude:** [What we won't build yet]

### Prototype Type
**Type:** [Paper/Digital/Physical/Role-play]
**Fidelity:** [Low/Medium/High]
**Time to Build:** [Duration]

### Materials Needed
- [Material 1]
- [Material 2]

### Key Questions to Answer
1. [Question 1]
2. [Question 2]
3. [Question 3]

### Success Criteria
- [How we'll know it worked]
```

### Storyboard 模板

```markdown
## Storyboard: [Concept Name]

### Scene 1: [Setup]
[Sketch/Description]
**What's happening:** [Action]
**User says/thinks:** "[Quote]"

### Scene 2: [Trigger]
[Sketch/Description]
**What's happening:** [Action]
**User says/thinks:** "[Quote]"

### Scene 3: [Interaction]
[Sketch/Description]
**What's happening:** [Action]
**User says/thinks:** "[Quote]"

### Scene 4: [Resolution]
[Sketch/Description]
**What's happening:** [Action]
**User says/thinks:** "[Quote]"
```

## Phase 5：Test（测试）

搞清楚什么管用、什么不行，然后迭代。

### 测试原则

| 原则 | 描述 |
|-----------|-------------|
| **Show, don't tell** | 让原型自己说话 |
| **Create experiences** | 让用户真正互动，而不是看 |
| **Ask "why?"** | 深挖反应背后的原因 |
| **Embrace failure** | 每次测试都在教你东西 |
| **Iterate** | build-test-learn 循环 |

### 测试会议模板

```markdown
## Test Session

**Prototype:** [Name/Version]
**Participant:** [Code or name]
**Date:** [ISO date]

### Introduction
"We're testing a concept, not you. There are no wrong answers.
Please think out loud as you interact with this."

### Scenario
[Context to set up the test]

### Observations

| Moment | Observation | Quote | Insight |
|--------|-------------|-------|---------|
| [When] | [What happened] | "[Said]" | [Meaning] |

### Key Learnings
1. [Learning 1]
2. [Learning 2]
3. [Learning 3]

### What to Keep
- [Element that worked]

### What to Change
- [Element to modify]

### Questions Raised
- [New question to explore]
```

### 迭代框架

```markdown
## Iteration Review

**Prototype Version:** [N]
**Tests Conducted:** [Number]

### Synthesis

| Finding | Frequency | Severity | Action |
|---------|-----------|----------|--------|
| [Finding] | [X/N] | H/M/L | Keep/Change/Remove |

### Next Iteration

**Priority Changes:**
1. [Change 1]
2. [Change 2]

**Hypothesis to Test:**
If we [change], then [expected outcome] because [reason].
```

## 工作坊议程模板

### 半日工作坊（4 小时）

```markdown
## Design Thinking Workshop

**Duration:** 4 hours
**Participants:** 6-12 people

### Agenda

**0:00 - 0:15** Welcome & Icebreaker
- Introductions
- Creative warm-up

**0:15 - 0:45** Empathy Share (30 min)
- Share research/interviews
- Empathy mapping exercise

**0:45 - 1:15** Define (30 min)
- Synthesize insights
- Create POV statements
- Generate HMW questions

**1:15 - 1:30** Break (15 min)

**1:30 - 2:15** Ideate (45 min)
- Brainstorming
- Clustering
- Voting

**2:15 - 3:15** Prototype (60 min)
- Teams build low-fi prototypes
- Prepare to present

**3:15 - 3:45** Share & Feedback (30 min)
- Each team presents
- Group feedback

**3:45 - 4:00** Wrap-up (15 min)
- Key decisions
- Next steps
```

### 全日 Sprint（8 小时）

```markdown
## Design Sprint (1-Day)

**Duration:** 8 hours (condensed from 5-day sprint)
**Participants:** 5-8 people cross-functional

### Morning: Understand & Define

**9:00 - 9:30** Set the stage
- Sprint goal
- Challenge statement

**9:30 - 10:30** Expert interviews / Lightning talks
- Stakeholder perspectives
- User insights

**10:30 - 11:00** How Might We
- Generate HMW from insights
- Vote on priority

**11:00 - 12:00** Sketch solutions
- Individual crazy 8s
- Solution sketches

### Afternoon: Build & Test

**12:00 - 1:00** Lunch

**1:00 - 1:30** Decide
- Critique sketches
- Vote
- Decide what to prototype

**1:30 - 4:00** Prototype
- Build testable prototype
- Prepare test script

**4:00 - 5:00** Test
- 3-5 user tests
- Capture feedback

**5:00 - 5:30** Debrief
- What we learned
- Next steps
```

## 与其他方法的集成

### 上游

- **User Research** — 输入到 Empathize 阶段
- **Stakeholder Analysis** — 识别要访谈的对象
- **Strategic Analysis** — 创新所需的商业上下文

### 下游

- **Lean Startup** — Build-Measure-Learn 循环
- **Agile Development** — 迭代式落地
- **Business Model Canvas** — 对方案进行建模

## .NET/C# 示例

把 Design Thinking 应用到软件产品时：

```csharp
// Example: Tracking Design Thinking sessions
public class DesignThinkingSession
{
    public Guid Id { get; init; }
    public required string ChallengeName { get; init; }
    public required DesignPhase CurrentPhase { get; init; }
    public required string HowMightWe { get; init; }
    public DateTimeOffset Created { get; init; }

    public List<EmpathyInsight> Insights { get; } = [];
    public PointOfView? Pov { get; set; }
    public List<Idea> Ideas { get; } = [];
    public List<Prototype> Prototypes { get; } = [];
    public List<TestResult> Tests { get; } = [];
}

public enum DesignPhase
{
    Empathize,
    Define,
    Ideate,
    Prototype,
    Test
}

public record EmpathyInsight(
    string Observation,
    string UserQuote,
    string Interpretation
);

public record PointOfView(
    string User,
    string Need,
    string Insight
);

public record Idea(
    string Title,
    string Description,
    int Votes,
    string Cluster
);
```

## 相关 skill

- `stakeholder-analysis` — 识别访谈对象
- `journey-mapping` — Empathize 阶段的可视化
- `business-model-canvas` — 对方案做建模
- `prioritization` — 选择要做成原型的 idea

## 版本历史

- **v1.0.0**（2025-12-26）：首次发布
