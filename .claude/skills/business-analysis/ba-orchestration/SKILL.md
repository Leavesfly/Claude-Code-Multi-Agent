---
name: ba-orchestration
description: Multi-technique business analysis orchestration using BABOK. Coordinates 14 techniques across strategic analysis, problem solving, planning, and design into comprehensive analysis packages.
allowed-tools: Read, Glob, Grep, Skill
---

# Business Analysis Orchestration（业务分析编排）

将多种业务分析技法编排为一套综合分析方案。基于 BABOK v3 的方法论，讲究把各种技法有效组合起来。

## 概览

业务分析很少只用单一技法。复杂的 initiative 需要把多种技法按适当顺序编排起来，把产出综合成可落地的建议。

## 可用技法（14 个 skill）

### 战略分析

| Skill | 用途 |
|-------|---------|
| `swot-pestle-analysis` | SWOT、PESTLE、波特五力 |
| `business-model-canvas` | BMC、Lean Canvas、9 格分析 |
| `benchmarking` | 竞争分析、行业对标 |

### 问题解决

| Skill | 用途 |
|-------|---------|
| `root-cause-analysis` | Fishbone/Ishikawa、5 Whys |
| `decision-analysis` | 决策表、加权评分、决策树 |
| `risk-analysis` | 风险登记表、概率/影响矩阵 |

### 规划与估算

| Skill | 用途 |
|-------|---------|
| `prioritization` | MoSCoW、Kano 模型、加权评分 |
| `estimation` | 类比法、参数法、PERT、story point |
| `capability-mapping` | 业务能力、成熟度模型 |
| `stakeholder-analysis` | Power/Interest、RACI、沟通计划 |

### 流程与数据设计

| Skill | 用途 |
|-------|---------|
| `value-stream-mapping` | 当前/未来状态、精益分析 |
| `journey-mapping` | 客户体验、touchpoint |
| `process-modeling` | BPMN 记号、workflow |
| `data-modeling` | ERD、数据字典 |

## 技法选型指南

| 目标 | 主要技法 | 辅助技法 |
|------|-------------------|----------------------|
| 战略规划 | SWOT/PESTLE | Capability Mapping、Benchmarking |
| 商业模式设计 | Business Model Canvas | Stakeholder Analysis、Value Stream |
| 流程改进 | Value Stream Mapping | Root Cause Analysis、Process Modeling |
| 客户体验 | Journey Mapping | Stakeholder Analysis、Prioritization |
| 问题调查 | Root Cause Analysis | Risk Analysis、Decision Analysis |
| 技术选型 | Decision Analysis | Benchmarking、Risk Analysis |
| 项目估算 | Estimation | Prioritization、Risk Analysis |
| Backlog 排优先级 | Prioritization | Stakeholder Analysis、Estimation |
| 数据库设计 | Data Modeling | Process Modeling、Capability Mapping |
| 风险管理 | Risk Analysis | Prioritization、Stakeholder Analysis |
| 转型规划 | Capability + VSM | SWOT、Stakeholder Analysis |
| 新 initiative | 全量分析 | 多技法组合包 |
| M&A 尽调 | Capability + Benchmarking | Stakeholder Analysis、Risk |
| 竞争定位 | Benchmarking | SWOT、Business Model Canvas |

## 标准 workflow

### Workflow 1：战略规划

```text
1. Stakeholder Analysis - Identify key stakeholders
2. Capability Mapping - Create capability model
3. Benchmarking - Compare against industry
4. SWOT/PESTLE Analysis - Environmental scan
5. Prioritization - Investment decisions
```

### Workflow 2：客户体验改进

```text
1. Stakeholder Analysis - Identify user segments
2. Journey Mapping - Map current experience
3. Value Stream Mapping - Identify operational waste
4. Capability Mapping - Link to capabilities
5. Prioritization - Impact vs effort matrix
```

### Workflow 3：流程优化

```text
1. Process Modeling - Capture current process (BPMN)
2. Value Stream Mapping - Identify waste and bottlenecks
3. Journey Mapping - User experience perspective
4. Root Cause Analysis - Identify improvement areas
5. Estimation - Plan improvement effort
```

### Workflow 4：全量分析组合包

```text
1. Stakeholder Analysis - Who is involved?
2. Process Modeling - What happens today?
3. Capability Mapping - What do we do?
4. Value Stream Mapping - How efficiently?
5. Journey Mapping - What's the experience?
6. Synthesis - Integrated findings
```

