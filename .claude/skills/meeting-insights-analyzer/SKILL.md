---
name: meeting-insights-analyzer
description: 分析会议记录和录音，以发现行为模式、沟通洞察和可行的反馈。识别您何时避免冲突、使用填充词、主导对话或错过倾听的机会。非常适合寻求提高沟通和领导技能的专业人士。
---

# Meeting Insights Analyzer（会议洞察分析器）

本 skill 把你的会议转录转成关于沟通模式的可行动洞察，帮你成为更有效的沟通者和领导者。

## 何时使用本 skill

- 跨多个会议分析你的沟通模式
- 获得关于你领导力与引导风格的反馈
- 识别你何时回避困难对话
- 理解你的发言习惯和口头禅
- 随时间追踪沟通能力的提升
- 用具体例子为绩效评估做准备
- 辅导团队成员改进沟通风格

## 本 skill 能做什么

1. **模式识别**：识别跨会议反复出现的行为，比如：
   - 回避冲突或间接沟通
   - 发言占比与轮流发言
   - 提问 vs. 陈述的模式
   - 主动倾听的信号
   - 决策方式

2. **沟通分析**：评估沟通的有效性：
   - 清晰度与直接程度
   - 口头禅和含糊用语的使用
   - 语气与情绪模式
   - 会议掌控与引导

3. **可行动反馈**：提供带时间戳的具体例子，包含：
   - 发生了什么
   - 为什么重要
   - 如何改进

4. **趋势追踪**：分析多个会议时对比模式随时间的变化

## 用法

### 基础准备

1. 把会议转录下载到一个文件夹（如 `~/meetings/`）
2. 在 Claude Code 里进到该文件夹
3. 提出你想要的分析

### 快速入门示例

```
Analyze all meetings in this folder and tell me when I avoided conflict.
```

```
Look at my meetings from the past month and identify my communication patterns.
```

```
Compare my facilitation style between these two meeting folders.
```

### 进阶分析

```
Analyze all transcripts in this folder and:
1. Identify when I interrupted others
2. Calculate my speaking ratio
3. Find moments I avoided giving direct feedback
4. Track my use of filler words
5. Show examples of good active listening
```

## 操作说明

当用户请求会议分析时：

1. **发现可用数据**
   - 扫描文件夹中的转录文件（.txt、.md、.vtt、.srt、.docx）
   - 检查文件是否包含说话人标签和时间戳
   - 确认会议的时间范围
   - 在转录中识别出用户的姓名/标识

2. **澄清分析目标**
   
   如果没有明确指定，问清用户想了解什么：
   - 具体行为（回避冲突、打断他人、口头禅）
   - 沟通有效性（清晰、直接、倾听）
   - 会议引导能力
   - 发言模式与占比
   - 待改进的成长方向
   
3. **模式分析**

   针对每项被请求的洞察：
   
   **回避冲突**：
   - 寻找含糊用语（"maybe"、"kind of"、"I think"）
   - 用间接措辞而非直接请求
   - 有张力时转移话题
   - 表面同意但不承诺（"yeah, but..."）
   - 对明显的问题避而不谈
   
   **发言占比**：
   - 计算发言时长占会议的百分比
   - 统计打断（用户打断他人与被他人打断）
   - 测量平均每次发言的时长
   - 追踪提问 vs. 陈述的比例
   
   **口头禅**：
   - 统计 "um"、"uh"、"like"、"you know"、"actually" 等
   - 记录每分钟或每次发言的出现频率
   - 识别哪些情境下会增多（紧张、不确定）
   
   **主动倾听**：
   - 引用他人此前观点的提问
   - 复述或总结他人的想法
   - 在他人贡献基础上延伸
   - 提出澄清性问题
   
   **领导力与引导**：
   - 决策方式（命令式 vs. 协作式）
   - 如何处理分歧
   - 是否纳入发言较少的参与者
   - 时间管理与议程掌控
   - 后续跟进与行动项的清晰度

4. **提供具体示例**

   对于发现的每个模式，包含：
   
   ```markdown
   ### [Pattern Name]
   
   **Finding**: [One-sentence summary of the pattern]
   
   **Frequency**: [X times across Y meetings]
   
   **Examples**:
   
   1. **[Meeting Name/Date]** - [Timestamp]
      
      **What Happened**:
      > [Actual quote from transcript]
      
      **Why This Matters**:
      [Explanation of the impact or missed opportunity]
      
      **Better Approach**:
      [Specific alternative phrasing or behavior]
   
   [Repeat for 2-3 strongest examples]
   ```

