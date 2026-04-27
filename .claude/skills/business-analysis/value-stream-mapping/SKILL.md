---
name: value-stream-mapping
description: Lean value stream mapping for identifying waste and optimization opportunities. Creates current/future state maps with cycle time analysis and improvement recommendations.
allowed-tools: Read, Glob, Grep, Task, Skill
---

# Value Stream Mapping（价值流图）

创建 Lean 价值流图，可视化流动、识别浪费、设计改进机会。基于 Lean 制造原则，并适配到知识工作。

## 什么是 Value Stream？

**value stream（价值流）** 是从请求到交付、向客户交付价值所需的一系列活动。价值流图（VSM）把这个流动可视化，用于识别：

- **增值活动** — 直接创造客户价值的步骤
- **Waste（Muda，浪费）** — 消耗资源却不增值的步骤
- **流动问题** — 瓶颈、延迟与低效

## 8 种 Waste（TIMWOODS）

| Waste | 描述 | 知识工作中的示例 |
|-------|-------------|---------------------------|
| **T**ransportation | 在不同位置之间搬运工作 | 团队间交接、工具切换 |
| **I**nventory | 未完成的工作在排队 | backlog、WIP、排队的请求 |
| **M**otion | 不必要的动作 | 上下文切换、找资料 |
| **W**aiting | 闲置时间 | 审批、阻塞、依赖 |
| **O**verproduction | 做得比需要的多 | 没人用的 feature、过早优化 |
| **O**verprocessing | 做得比必要的多 | 镀金、过量文档 |
| **D**efects | 导致返工的错误 | bug、沟通失误、返工 |
| **S**kills | 人才未被用好 | 可以自动化的手工工作 |

## 工作流

### Phase 1：定义范围

#### Step 1：识别价值流

| 问题 | 答案 |
|----------|--------|
| 我们在交付什么价值？ | [客户产出] |
| 客户是谁？ | [内部/外部客户] |
| 价值流从哪里开始？ | [触发/请求] |
| 到哪里结束？ | [价值已交付] |

#### Step 2：设定边界

```markdown
## Value Stream Definition

**Name:** Customer Order Fulfillment
**Trigger:** Customer places order
**End State:** Customer receives product
**Scope:** Order entry → Shipping (excludes manufacturing)
**Customer:** External retail customer
```

### Phase 2：Current State Map（现状图）

#### Step 1：走查流程

现场观察真实工作（gemba walk）：

- 跟随一个真实的工件走完整个流程
- 记录"真实发生了什么"，而不是"应该发生什么"
- 标注交接点、延迟与绕路

#### Step 2：记录流程步骤

对每个步骤，记录：

| 度量 | 定义 | 测量方式 |
|--------|------------|----------------|
| **Process Time (PT)** | 实际工作时间 | 真正在做事的时间 |
| **Lead Time (LT)** | 总经过时间 | 从开始到结束的墙上时间 |
| **Wait Time (WT)** | 等待时间 | LT - PT |
| **% Complete & Accurate (%C&A)** | 一次合格率 | 无需返工的比例 |

#### Step 3：绘制 Current State Map

```markdown
## Current State Value Stream

| Step | Owner | PT | LT | %C&A | Inventory | Notes |
|------|-------|-----|-----|------|-----------|-------|
| Order Entry | Sales | 15m | 2h | 85% | 50 orders | Manual entry |
| Credit Check | Finance | 10m | 8h | 95% | 30 orders | Batch processing |
| Inventory Alloc | Warehouse | 5m | 4h | 90% | 20 orders | System lookup |
| Pick & Pack | Warehouse | 30m | 6h | 92% | 40 orders | Manual process |
| Ship | Logistics | 10m | 24h | 98% | 100 orders | Carrier pickup |

**Total Process Time:** 70 minutes
**Total Lead Time:** 44 hours
**Flow Efficiency:** 2.7% (PT / LT)
```

### Phase 3：分析 Waste

#### Step 1：计算 Flow Efficiency

```text
Flow Efficiency = Process Time / Lead Time × 100%

Example: 70 min / 2640 min = 2.7%
```

**解读：**

- < 5%：浪费严重（未优化流程的典型值）
- 5-15%：一般效率
- 15-25%：不错
- > 25%：非常好（知识工作中罕见）

