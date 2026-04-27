---
name: estimation
description: Estimation techniques including analogous, parametric, three-point, and expert judgment methods. Provides effort, cost, duration, and complexity estimates for projects, features, and tasks.
allowed-tools: Read, Glob, Grep, Task, Skill
---

# Estimation（估算）

## 何时使用本 skill

在以下场景使用：

- **Estimation 任务** — 使用 analogous、parametric、三点估算、专家判断等方法；为项目、功能、任务提供工作量、成本、工期与复杂度的估算
- **规划或设计** — 需要估算方法的指引
- **最佳实践** — 想遵循成熟的模式与标准

## 概览

使用经验证的估算技术，系统性地估算工作量、成本、工期与复杂度。支持 analogous、parametric、三点（PERT）、专家判断等方法，适用于项目、功能和任务。

## 什么是 Estimation？

**Estimation（估算）** 是对完成工作所需的资源、时间与成本进行预测的过程。良好的估算需平衡：

- **Accuracy（准确性）**：估算值与实际结果的接近程度
- **Precision（精确性）**：估算的一致性
- **Speed（速度）**：得出估算的时间
- **Communication（沟通）**：对不确定性的共识

### Estimate vs Commitment

| 概念 | 定义 | 用途 |
|---------|------------|-----|
| **Estimate（估算）** | 基于当前知识的最佳猜测 | 规划、预测 |
| **Commitment（承诺）** | 按日期/成本交付的承诺 | 合同、截止 |
| **Target（目标）** | 期望达成的结果 | 目标、OKR |

**关键原则：** 估算是带不确定性的区间，而不是单点保证。

## 估算技术

### Analogous Estimation（类比估算，Top-Down）

基于相似的历史工作进行估算：

| 步骤 | 动作 |
|------|--------|
| 1 | 找到相似的已完成项目/功能 |
| 2 | 取出实际的工作量/成本/工期 |
| 3 | 针对差异（复杂度、团队、技术）做调整 |
| 4 | 套用调整系数 |

**公式：**

```text
New Estimate = Historical Actual × Adjustment Factor

Example:
Similar project took 200 hours
New project is ~20% more complex
Estimate = 200 × 1.20 = 240 hours
```

**何时使用：** 项目早期、细节有限、具备历史数据的成熟团队

**准确性：** ±25-50%（有良好历史数据会更准）

### Parametric Estimation（参数估算）

基于统计关系进行估算：

| 要素 | 描述 |
|---------|-------------|
| **Unit of Work** | 可度量的工作单元（screen、API、table） |
| **Productivity Rate** | 来自历史数据的单位工作量 |
| **Quantity** | 需要产出的单元数量 |

**公式：**

```text
Estimate = Quantity × Productivity Rate

Example:
10 API endpoints × 16 hours/endpoint = 160 hours
```

**常见生产率指标：**

| 工作类型 | 指标 | 典型区间 |
|-----------|--------|---------------|
| UI screens | 小时/screen | 8-40 小时 |
| API endpoints | 小时/endpoint | 4-24 小时 |
| 数据库表 | 小时/table | 4-16 小时 |
| Test case | 小时/test | 0.5-4 小时 |
| 文档页 | 小时/page | 2-8 小时 |

**何时使用：** 重复性工作、有良好历史数据、技术相似

**准确性：** ±15-25%（使用经过校准的速率）

### Three-Point Estimation（PERT 三点估算）

用乐观、最可能、悲观三个值进行估算：

| 取值 | 符号 | 定义 |
|-------|--------|------------|
| **Optimistic（乐观）** | O | 最好情况，一切顺利 |
| **Most Likely（最可能）** | M | 最可能出现的结果 |
| **Pessimistic（悲观）** | P | 最坏情况，出现问题 |

**PERT 公式（加权平均）：**

```text
Expected = (O + 4M + P) / 6
Standard Deviation = (P - O) / 6

Example:
O = 5 days, M = 8 days, P = 17 days
Expected = (5 + 4×8 + 17) / 6 = 9 days
Std Dev = (17 - 5) / 6 = 2 days
```

**置信区间：**

| 置信度 | 计算 | 示例（E=9, SD=2） |
|------------|-------------|---------------------|
| 68% | E ± 1 SD | 7-11 天 |
| 95% | E ± 2 SD | 5-13 天 |
| 99.7% | E ± 3 SD | 3-15 天 |

**何时使用：** 不确定的工作、新技术、需要传达风险

**准确性：** 提供显式的不确定性区间

### Expert Judgment（专家判断）

基于集体专家知识进行估算：

**Wideband Delphi 流程：**

| 轮次 | 活动 |
|-------|----------|
| 1 | 专家独立估算 |
| 2 | 收集并匿名共享估算 |
| 3 | 讨论过高/过低的离群值，分享理由 |
| 4 | 再次独立估算 |
| 5 | 重复直到收敛（或取均值） |

**Planning Poker（敏捷）：**

