---
name: usability-testing
description: 设计和规划可用性测试 - 任务创建、出声思维协议、主持人脚本、指标定义和分析框架。
allowed-tools: Read, Glob, Grep, Task
---

# Usability Testing（可用性测试）

设计和执行可用性测试，以评估用户使用你的产品完成任务的能力。

## When to Use This Skill（何时使用此技能）

在以下情况使用此技能：

- **可用性测试任务** - 正在设计和规划可用性测试 - 任务创建、出声思维协议、主持人脚本、指标定义和分析框架
- **规划或设计阶段** - 需要关于可用性测试方法的指导
- **最佳实践** - 希望遵循既定的模式和标准

## MANDATORY: Skill Loading First（强制：首先加载技能）

在回答任何可用性测试问题之前：

2. 使用既定的可用性测试方法论来源
3. 基于经过验证的 UX 研究实践提供所有指导

## Usability Testing Types（可用性测试类型）

### Moderated vs Unmoderated（有主持人与无主持人）

| 方面 | Moderated（有主持人） | Unmoderated（无主持人） |
|--------|-----------|-------------|
| **Facilitator（主持人）** | 在场，引导会话 | 缺席，自动化 |
| **Depth（深度）** | 深入洞察，可追问 | 表面层次，聚焦任务 |
| **Sample Size（样本量）** | 通常 5-8 人 | 通常 20-100+ 人 |
| **Cost（成本）** | 较高（主持人时间） | 较低（可扩展） |
| **Turnaround（周转时间）** | 数天至数周 | 数小时至数天 |
| **Best For（适用于）** | 复杂流程、探索性研究 | 验证、基准测试 |

### Testing Formats（测试形式）

| 形式 | 描述 | 使用时机 |
|--------|-------------|-------------|
| **In-Person Moderated（现场有主持人）** | 面对面，受控环境 | 高保真原型、敏感主题 |
| **Remote Moderated（远程有主持人）** | 视频通话，屏幕共享 | 地理多样性、便利性 |
| **Remote Unmoderated（远程无主持人）** | 录制任务，无主持人 | 规模化、快速验证 |
| **Guerrilla Testing（游击测试）** | 在公共场所快速测试 | 早期概念、低预算 |
| **First-Click Testing（首次点击测试）** | 用户首先点击的位置 | 导航、标签设计 |
| **5-Second Test（5秒测试）** | 第一印象 | 视觉层次、信息传达 |
| **Benchmark Testing（基准测试）** | 重复测量 | 跟踪改进情况 |

## Task Design（任务设计）

### Task Writing Principles（任务编写原则）

**好的任务应该：**

- **Realistic（真实）：** 基于实际用户目标
- **Actionable（可执行）：** 有明确的完成状态
- **Specific enough（足够具体）：** 提供必要的上下文
- **Open enough（足够开放）：** 不规定具体路径
- **Measurable（可衡量）：** 可以定义成功标准

### Task Structure Template（任务结构模板）

```markdown
## Task [N]: [Short Name]

**Scenario:**
[上下文设置 - 用户为什么执行此操作]

**Task:**
[要完成什么 - 目标，而非步骤]

**Success Criteria:**
- [ ] [可观察的完成指标]
- [ ] [次要成功指标（如适用）]

**Metrics:**
- 任务成功（二元或分级）
- 任务用时
- 错误/所需协助
- 满意度评分

**Probes (Moderated):**
- What are you thinking right now?
- What did you expect to happen?
- What would you do next?
```

### Task Examples（任务示例）

**Poor Task（差的任务）：**
"Click the hamburger menu, then click Settings, then Privacy, then change your notification preferences." - "点击汉堡菜单，然后点击设置，然后点击隐私，然后更改你的通知偏好设置。"

**Good Task（好的任务）：**
"You've been receiving too many email notifications from this app. Find where you can change your notification settings." - "你从这个应用收到了太多电子邮件通知。找到可以更改通知设置的地方。"

**Poor Task（差的任务）：**
"Test the checkout flow." - "测试结账流程。"

**Good Task（好的任务）：**
"You've been shopping for a birthday gift and found a book you want to purchase. Complete your purchase using the credit card already saved in your account." - "你一直在购买生日礼物，找到了一本你想购买的书。使用账户中已保存的信用卡完成购买。"