#### Step 2：按类型识别 Waste

```markdown
## Waste Analysis

### Waiting (45% of lead time)
- Credit check batch processing: 8 hours → could be real-time
- Carrier pickup schedule: 24 hours → could be on-demand

### Inventory (150 orders WIP)
- Orders queue at each step
- No pull system in place

### Defects (15% rework at order entry)
- Manual data entry errors
- Missing required fields

### Overprocessing
- Full credit check for returning customers
- Duplicate data entry across systems
```

#### Step 3：识别瓶颈

约束（bottleneck）决定了整个系统的产能：

```markdown
## Bottleneck Analysis

**Primary Bottleneck:** Credit Check
- Highest queue (30 orders)
- Batch processing creates 8-hour wait
- Blocks downstream flow

**Secondary Bottleneck:** Pick & Pack
- Manual process
- Variable cycle time
```

### Phase 4：Future State 设计

#### Step 1：套用 Lean 原则

| 原则 | 当前问题 | Future State 方案 |
|-----------|----------------|----------------------|
| **Flow（流动）** | 批处理 | 连续流，单件流 |
| **Pull（拉动）** | 基于预测的推送 | 基于需求的拉动 |
| **Perfection（零缺陷）** | 先接受缺陷、后面再修 | 内建质量 |

#### Step 2：设计改进措施

```markdown
## Future State Design

### Eliminate Waiting
- Real-time credit API vs. batch
- On-demand carrier pickup

### Reduce Inventory
- WIP limits at each step
- Pull signals between steps

### Improve Quality
- Validation at order entry
- Auto-populate from CRM

### Automate Motion Waste
- Single system vs. multiple tools
- API integrations
```

#### Step 3：绘制 Future State Map

```markdown
## Future State Value Stream

| Step | Owner | PT | LT | %C&A | Inventory | Changes |
|------|-------|-----|-----|------|-----------|---------|
| Order Entry | Sales | 10m | 30m | 98% | 10 orders | Validation, auto-fill |
| Credit Check | System | 1m | 5m | 99% | 0 | Real-time API |
| Inventory Alloc | System | 1m | 5m | 99% | 0 | Automated |
| Pick & Pack | Warehouse | 25m | 2h | 96% | 15 orders | Better tooling |
| Ship | Logistics | 10m | 4h | 99% | 20 orders | On-demand pickup |

**Target Process Time:** 47 minutes (33% reduction)
**Target Lead Time:** 6.7 hours (85% reduction)
**Target Flow Efficiency:** 11.7% (4x improvement)
```

### Phase 5：Implementation Roadmap（落地 Roadmap）

#### Step 1：为改进排序

| 改进 | 收益 | 投入 | 优先级 |
|-------------|--------|--------|----------|
| Real-time credit API | 高 (8h → 5m) | 中 | 1 |
| Order entry validation | 中 (错误率 15% → 2%) | 低 | 2 |
| On-demand carrier | 高 (24h → 4h) | 高 | 3 |
| Automated inventory | 中 | 中 | 4 |

#### Step 2：定义 Kaizen 事件

聚焦式的改进 workshop：

```markdown
## Kaizen Event: Credit Check Automation

**Scope:** Eliminate credit check batch processing
**Target:** 8 hours → 5 minutes lead time
**Team:** Finance, IT, Process Owner
**Duration:** 1 week intensive
**Deliverables:**
- API integration specification
- Process redesign
- Training materials
```

## 输出格式

### 叙述式摘要

```markdown
## Value Stream Analysis Summary

**Value Stream:** [Name]
**Date:** [ISO date]
**Analyst:** value-stream-analyst

### Current State Metrics
- **Process Time:** X minutes
- **Lead Time:** Y hours
- **Flow Efficiency:** Z%
- **Primary Bottleneck:** [Step]

### Top Waste Categories
1. **Waiting (X% of LT):** [Description]
2. **Inventory (X orders WIP):** [Description]
3. **[Other waste]:** [Description]

### Future State Targets
- **Lead Time Reduction:** X%
- **Flow Efficiency Target:** Y%
- **Quality Target:** Z% first-time right

### Recommended Actions
1. [High priority improvement]
2. [Medium priority improvement]
3. [Lower priority improvement]
```

### 结构化数据（YAML）