### Workflow 5：战略评估

```text
1. SWOT/PESTLE Analysis - Environmental scan
2. Benchmarking - Competitive position
3. Business Model Canvas - Current model
4. Capability Mapping - Internal strengths
5. Gap Analysis - Strategic gaps
6. Prioritization - Strategic initiatives
```

### Workflow 6：问题调查

```text
1. Stakeholder Analysis - Who's affected?
2. Root Cause Analysis - What's causing it?
3. Risk Analysis - What's at risk?
4. Decision Analysis - What are options?
5. Prioritization - Which solution?
6. Estimation - Implementation effort
```

### Workflow 7：新项目规划

```text
1. Stakeholder Analysis - Who's involved?
2. Risk Analysis - Project risks
3. Estimation - Effort and duration
4. Prioritization - Backlog ordering
5. Process Modeling - Key workflows
6. Data Modeling - Data structures
```

### Workflow 8：数据库/系统设计

```text
1. Process Modeling - Business workflows
2. Data Modeling - Entity relationships
3. Stakeholder Analysis - Data owners
4. Risk Analysis - Data risks
5. Capability Mapping - System capabilities
```

## 编排流程

### 步骤 1：理解目标

先问清楚：

- 需要做什么决策？
- 谁是关键 stakeholder？
- 时间周期是多少？
- 有哪些数据/访问权限？

### 步骤 2：选择 workflow

根据目标，从上面的列表中选择合适的 workflow，或设计自定义的序列。

### 步骤 3：执行技法

对于 workflow 中的每个技法：

1. 加载相应的 skill
2. 收集所需输入
3. 执行该技法
4. 记录输出

### 步骤 4：综合产出

把各项输出综合为一体化分析：

1. **交叉引用 stakeholder** 与能力 owner
2. **把痛点关联到**能力与流程
3. 在所有技法之间**对齐改进点**
4. 按影响与可行性**给建议排优先级**
5. **构建统一的路线图**

## 并行 vs 串行执行

### 何时并行

以下情况可并行执行：

- 技法之间没有数据依赖
- 由不同的 stakeholder/SME 提供输入
- 时间压力要求更快完成

### 何时串行

以下情况应串行执行：

- 某个技法的输出是下一个的输入
- 两者面对同一批 stakeholder（避免疲劳）
- 先前的技法会影响后续的范围

### 并行规划示例

**Phase 1（并行）：**

- Stakeholder Analysis
- Capability Mapping（discovery）

**Phase 2（Phase 1 之后并行）：**

- Value Stream Mapping（使用 stakeholder 上下文）
- Journey Mapping（使用 stakeholder 分析得到的 persona）

**Phase 3（串行）：**

- 综合与整合

## 输出组合包结构

### 一体化分析组合包

```markdown
## Integrated Business Analysis: [Initiative]

### Executive Summary
[Key findings and recommendations]

### Stakeholder Landscape
[From stakeholder-analysis skill]

### Capability Model
[From capability-mapping skill]

### Value Stream Analysis
[From value-stream-mapping skill]

### Customer Experience
[From journey-mapping skill]

### Integrated Findings
[Cross-cutting insights]

### Prioritized Roadmap
[Unified recommendations]

### Appendix: Detailed Outputs
[Full outputs from each technique]
```

## 质量检查清单

编排出的组合包必须包含：

- [ ] 清晰的问题/目标说明
- [ ] 技法选型的理由
- [ ] 每项技法的输出
- [ ] 交叉引用后的发现
- [ ] 一体化的建议
- [ ] 按优先级排序的路线图
- [ ] 交付物之间的可追溯性

## 相关 skill

- `stakeholder-analysis` — Stakeholder 识别与参与
- `capability-mapping` — 业务能力建模
- `value-stream-mapping` — 精益 value stream 分析
- `journey-mapping` — 客户/用户体验映射
- `process-modeling` — BPMN 流程文档
- `data-modeling` — Entity-Relationship 设计
- `root-cause-analysis` — 问题调查
- `swot-pestle-analysis` — 战略环境扫描
- `business-model-canvas` — 商业模式设计
- `prioritization` — MoSCoW、Kano、加权评分
- `risk-analysis` — 风险识别与缓解
- `decision-analysis` — 决策支持技法
- `estimation` — 工作量与工期估算
- `benchmarking` — 竞争分析

## 版本历史

- **v1.0.0**（2026-01-10）：首次发布 — 从 ba-orchestrator agent 中抽取而来