### Task Difficulty Progression（任务难度递进）

将任务从易到难进行结构化安排：

1. **Warm-up task（热身任务）** - 简单，建立信心
2. **Primary tasks（主要任务）** - 正在测试的核心功能
3. **Secondary tasks（次要任务）** - 相关但不那么关键
4. **Stretch tasks（延伸任务）** - 边界情况、高级功能

## Think-Aloud Protocol（出声思维协议）

### Concurrent Think-Aloud (CTA)（并发出声思维）

参与者在执行任务时口头表达他们的想法。

**Moderator Prompts（主持人提示语）：**

- "Keep talking..."（继续说...）
- "What are you looking at?"（你在看什么？）
- "What's going through your mind?"（你在想什么？）
- "What do you expect to happen?"（你期望发生什么？）

**Advantages（优势）：**

- 实时洞察思维过程
- 自然捕捉困惑点
- 丰富的定性数据

**Disadvantages（劣势）：**

- 可能减慢任务完成速度
- 对某些参与者来说不自然
- 可能影响行为

### Retrospective Think-Aloud (RTA)（回顾性出声思维）

参与者在事后回顾录像并解释当时的想法。

**Moderator Approach（主持人方法）：**

- 回放录像
- 在关键时刻暂停
- 询问"What were you thinking here?"（你当时在想什么？）

**Advantages（优势）：**

- 不干扰任务执行
- 更准确的时间测量
- 适合复杂/快速任务

**Disadvantages（劣势）：**

- 记忆衰减
- 事后合理化
- 会话时间更长

### Hybrid Approach（混合方法）

- 静默完成任务
- 在自然停顿处进行简短的任务中探针提问
- 任务后汇报问题

## Moderator Script（主持人脚本）

### Session Structure（会话结构）

```markdown
# Usability Test Script: [Product/Feature]

## Pre-Session Setup (10 min before) - 会话前准备（提前10分钟）
- [ ] Test recording equipment - 测试录制设备
- [ ] Verify prototype/product works - 验证原型/产品正常工作
- [ ] Review participant background - 审查参与者背景
- [ ] Prepare task cards/materials - 准备任务卡片/材料
- [ ] Set up note-taking template - 设置笔记模板

## Introduction (5 min) - 介绍（5分钟）

"Hello [Name], thank you for helping us today. I'm [Researcher] and
I'll be guiding our session. [Observer] is taking notes.

We're testing [product], not you. There are no wrong answers or
mistakes—everything you do helps us improve the design.

I'll ask you to complete some tasks and think out loud as you go.
Please share whatever comes to mind—your thoughts, reactions,
questions, even frustrations. There's no need to be polite about
problems you encounter.

We're recording the session to help with our notes. The recording
is confidential and only for our team.

Do you have any questions before we begin?"

## Warm-up Questions (3 min) - 热身问题（3分钟）
- Tell me briefly about your role and typical day. - 简要介绍一下你的角色和典型的一天。
- How familiar are you with [product category]? - 你对[产品类别]有多熟悉？
- What tools do you currently use for [relevant activity]? - 你目前使用什么工具进行[相关活动]？

## Task Introduction - 任务介绍
"I'm going to give you a series of tasks. I'll read each task aloud
and you'll also have it written down. Please read it back to me so
I know we're on the same page.

Remember to think out loud. If you get stuck or would normally give
up, just let me know—we can move on. Ready?"

## Tasks - 任务

### Task 1: [Name] (Warm-up) - 任务1：[名称]（热身）
[Read task aloud, hand written card] - [大声朗读任务，递上书面卡片]

**Observer Notes:** - 观察者笔记：
- Start time: ___ - 开始时间：___
- End time: ___ - 结束时间：___
- Success: [ ] Complete [ ] Partial [ ] Fail - 成功：[ ] 完成 [ ] 部分 [ ] 失败
- Errors: ___ - 错误：___
- Assists: ___ - 协助：___
- Path taken: ___ - 采取的路径：___

**Post-task Questions:** - 任务后问题：
- How difficult was that task? (1-7 scale) - 那个任务有多难？（1-7分制）
- What, if anything, was confusing? - 有什么令人困惑的地方吗？

### Task 2: [Name] (Primary) - 任务2：[名称]（主要）
[Continue pattern...] - [继续模式...]

### Task 3: [Name] (Primary) - 任务3：[名称]（主要）
...

## Post-Test Questions (5 min) - 测试后问题（5分钟）
- What stood out to you about this experience? - 这次体验中什么让你印象深刻？
- What was the most frustrating part? - 最令你沮丧的部分是什么？
- What was the most satisfying part? - 最令你满意的部分是什么？
- How does this compare to [competitor/current solution]? - 这与[竞争对手/当前解决方案]相比如何？
- Would you recommend this to a colleague? Why/why not? - 你会向同事推荐这个吗？为什么/为什么不？

## SUS Questionnaire (3 min) - SUS 问卷（3分钟）
[Administer System Usability Scale] - [执行系统可用性量表]

## Wrap-up (2 min) - 总结（2分钟）
"Thank you so much for your time and feedback. Your insights will
directly influence how we improve [product].

Do you have any final questions for me?

[Explain incentive process]" - [解释激励流程]

## Post-Session - 会话后
- [ ] Save recording - 保存录像
- [ ] Complete observer notes - 完成观察者笔记
- [ ] Note immediate impressions - 记录即时印象
- [ ] Highlight key quotes/moments - 突出显示关键引用/时刻
- [ ] Debrief with observers - 与观察者汇报
```