```yaml
value_stream:
  name: "Customer Order Fulfillment"
  version: "1.0"
  date: "{ISO-8601-date}"
  analyst: "value-stream-analyst"

  boundaries:
    trigger: "Customer places order"
    end_state: "Customer receives product"
    customer: "External retail customer"

  current_state:
    total_process_time_minutes: 70
    total_lead_time_hours: 44
    flow_efficiency_percent: 2.7
    wip_total: 240

    steps:
      - name: "Order Entry"
        owner: "Sales"
        process_time_minutes: 15
        lead_time_hours: 2
        complete_accurate_percent: 85
        inventory: 50
        waste_types:
          - type: defects
            description: "Manual entry errors"
          - type: motion
            description: "Multiple system entry"

      - name: "Credit Check"
        owner: "Finance"
        process_time_minutes: 10
        lead_time_hours: 8
        complete_accurate_percent: 95
        inventory: 30
        is_bottleneck: true
        waste_types:
          - type: waiting
            description: "Batch processing"

  waste_analysis:
    - type: waiting
      percent_of_lead_time: 45
      root_causes:
        - "Batch processing"
        - "Scheduled pickups"

    - type: inventory
      total_wip: 240
      root_causes:
        - "No WIP limits"
        - "Push system"

  future_state:
    target_process_time_minutes: 47
    target_lead_time_hours: 6.7
    target_flow_efficiency_percent: 11.7

    improvements:
      - name: "Real-time credit API"
        waste_eliminated: waiting
        impact: high
        effort: medium
        lead_time_reduction_hours: 7.9

  roadmap:
    - phase: 1
      improvements: ["Real-time credit API", "Order validation"]
      duration_weeks: 4

    - phase: 2
      improvements: ["On-demand carrier", "Inventory automation"]
      duration_weeks: 8
```

### Mermaid 图表

**Current State Flow：**

```mermaid
flowchart LR
    subgraph Current State
        A[Order Entry<br/>PT: 15m | LT: 2h<br/>WIP: 50] --> B[Credit Check<br/>PT: 10m | LT: 8h<br/>WIP: 30]
        B --> C[Inventory<br/>PT: 5m | LT: 4h<br/>WIP: 20]
        C --> D[Pick & Pack<br/>PT: 30m | LT: 6h<br/>WIP: 40]
        D --> E[Ship<br/>PT: 10m | LT: 24h<br/>WIP: 100]
    end

    style B fill:#ff9999
```

**Future State Flow：**

```mermaid
flowchart LR
    subgraph Future State
        A[Order Entry<br/>PT: 10m | LT: 30m<br/>WIP: 10] --> B[Credit<br/>API: 1m<br/>WIP: 0]
        B --> C[Inventory<br/>Auto: 1m<br/>WIP: 0]
        C --> D[Pick & Pack<br/>PT: 25m | LT: 2h<br/>WIP: 15]
        D --> E[Ship<br/>PT: 10m | LT: 4h<br/>WIP: 20]
    end

    style B fill:#99ff99
    style C fill:#99ff99
```

## 何时使用

| 场景 | 是否使用 VSM？ |
|----------|--------------------------|
| 流程改进 | 是 — 识别浪费 |
| 新系统设计 | 是 — 围绕 flow 设计 |
| 成本削减 | 是 — 找低效点 |
| lead time 投诉 | 是 — 找瓶颈 |
| 质量问题 | 部分适用 — 配合根因分析 |
| 全新项目 | 也许 — 现状信息有限 |

## 集成

### 上游（Discovery 阶段）

- **process-modeling** — 理解现状流程
- **capability-mapping** — 关联到业务能力
- **stakeholder-analysis** — 识别流程 owner

### 下游

- **Requirements** — 改进需求
- **Systems design** — 自动化机会
- **Project planning** — Kaizen 事件规划

## 相关 skill

- `journey-mapping` — 客户体验视角
- `capability-mapping` — 从能力视角看价值交付
- `root-cause-analysis` — 调查瓶颈根因
- `prioritization` — 为改进 initiative 排优先级
- `process-modeling` — 详细 BPMN 流程图
- `estimation` — 估算改进投入
- `benchmarking` — 现状 vs 目标 对标

## 版本历史

- **v1.0.0**（2025-12-26）：首次发布