| 步骤 | 动作 |
|------|--------|
| 1 | 展示要估算的 item |
| 2 | 简短讨论（2-5 分钟） |
| 3 | 每位成员私下出牌 |
| 4 | 同时揭示 |
| 5 | 讨论离群值 |
| 6 | 重新投票直到达成共识 |

**常见量表：**

| 量表类型 | 取值 |
|------------|--------|
| Fibonacci | 1, 2, 3, 5, 8, 13, 21, 34, 55, 89 |
| Modified Fibonacci | 0, 0.5, 1, 2, 3, 5, 8, 13, 20, 40, 100 |
| T-shirt | XS, S, M, L, XL, XXL |
| 2 的幂 | 1, 2, 4, 8, 16, 32, 64 |

**何时使用：** 复杂工作、需要多视角、团队对齐

### Function Point Analysis（功能点分析）

基于功能规模估算（适合较大系统）：

| 组件 | 描述 | 权重区间 |
|-----------|-------------|--------------|
| External Inputs | 进入系统的数据 | 3-6 |
| External Outputs | 离开系统的数据 | 4-7 |
| External Inquiries | 只读查询 | 3-6 |
| Internal Files | 逻辑数据存储 | 7-15 |
| External Interfaces | 共享数据 | 5-10 |

**流程：**

1. 统计各类组件的数量
2. 划分复杂度（低/中/高）
3. 套用权重
4. 计算未调整功能点
5. 套用技术复杂度系数

**何时使用：** 大型项目、正式合同、行业对标

## 相对估算

### Story Points（故事点）

相对复杂度/工作量度量：

| 点数 | 相对规模 | 示例 |
|--------|---------------|---------|
| 1 | 琐碎 | 修笔误、改配置 |
| 2 | 简单 | 简单 bug fix、小 feature |
| 3 | 中等 | 标准 feature，中等复杂度 |
| 5 | 复杂 | 多组件 feature |
| 8 | 非常复杂 | 集成类工作、不确定性高 |
| 13 | Epic 级 | 考虑拆分 |
| 21+ | 过大 | 必须拆分 |

**基线：** 选一个大家都理解的 story 作参照（例如"这个是 3 点"）

**Velocity（速度）：** 每迭代完成的 story point（用于预测）

### T-Shirt Sizing（T 恤码估算）

roadmap 规划的快速相对估算：

| 尺码 | 工作量区间 | 工期区间 |
|------|--------------|----------------|
| XS | 1-4 小时 | < 1 天 |
| S | 0.5-2 天 | 1-2 天 |
| M | 2-5 天 | 3-5 天 |
| L | 1-2 周 | 1-2 周 |
| XL | 2-4 周 | 2-4 周 |
| XXL | 1-3 月 | 过大，需拆分 |

## 工作流

### Phase 1：准备

#### Step 1：厘清范围

```markdown
## Estimation Request

**Item:** [What's being estimated]
**Requester:** [Who needs the estimate]
**Purpose:** [Planning/budgeting/commitment]
**Deadline:** [When estimate is needed]
**Precision:** [ROM/Budget/Definitive]

### Scope Definition

- **In Scope:** [What's included]
- **Out of Scope:** [What's excluded]
- **Assumptions:** [Key assumptions]
- **Constraints:** [Known constraints]
```

#### Step 2：选择估算技术

| 情境 | 推荐技术 |
|-----------|----------------------|
| 项目早期 | Analogous + T-shirt sizing |
| 需求细化 | Parametric + 三点估算 |
| 敏捷 backlog | Story point + Planning poker |
| 新技术/新领域 | Expert judgment + 三点估算 |
| 合同/预算 | Function point + Parametric |

### Phase 2：估算

#### Step 1：分解工作

拆解到可估算的单元（半天到 2 周的 chunk）

#### Step 2：应用技术

使用上述选定的技术

#### Step 3：加上 contingency

```markdown
## Contingency Calculation

| Risk Level | Contingency | When to Use |
|------------|-------------|-------------|
| Low | 10-15% | Well-understood, experienced team |
| Medium | 20-30% | Some unknowns, new team members |
| High | 40-50% | Significant unknowns, new technology |
| Very High | 75-100% | Research, innovation, first-of-kind |
```

#### Step 4：校验

- 与相似工作做 sanity check
- 与团队复核
- 检查是否漏项
- 验证假设

### Phase 3：沟通

#### Step 1：以区间表达

```markdown
## Estimate Summary

**Effort:** 160-200 hours (confidence: 80%)
**Duration:** 4-5 weeks (with 2 developers)
**Cost:** $24,000-$30,000

**Key Risks:**
- API integration complexity unknown
- Dependency on third-party availability
```

#### Step 2：记录假设

```markdown
## Estimation Assumptions

1. Requirements are stable and complete
2. Team has 80% availability (20% overhead)
3. No major technology changes
4. Dependencies delivered on time
5. [Additional assumptions...]

**If assumptions change, estimate should be revisited.**
```

## 输出格式

### 叙述式摘要