## Usability Metrics（可用性指标）

### Task-Level Metrics（任务级指标）

| 指标 | 定义 | 测量方式 |
|--------|------------|-------------|
| **Task Success（任务成功）** | 成功完成 | 二元 (0/1) 或分级 (0/0.5/1) |
| **Time on Task（任务用时）** | 完成所需时长 | 秒/分钟 |
| **Errors（错误）** | 犯下的错误 | 计数 |
| **Assists（协助次数）** | 请求帮助的次数 | 计数 |
| **Lostness（迷失度）** | 导航效率 | (N/S) - (S/N)，其中 N=访问的节点数，S=最小节点数 |
| **First Click（首次点击）** | 正确的初始操作 | 二元 |

### Study-Level Metrics（研究级指标）

| 指标 | 公式 | 基准值 |
|--------|---------|-----------|
| **Task Success Rate（任务成功率）** | 成功次数 / 尝试次数 | 平均 78% (Sauro) |
| **Average Time（平均时间）** | 时间总和 / n | 取决于任务 |
| **Error Rate（错误率）** | 错误数 / 任务数 | 越低越好 |
| **SUS Score（SUS 分数）** | 标准化公式 | 68 = 平均水平 |
| **SEQ (Single Ease)（单次易用性）** | 7分制任务后评分 | 5.5 = 平均水平 |
| **SUPR-Q** | 网站 UX 基准 | 百分位排名 |

### System Usability Scale (SUS)（系统可用性量表）

```csharp
// SUS Score calculation
public class SusCalculator
{
    private static readonly string[] Questions =
    [
        "I think that I would like to use this system frequently.",
        "I found the system unnecessarily complex.",
        "I thought the system was easy to use.",
        "I think that I would need the support of a technical person.",
        "I found the various functions well integrated.",
        "I thought there was too much inconsistency.",
        "I imagine most people would learn to use quickly.",
        "I found the system very cumbersome to use.",
        "I felt very confident using the system.",
        "I needed to learn a lot before I could get going."
    ];

    public decimal Calculate(int[] responses)
    {
        if (responses.Length != 10)
            throw new ArgumentException("SUS requires exactly 10 responses");

        // Responses are 1-5 (Strongly Disagree to Strongly Agree)
        decimal score = 0;

        for (int i = 0; i < 10; i++)
        {
            // Odd questions (1,3,5,7,9): score = response - 1
            // Even questions (2,4,6,8,10): score = 5 - response
            score += i % 2 == 0
                ? responses[i] - 1
                : 5 - responses[i];
        }

        // Multiply by 2.5 to get 0-100 scale
        return score * 2.5m;
    }

    public SusInterpretation Interpret(decimal score) => score switch
    {
        >= 85 => SusInterpretation.Excellent,    // Top 10%
        >= 72 => SusInterpretation.Good,         // Top 30%
        >= 68 => SusInterpretation.Average,      // Median
        >= 51 => SusInterpretation.BelowAverage,
        _ => SusInterpretation.Poor
    };
}
```

