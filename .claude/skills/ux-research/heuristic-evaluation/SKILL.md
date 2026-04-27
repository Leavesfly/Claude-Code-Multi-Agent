---
name: heuristic-evaluation
description: 进行启发式评估 - Nielsen 10 条启发式、严重度评级、专家评审方法、认知走查和可用性检查。
allowed-tools: Read, Glob, Grep, Task
---

# 启发式评估（Heuristic Evaluation）

使用既定的启发式和评估框架进行专家可用性评审。

## 何时使用此技能

在以下场景使用此技能：

- **启发式评估任务** - 进行启发式评估工作 - Nielsen 10 条启发式、严重度评级、专家评审方法、认知走查和可用性检查
- **规划或设计阶段** - 需要关于启发式评估方法的指导
- **最佳实践** - 希望遵循既定的模式和标准

## 强制要求：优先加载技能

在回答任何启发式评估问题之前：

2. 使用 Nielsen Norman Group 和既定的 UX 方法论
3. 所有指导基于经过验证的评估实践

## Nielsen 10 条可用性启发式（Nielsen's 10 Usability Heuristics）

### 1. Visibility of System Status（系统状态可见性）

系统应通过合理的反馈，在合理的时间内始终让用户了解当前正在发生的事情。

**检查要点：**

- 操作期间的加载指示器
- 多步骤流程的进度条
- 操作后的状态消息
- 实时验证反馈
- 清晰的状态指示

**示例：**

| Good | Bad |
|------|-----|
| Spinner during form submission | Page freezes with no feedback |
| "3 of 5 steps complete" | Multi-step form with no progress |
| "Saved successfully" toast | Silent save with no confirmation |

### 2. Match Between System and Real World（系统与真实世界的匹配）

系统应使用用户的语言，采用用户熟悉的词汇、短语和概念。

**检查要点：**

- 使用平实语言，避免行话
- 熟悉的隐喻和图标
- 逻辑的信息顺序
- 文化适宜性
- 领域适当的术语

**示例：**

| Good | Bad |
|------|-----|
| "Save" button | "Persist to datastore" |
| Shopping cart icon | Abstract database icon |
| "Your order" | "Transaction #38291" |

### 3. User Control and Freedom（用户控制与自由）

用户经常会误选系统功能，需要一个清晰标记的"紧急出口"来离开不需要的状态。

**检查要点：**

- 撤销/重做功能
- 工作流中的取消按钮
- 轻松导航返回
- 从模态对话框退出
- Esc 键支持

**示例：**

| Good | Bad |
|------|-----|
| Undo after delete | Immediate permanent deletion |
| "Cancel" in forms | Only "Submit" available |
| X to close modal | Click outside only |

### 4. Consistency and Standards（一致性与标准）

用户不应疑惑不同的词语、情境或操作是否意味着相同的事情。

**检查要点：**

- 一致的术语
- 统一的视觉设计
- 标准的交互模式
- 平台约定
- 可预测的布局

**示例：**

| Good | Bad |
|------|-----|
| All buttons look like buttons | Inconsistent button styles |
| Same action, same location | "Save" in different places |
| Standard icons (gear = settings) | Custom icons without labels |

### 5. Error Prevention（错误预防）

比良好的错误消息更好的是精心设计，防止问题发生。

**检查要点：**

- 破坏性操作的确认
- 输入约束/验证
- 默认安全选项
- 清晰的禁用状态
- 上下文帮助

**示例：**

| Good | Bad |
|------|-----|
| "Are you sure you want to delete?" | Immediate delete on click |
| Date picker (not free text) | Ambiguous date format entry |
| Grayed out unavailable options | Error after selection |

### 6. Recognition Rather Than Recall（识别而非回忆）

通过使对象、操作和选项可见，最大限度地减少用户的记忆负担。

**检查要点：**

- 可见的选项/菜单
- 最近使用的项目
- 自动完成/建议
- 清晰的标签
- 上下文信息

**示例：**

| Good | Bad |
|------|-----|
| Dropdown with options | Must type exact value |
| "Recent searches" | Empty search box |
| Labels on form fields | Placeholder-only inputs |