```markdown
## Estimation Summary

**Item:** [Feature/Project name]
**Date:** [ISO date]
**Estimator:** estimation-analyst
**Technique:** [Technique used]

### Estimate

| Dimension | Low | Expected | High | Confidence |
|-----------|-----|----------|------|------------|
| Effort | 120h | 160h | 220h | 80% |
| Duration | 4w | 5w | 7w | 80% |
| Cost | $18K | $24K | $33K | 80% |

### Basis of Estimate

- **Historical Reference:** [Similar past work]
- **Productivity Rate:** [If parametric]
- **Expert Input:** [Who contributed]

### Assumptions

1. [Assumption 1]
2. [Assumption 2]

### Risks Affecting Estimate

| Risk | Impact on Estimate |
|------|-------------------|
| [Risk 1] | +20% if occurs |
| [Risk 2] | +15% if occurs |

### Recommendations

1. [Next steps for refining estimate]
2. [When to re-estimate]
```

### 结构化数据（YAML）

```yaml
estimation:
  version: "1.0"
  date: "2025-01-15"
  item: "User Dashboard Feature"
  estimator: "estimation-analyst"
  technique: "three_point"

  scope:
    description: "Interactive user dashboard with analytics"
    in_scope:
      - "Dashboard UI components"
      - "Data visualization"
      - "User preferences"
    out_of_scope:
      - "Backend analytics engine"
      - "Real-time updates"
    assumptions:
      - "API endpoints available"
      - "Design mockups complete"

  estimates:
    effort:
      optimistic: 120
      most_likely: 160
      pessimistic: 240
      expected: 166
      unit: "hours"
      std_deviation: 20
    duration:
      optimistic: 3
      most_likely: 4
      pessimistic: 6
      expected: 4.2
      unit: "weeks"
    cost:
      expected: 24000
      range_low: 18000
      range_high: 36000
      currency: "USD"

  confidence: 0.80
  contingency: 0.20

  breakdown:
    - component: "UI Components"
      effort: 60
      technique: "parametric"
      rate: "15h/component"
      quantity: 4
    - component: "Data Integration"
      effort: 40
      technique: "analogous"
      reference: "PROJ-123"
    - component: "Testing"
      effort: 40
      technique: "percentage"
      percentage: 0.25

  risks:
    - risk: "API complexity higher than expected"
      probability: 0.3
      impact_hours: 30
    - risk: "Design changes during development"
      probability: 0.2
      impact_hours: 20

  historical_comparison:
    similar_item: "Admin Dashboard"
    actual_effort: 180
    adjustment_factor: 0.9
```

### 分解表

```markdown
## Effort Breakdown

| Component | Technique | Estimate | Contingency | Total |
|-----------|-----------|----------|-------------|-------|
| UI Components | Parametric | 60h | 12h | 72h |
| Data Integration | Analogous | 40h | 10h | 50h |
| Testing | % of Dev | 40h | 8h | 48h |
| Documentation | Parametric | 16h | 4h | 20h |
| **Subtotal** | | **156h** | **34h** | **190h** |
| Management Overhead | 10% | | | 19h |
| **Total** | | | | **209h** |
```

## 估算的准确性

### Cone of Uncertainty（不确定性锥）

随着项目推进，估算准确度会提升：

| 阶段 | 准确区间 |
|-------|----------------|
| 初始概念 | 0.25x - 4x |
| 立项后 | 0.5x - 2x |
| 需求完成 | 0.67x - 1.5x |
| 详细设计 | 0.8x - 1.25x |
| 代码完成 | 0.9x - 1.1x |

**启示：** 早期估算需要更宽的区间；随着认知增长再细化。

### 估算层次

| 层次 | 准确度 | 使用时机 |
|-------|----------|-----------|
| **ROM**（Rough Order of Magnitude，粗量级） | -25% 到 +75% | 初步预算 |
| **Budget**（预算级） | -10% 到 +25% | 项目审批 |
| **Definitive**（确定级） | -5% 到 +10% | 执行基线 |

## 常见陷阱

| 陷阱 | 如何规避 |
|---------|------------|
| 给单点估算 | 始终给区间 |
| 乐观偏见 | 用历史数据、加 contingency |
| 锚定效应 | 在看到他人估算前先独立估算 |
| 规划谬误 | 纳入现实的开销与风险 |
| 范围蔓延 | 记录假设，范围变更时重新估算 |
| 精度表演 | 让精度匹配实际认知 |

## 集成

### 上游

- **Requirements** — 被估算的对象
- **risk-analysis** — 影响估算的风险
- **stakeholder-analysis** — 谁需要估算、需要什么精度

### 下游

- **Project planning** — 资源分配、排期
- **Budgeting** — 成本预测
- **prioritization** — 为 value/effort 分析提供成本输入

## 相关 skill

- `risk-analysis` — 影响估算的风险
- `prioritization` — 用估算做优先级排序
- `decision-analysis` — 权衡决策
- `value-stream-mapping` — 估算流程改进的工作量

## 版本历史

- **v1.0.0**（2025-12-26）：首次发布