5. **综合洞察**

   分析完所有模式后，给出：
   
   ```markdown
   # Meeting Insights Summary
   
   **Analysis Period**: [Date range]
   **Meetings Analyzed**: [X meetings]
   **Total Duration**: [X hours]
   
   ## Key Patterns Identified
   
   ### 1. [Primary Pattern]
   - **Observed**: [What you saw]
   - **Impact**: [Why it matters]
   - **Recommendation**: [How to improve]
   
   ### 2. [Second Pattern]
   [Same structure]
   
   ## Communication Strengths
   
   1. [Strength 1 with example]
   2. [Strength 2 with example]
   3. [Strength 3 with example]
   
   ## Growth Opportunities
   
   1. **[Area 1]**: [Specific, actionable advice]
   2. **[Area 2]**: [Specific, actionable advice]
   3. **[Area 3]**: [Specific, actionable advice]
   
   ## Speaking Statistics
   
   - Average speaking time: [X% of meeting]
   - Questions asked: [X per meeting average]
   - Filler words: [X per minute]
   - Interruptions: [X given / Y received per meeting]
   
   ## Next Steps
   
   [3-5 concrete actions to improve communication]
   ```

6. **提供后续选项**
   - 在未来的会议中继续追踪同样的指标
   - 深挖某场具体会议或某个模式
   - 与行业基准对比
   - 制定个人沟通发展计划
   - 生成一份绩效评估用的总结

## 示例

### 示例 1：回避冲突分析（灵感来自 Dan Shipper）

**User**："I download all of my meeting recordings and put them in a folder. Tell me all the times I've subtly avoided conflict."

**输出**：
```markdown
# Conflict Avoidance Patterns

Found 23 instances across 15 meetings where you used indirect 
communication or avoided addressing tensions directly.

## Pattern: Hedging on Critical Feedback

**Frequency**: 8 times across 7 meetings

**Example 1: 1:1 with Sarah** - 00:14:32

**What Happened**:
> "So, I was thinking... maybe we could, like, potentially 
> consider looking at the timeline again? I mean, if you 
> think that makes sense. But whatever you think is best!"

**Why This Matters**:
You needed to address that Sarah's project was behind schedule, 
but the hedging language ("maybe", "potentially", "if you think") 
and the deflection at the end ("whatever you think") made it easy 
for her to miss the urgency.

**Better Approach**:
"Sarah, the project is two weeks behind schedule. We need to 
discuss what's blocking progress and create a new timeline 
today."

[Additional examples...]

## Recommendations

1. **Name the issue directly** in the first sentence
2. **Remove hedging words** like "maybe," "kind of," "sort of"
3. **Ask specific questions** instead of hinting
4. **Schedule difficult conversations** instead of raising them casually
```

### 示例 2：领导力引导

**User**："Analyze my team meetings and tell me about my facilitation style."

**输出**：提供以下维度的洞察：
- 你 vs. 团队成员的发言占比（60% vs. 40%）
- 你是提问还是陈述（3:1 的比例）
- 你如何处理分歧（倾向于过快结论）
- 谁发言最少、你是否把他们拉进来
- 引导得好和错过的引导时刻的示例

### 示例 3：个人成长追踪

**User**："Compare my meetings from Q1 vs. Q2 to see if I've improved my listening skills."

**输出**：生成对比分析，展示：
- 打断次数下降（每场 8 次 → 每场 3 次）
- 澄清性提问增加（每场 2 次 → 7 次）
- 在他人想法上延伸的能力提升
- 展示差异的具体例子
- 仍需成长的领域

## 准备工作建议

### 获取会议转录

**从 Granola**（Lenny's Newsletter 订阅免费送）：
- Granola 自动转录你的会议
- 把转录导出到一个文件夹：[具体操作说明]
- 把 Claude Code 指向该文件夹

**从 Zoom**：
- 开启云录制与转录
- 会后下载 VTT 或 SRT 文件
- 存到一个专门的文件夹

**从 Google Meet**：
- 使用 Google Docs 自动转录
- 把转录文档保存到一个文件夹
- 以 .txt 下载或授予 Claude Code 访问权

**从 Fireflies.ai、Otter.ai 等**：
- 批量导出转录
- 存到本地文件夹
- 对该文件夹做分析

### 最佳实践

1. **命名一致**：用 `YYYY-MM-DD - Meeting Name.txt` 格式
2. **定期分析**：按月或季度回顾趋势
3. **具体提问**：一次只问一个行为，能挖得更深
4. **隐私**：把敏感会议数据留在本地
5. **行动导向**：一次只聚焦一个改进方向

## 常见分析请求

- "When do I avoid difficult conversations?"
- "How often do I interrupt others?"
- "What's my speaking vs. listening ratio?"
- "Do I ask good questions?"
- "How do I handle disagreement?"
- "Am I inclusive of all voices?"
- "Do I use too many filler words?"
- "How clear are my action items?"
- "Do I stay on agenda or get sidetracked?"
- "How has my communication changed over time?"

## 相关用例

- 基于洞察制定个人发展计划
- 用具体例子准备绩效评估材料
- 辅导下属的沟通
- 分析客户电话寻找销售或客服模式
- 研究谈判策略与结果