### Task Success Grading（任务成功分级）

```csharp
public enum TaskSuccessLevel
{
    Complete = 100,      // Completed without assistance
    PartialMinor = 75,   // Completed with minor struggle
    PartialMajor = 50,   // Completed with significant difficulty
    Assisted = 25,       // Required moderator hint
    Failure = 0          // Could not complete
}

public class TaskResult
{
    public required Guid TaskId { get; init; }
    public required Guid ParticipantId { get; init; }
    public required TaskSuccessLevel Success { get; init; }
    public required TimeSpan Duration { get; init; }
    public required int ErrorCount { get; init; }
    public required int AssistCount { get; init; }
    public required int SingleEaseQuestion { get; init; } // 1-7 scale
    public string? Notes { get; init; }
    public List<string> ClickPath { get; init; } = [];
}
```

## Remote Unmoderated Testing（远程无主持人测试）

### Platform Setup Considerations（平台设置考虑因素）

```csharp
// Configuration for unmoderated test
public class UnmoderatedTestConfig
{
    public required string TestName { get; init; }
    public required string WelcomeMessage { get; init; }
    public required List<ScreenerQuestion> Screener { get; init; }
    public required List<UnmoderatedTask> Tasks { get; init; }
    public required List<PostTestQuestion> PostQuestions { get; init; }

    public TestSettings Settings { get; init; } = new()
    {
        RecordScreen = true,
        RecordAudio = true,
        RecordWebcam = false,
        RequireThinkAloud = true,
        MaxTestDuration = TimeSpan.FromMinutes(30),
        AllowTaskSkip = true
    };
}

public class UnmoderatedTask
{
    public required int Order { get; init; }
    public required string Scenario { get; init; }
    public required string TaskInstructions { get; init; }
    public required string PrototypeUrl { get; init; }
    public TimeSpan? TimeLimit { get; init; }
    public bool RequireRecording { get; init; } = true;
    public List<PostTaskQuestion> FollowUp { get; init; } = [];
}
```

### Unmoderated Task Writing Tips（无主持人任务编写技巧）