### 7. Flexibility and Efficiency of Use（灵活性与效率）

加速器——新手用户看不见的功能——通常可以加快专家用户的交互速度。

**检查要点：**

- 键盘快捷键
- 自定义选项
- 高级用户功能
- 批量操作
- 快速操作

**示例：**

| Good | Bad |
|------|-----|
| Ctrl+S to save | Must click menu > Save |
| Drag-and-drop + click | Drag-and-drop only |
| "Select all" checkbox | Must select items one by one |

### 8. Aesthetic and Minimalist Design（美学与极简设计）

对话框不应包含无关或很少需要的信息。

**检查要点：**

- 干净、不杂乱的界面
- 渐进式披露
- 基本信息优先
- 适当的留白
- 清晰的视觉层次

**示例：**

| Good | Bad |
|------|-----|
| Key info prominent | Wall of text |
| "Show more" for details | Everything visible at once |
| Clean forms | 50-field forms on one page |

### 9. Help Users Recognize, Diagnose, and Recover from Errors（帮助用户识别、诊断和恢复错误）

错误消息应使用平实语言表达，准确指出问题，并建设性地提出解决方案。

**检查要点：**

- 平实语言的错误提示
- 具体的问题识别
- 建设性的解决方案
- 清晰的下一步操作
- 错误位置指示

**示例：**

| Good | Bad |
|------|-----|
| "Email format invalid. Example: <name@example.com>" | "Invalid input" |
| Error shown next to field | All errors at top of page |
| "Try again" or "Contact support" | Just an error message |

### 10. Help and Documentation（帮助与文档）

尽管如果系统可以在没有文档的情况下使用会更好，但提供帮助和文档可能是必要的。

**检查要点：**

- 上下文帮助
- 可搜索的文档
- 任务导向的帮助
- 复杂功能的工具提示
- 新用户的入门引导

**示例：**

| Good | Bad |
|------|-----|
| Tooltip on hover | No explanation for icons |
| Inline help text | Separate help document only |
| "What's this?" links | Users must figure it out |

## 严重度评级（Severity Ratings）

### 4 点严重度量表（4-Point Severity Scale）

| 评级 | 严重度 | 定义 | 行动 |
|--------|----------|------------|--------|
| **0** | 不是问题 | 我不认为这是可用性问题 | 无需处理 |
| **1** | Cosmetic（表面问题） | 仅是表面问题；有时间再修复 | 低优先级 |
| **2** | Minor（轻微） | 轻微可用性问题；低优先级修复 | 中优先级 |
| **3** | Major（严重） | 严重可用性问题；需要重要修复 | 高优先级 |
| **4** | Catastrophic（灾难性） | 可用性灾难；发布前必须修复 | 关键/阻塞 |

### 严重度计算（Severity Calculation）

考虑三个因素：

| 因素 | 问题 | 级别 |
|--------|----------|--------|
| **Frequency（频率）** | 问题多久发生一次？ | Rare（罕见）/ Occasional（偶尔）/ Frequent（频繁） |
| **Impact（影响）** | 发生时有多严重？ | Low（低）/ Medium（中）/ High（高） |
| **Persistence（持续性）** | 用户能否轻松克服？ | Easy（容易）/ Moderate（中等）/ Difficult（困难） |

```csharp
public class SeverityAssessment
{
    public required Frequency Frequency { get; init; }
    public required Impact Impact { get; init; }
    public required Persistence Persistence { get; init; }

    public Severity CalculatedSeverity => (Frequency, Impact, Persistence) switch
    {
        (Frequency.Frequent, Impact.High, Persistence.Difficult) => Severity.Catastrophic,
        (Frequency.Frequent, Impact.High, _) => Severity.Major,
        (_, Impact.High, Persistence.Difficult) => Severity.Major,
        (Frequency.Frequent, Impact.Medium, _) => Severity.Major,
        (Frequency.Occasional, Impact.Medium, _) => Severity.Minor,
        (Frequency.Rare, Impact.Low, _) => Severity.Cosmetic,
        _ => Severity.Minor
    };
}

public enum Frequency { Rare, Occasional, Frequent }
public enum Impact { Low, Medium, High }
public enum Persistence { Easy, Moderate, Difficult }
public enum Severity { Cosmetic = 1, Minor = 2, Major = 3, Catastrophic = 4 }
```