- **More specific** than moderated (no moderator to clarify) - 比有主持人测试更具体（没有主持人可以澄清）
- **Include context** in the scenario itself - 在场景本身中包含上下文
- **Define clear endpoints** (how do they know they're done?) - 定义明确的终点（他们如何知道自己完成了？）
- **Provide escape hatch** ("If you can't complete this, click 'I'm stuck'") - 提供退出选项（"如果你无法完成此任务，点击'我卡住了'"）

## Analysis Framework（分析框架）

### Session Analysis Template（会话分析模板）

```markdown
# Session Analysis: P[N]

**Participant:** [ID/Code]
**Date:** [日期]
**Duration:** [时长]

## Task Performance

| Task | Success | Time | Errors | Assists | SEQ |
|------|---------|------|--------|---------|-----|
| T1 | ✓ | 1:23 | 0 | 0 | 6 |
| T2 | ~ | 3:45 | 2 | 1 | 4 |
| T3 | ✗ | 5:00+ | 3 | - | 2 |

## Key Observations

### Positive（积极方面）
- [表现良好的地方]

### Issues Found（发现的问题）
1. **[问题名称]** - Severity: [Critical/Major/Minor]
   - Location: [界面中的位置]
   - Behavior: [用户的行为]
   - Quote: "[参与者的口头表达]"
   - Impact: [对任务的影响]

### Notable Quotes（值得注意的引用）
- "[引用]" - Re: [主题]

## Recommendations（建议）
- [立即行动]
- [设计考虑]
```

### Severity Rating Scale（严重程度评级标准）

| 严重程度 | 定义 | 行动 |
|----------|------------|--------|
| **Critical (4)（严重）** | 阻止任务完成 | 发布前必须修复 |
| **Major (3)（主要）** | 造成显著困难 | 发布前应该修复 |
| **Minor (2)（次要）** | 造成轻微犹豫 | 如果可能则修复 |
| **Cosmetic (1)（表面）** | 注意到但不影响任务 | 未来考虑 |

### Rainbow Spreadsheet（彩虹表格）

跨参与者跟踪问题：

```markdown
| Issue | P1 | P2 | P3 | P4 | P5 | Count | Severity |
|-------|----|----|----|----|-------|-----|----------|
| Can't find settings（找不到设置） | X | X |   | X |   | 3/5 | Major |
| Confusing label（标签混淆） | X | X | X | X |   | 4/5 | Major |
| Slow load time（加载缓慢） |   | X |   |   | X | 2/5 | Minor |
```

## .NET Test Management Model（.NET 测试管理模型）

```csharp
public class UsabilityTest
{
    public Guid Id { get; init; }
    public required string Name { get; init; }
    public required UsabilityTestType Type { get; init; }
    public required string ProductVersion { get; init; }
    public required List<UsabilityTask> Tasks { get; init; }
    public required int TargetParticipants { get; init; }

    public List<TestSession> Sessions { get; } = [];
    public List<UsabilityIssue> Issues { get; } = [];

    public UsabilityTestMetrics CalculateMetrics()
    {
        var completedSessions = Sessions.Where(s => s.Status == SessionStatus.Completed);

        return new UsabilityTestMetrics
        {
            TotalParticipants = completedSessions.Count(),
            OverallSuccessRate = CalculateOverallSuccess(completedSessions),
            AverageSusScore = CalculateAverageSus(completedSessions),
            TaskMetrics = Tasks.Select(t => CalculateTaskMetrics(t, completedSessions)).ToList(),
            IssuesBySeverity = Issues.GroupBy(i => i.Severity)
                .ToDictionary(g => g.Key, g => g.Count())
        };
    }
}

public class UsabilityIssue
{
    public Guid Id { get; init; }
    public required string Title { get; init; }
    public required string Description { get; init; }
    public required IssueSeverity Severity { get; init; }
    public required string Location { get; init; }
    public required List<Guid> AffectedParticipants { get; init; }
    public string? Recommendation { get; init; }
    public IssueStatus Status { get; set; } = IssueStatus.Open;
}

public record UsabilityTestMetrics
{
    public required int TotalParticipants { get; init; }
    public required decimal OverallSuccessRate { get; init; }
    public required decimal AverageSusScore { get; init; }
    public required List<TaskMetrics> TaskMetrics { get; init; }
    public required Dictionary<IssueSeverity, int> IssuesBySeverity { get; init; }
}
```

## Checklist: Usability Test Planning（检查清单：可用性测试规划）

### Design Phase（设计阶段）

- [ ] Test objectives defined - 定义测试目标
- [ ] Tasks written and reviewed - 编写并审查任务
- [ ] Success criteria established - 建立成功标准
- [ ] Metrics selected - 选择指标
- [ ] Moderator script drafted - 起草主持人脚本

### Preparation（准备阶段）

- [ ] Prototype/product ready - 原型/产品就绪
- [ ] Recording tools tested - 测试录制工具
- [ ] Participant schedule set - 设定参与者时间表
- [ ] Pilot test completed - 完成试点测试
- [ ] Script refined from pilot - 根据试点测试完善脚本

### Execution（执行阶段）

- [ ] Consistent protocol across sessions - 跨会话保持一致的协议
- [ ] Notes captured during sessions - 在会话期间记录笔记
- [ ] Issues logged as encountered - 遇到问题时立即记录
- [ ] Daily debriefs conducted - 进行每日汇报

### Analysis（分析阶段）

- [ ] All sessions reviewed - 审查所有会话
- [ ] Metrics calculated - 计算指标
- [ ] Issues compiled and rated - 汇总并评级问题
- [ ] Findings synthesized - 综合发现
- [ ] Recommendations prioritized - 优先级排序建议

## Related Skills（相关技能）

- `user-research-planning` - 整体研究规划
- `heuristic-evaluation` - 专家审查方法
- `accessibility-planning` - 包容性测试实践
- `prototype-strategy` - 用于测试的原型保真度

**Last Updated:** 2025-12-27