## 评估流程（Evaluation Process）

### 步骤 1：规划（Planning）

```markdown
## Heuristic Evaluation Plan

**Product/Feature:** [Name]
**Evaluators:** [List 3-5 evaluators]
**Heuristics:** Nielsen's 10 (or custom set)
**Scope:** [Pages/flows to evaluate]
**Timeline:** [Dates]

### Evaluation Sessions
| Evaluator | Session 1 | Session 2 |
|-----------|-----------|-----------|
| [Name 1] | [Date/Time] | [Date/Time] |
| [Name 2] | [Date/Time] | [Date/Time] |

### Materials
- [ ] Heuristics reference sheet
- [ ] Issue logging template
- [ ] Access to product/prototype
- [ ] Screen recording (optional)
```

### 步骤 2：独立评估（Individual Evaluation）

每位评估员独立评审：

1. **第一遍** - 自由探索，记录初步印象
2. **第二遍** - 针对每条启发式进行系统审查
3. **记录问题** - 记录发现的每个问题

### 步骤 3：问题记录（Issue Logging）

```csharp
public class HeuristicIssue
{
    public Guid Id { get; init; }
    public required string Location { get; init; }
    public required string Description { get; init; }
    public required int HeuristicNumber { get; init; }
    public required string HeuristicName { get; init; }
    public required Severity Severity { get; init; }
    public string? Screenshot { get; init; }
    public string? Recommendation { get; init; }
    public Guid EvaluatorId { get; init; }
}
```

### Issue Template

```markdown
## Issue: [Short Title]

**Location:** [Page/screen/component]
**Heuristic:** #[N] - [Heuristic Name]
**Severity:** [0-4]

### Description
[What is the problem?]

### Evidence
[Screenshot or description of what user sees]

### Impact
[How does this affect users?]

### Recommendation
[Suggested fix]
```

### 步骤 4：整合（Consolidation）

合并所有评估员的发现：

1. 去除重复项
2. 汇总严重度评级
3. 按启发式或位置分组
4. 确定修复优先级

```csharp
public class ConsolidatedIssue
{
    public Guid Id { get; init; }
    public required string Location { get; init; }
    public required string Description { get; init; }
    public required int HeuristicNumber { get; init; }
    public required List<Severity> IndividualRatings { get; init; }
    public Severity AverageSeverity =>
        (Severity)Math.Round(IndividualRatings.Average(s => (int)s));
    public int EvaluatorCount => IndividualRatings.Count;
    public required string Recommendation { get; init; }
}
```

## 报告模板（Report Template）

```markdown
# Heuristic Evaluation Report

## Executive Summary

**Product:** [Name]
**Evaluation Date:** [Date]
**Evaluators:** [Names]
**Total Issues Found:** [N]

### Issues by Severity
| Severity | Count |
|----------|-------|
| Catastrophic (4) | [N] |
| Major (3) | [N] |
| Minor (2) | [N] |
| Cosmetic (1) | [N] |

### Issues by Heuristic
| Heuristic | Count |
|-----------|-------|
| #1 Visibility of System Status | [N] |
| #2 Match with Real World | [N] |
| ... | ... |

---

## Detailed Findings

### Catastrophic Issues

#### Issue 1: [Title]
- **Location:** [Where]
- **Heuristic:** #[N] - [Name]
- **Severity:** 4 (Catastrophic)
- **Evaluators:** [N]/[Total] identified

**Description:**
[Detailed description]

**Screenshot:**
[Image]

**Impact:**
[Effect on users]

**Recommendation:**
[How to fix]

---

[Continue for each issue, grouped by severity]

---

## Recommendations

### Immediate Action (Severity 4)
1. [Action item]
2. [Action item]

### High Priority (Severity 3)
1. [Action item]
2. [Action item]

### Medium Priority (Severity 2)
1. [Action item]

---

## Methodology

This evaluation followed Nielsen's heuristic evaluation methodology with [N] independent evaluators. Each evaluator conducted two passes through the interface, documenting issues against the 10 usability heuristics. Findings were consolidated and severity ratings averaged.

---

## Appendices

### A: Heuristics Reference
[Full heuristics definitions]

### B: Raw Findings by Evaluator
[Individual evaluator notes]
```

## 认知走查（Cognitive Walkthrough）

### 替代检查方法（Alternative Inspection Method）

认知走查专注于新用户完成特定任务时的可学习性。

### 走查问题（Walkthrough Questions）

在任务的每个步骤中，询问：

1. **用户是否会尝试达到正确的效果？**
   - 目标是否明显？
   - 用户知道该做什么吗？

2. **用户是否会注意到正确的操作可用？**
   - 选项是否可见？
   - 标签是否易于理解？

3. **用户是否会将正确的操作与效果关联起来？**
   - 标签是否符合预期？
   - 映射是否合理？

4. **如果执行了正确的操作，用户是否会看到进展？**
   - 是否提供反馈？
   - 系统是否确认成功？

### 走查模板（Walkthrough Template）

```markdown
## Cognitive Walkthrough: [Task Name]

**Task:** [What user is trying to accomplish]
**User Profile:** [Who is this user?]
**Starting Point:** [Where does the task begin?]

### Step 1: [Action]
| Question | Answer | Issue? |
|----------|--------|--------|
| Will user try to achieve right effect? | [Y/N/Maybe] | [Issue if no] |
| Will user notice correct action? | [Y/N/Maybe] | [Issue if no] |
| Will user associate action with effect? | [Y/N/Maybe] | [Issue if no] |
| Will user see progress? | [Y/N/Maybe] | [Issue if no] |

**Notes:** [Additional observations]

### Step 2: [Action]
[Continue pattern...]

---

## Summary

**Success Likelihood:** [High/Medium/Low]
**Key Barriers:**
1. [Barrier 1]
2. [Barrier 2]

**Recommendations:**
1. [Improvement 1]
2. [Improvement 2]
```

## 扩展启发式（Extended Heuristics）

### 额外启发式（Beyond Nielsen's 10）

| 启发式 | 描述 |
|-----------|-------------|
| **Accessibility（可访问性）** | 残障人士可用 |
| **Privacy & Security（隐私与安全）** | 用户感到数据安全 |
| **Personalization（个性化）** | 适应用户偏好 |
| **Memorability（可记忆性）** | 易于记住如何使用 |
| **Satisfaction（满意度）** | 愉快、 enjoyable 的体验 |
| **Efficiency（效率）** | 以最小努力完成任务 |
| **Learnability（可学习性）** | 新用户易于学习 |

### 平台特定启发式（Platform-Specific Heuristics）

#### 移动端启发式（Mobile Heuristics）

- 触摸目标 ≥44px
- 单手可达
- 离线工作
- 尊重系统设置
- 适合上下文

#### Web 应用启发式（Web Application Heuristics）

- 快速加载时间
- 跨浏览器工作
- 响应式设计
- URL 反映状态
- 支持书签/分享

## 检查清单：启发式评估（Checklist: Heuristic Evaluation）

### 规划

- [ ] 范围定义（页面/流程）
- [ ] 确定评估员（3-5 人）
- [ ] 选择启发式
- [ ] 安排产品访问权限
- [ ] 设定时间表

### 评估

- [ ] 完成第一遍（探索）
- [ ] 完成第二遍（系统审查）
- [ ] 记录问题并标注严重度
- [ ] 捕获截图
- [ ] 记录建议

### 整合

- [ ] 收集所有评估员的发现
- [ ] 合并重复项
- [ ] 平均严重度评级
- [ ] 确定问题优先级
- [ ] 起草报告

### 报告

- [ ] 撰写执行摘要
- [ ] 记录问题并提供证据
- [ ] 提供建议
- [ ] 描述方法论
- [ ] 向利益相关者汇报

## 相关技能（Related Skills）

- `usability-testing` - 基于用户的评估
- `accessibility-planning` - 可访问性特定审查
- `design-system-planning` - 一致性评估
- `user-research-planning` - 研究方法选择
