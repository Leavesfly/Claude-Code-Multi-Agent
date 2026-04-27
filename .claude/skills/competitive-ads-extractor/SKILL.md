---
name: competitive-ads-extractor
description: 从广告库（Facebook、LinkedIn 等）中提取和分析竞争对手的广告，以了解哪些消息、问题和创意方法有效。帮助激发和改进您自己的广告活动。
---

# Competitive Ads Extractor（竞品广告提取器）

本 skill 从广告库中提取你的竞品广告，并分析哪些在起作用——他们强调的问题、目标用例，以及正在产生共鸣的文案/创意。

## 何时使用本 skill

- 研究竞品广告策略
- 为自己的广告寻找灵感
- 理解市场定位
- 识别成功的广告模式
- 分析有效的信息传达方式
- 发现新的用例或痛点
- 基于已验证的概念规划广告活动

## 本 skill 能做什么

1. **提取广告**：抓取 Facebook Ad Library、LinkedIn 等平台的广告
2. **截取截图**：保存所有广告的视觉副本
3. **分析信息**：识别问题、用例和 value proposition
4. **广告归类**：按主题、受众或格式分组
5. **识别模式**：找出常见且成功的方式
6. **提供洞察**：解释为什么某些广告可能效果好

## 用法

### 基础提取

```
Extract all current ads from [Competitor Name] on Facebook Ad Library
```

```
Scrape ads from [Company] and analyze their messaging
```

### 专项分析

```
Get all ads from [Competitor] focusing on their messaging 
about [specific problem]. What pain points are they highlighting?
```

### 竞品集合分析

```
Extract ads from these 5 competitors: [list]. 
Compare their approaches and tell me what's working.
```

### 指定平台

```
Get LinkedIn ads from [Competitor] and analyze their 
B2B positioning strategy
```

## 示例

**User**："Extract ads from Notion on Facebook Ad Library and tell me what messaging is working for them."

**过程**：
```
Accessing Facebook Ad Library...
Searching for: Notion
Found: 23 active ads

Extracting screenshots...
[████████████████████] 100%

Saved to: competitor-ads/notion/
- ad-001-collaboration.png
- ad-002-productivity.png
- ad-003-templates.png
...

Analyzing messaging...
```

**输出**：
```
# Notion Ad Analysis

## Overview
- Total Ads: 23 active
- Primary Themes: Productivity (35%), Collaboration (30%), 
  Templates (20%), AI Features (15%)
- Ad Formats: Static images (60%), Video (40%)
- CTA Patterns: "Try for free", "Get started"

## Key Problems They're Highlighting

1. **Scattered Information** (8 ads)
   Copy: "Stop switching between 10 different tools"
   Why it works: Direct pain point many teams face

2. **Meeting Overload** (5 ads)
   Copy: "Replace unnecessary meetings with async updates"
   Why it works: Post-COVID remote work pain point

3. **Lost Documentation** (4 ads)
   Copy: "Never ask 'where is that doc?' again"
   Why it works: Universal workplace frustration

## Successful Creative Patterns

### Pattern 1: Before/After Split
- Shows chaotic tool landscape → Clean Notion workspace
- Used in 6 high-performing ads
- Visual metaphor is immediately clear

### Pattern 2: Feature Showcase
- GIF of actual product usage
- Shows specific feature in 5 seconds
- Used for new features (AI, templates)

### Pattern 3: Social Proof
- "Join 20M users" messaging
- Customer logos
- Used in 4 ads targeting enterprise

## Copy That's Working

Best Headlines:
1. "Your team's knowledge, finally in one place"
   → Benefit-focused, addresses pain directly
   
2. "The all-in-one workspace"
   → Clear positioning, broad appeal
   
3. "AI that actually helps you work"
   → Addresses AI skepticism, practical angle

Best Body Copy Patterns:
- Short sentences (under 10 words)
- Focus on outcomes not features
- Include specific numbers ("Cut meetings by 50%")

## Audience Targeting Insights

Based on ad variations:
- Startup founders: Solo productivity angle
- Team leads: Collaboration and alignment
- Enterprise: Security and compliance mentions
- Students: Free plan, templates, organization

## Recommendations for Your Ads

1. **Test the "tool sprawl" pain point**
   → Strong resonance based on their ad frequency

2. **Use product screenshots over abstract visuals**
   → All their top ads show actual UI

3. **Lead with the problem, not the solution**
   → "Tired of X?" performs better than "Introducing Y"

4. **Keep copy under 100 characters**
   → Their shortest ads seem most frequent

5. **Test before/after visual formats**
   → Proven pattern in their creative

## Files Saved
- All ads: ~/competitor-ads/notion/
- Analysis: ~/competitor-ads/notion/analysis.md
- Best performers: ~/competitor-ads/notion/top-10/
```

**灵感来源：** Lenny's Newsletter 中 Sumant Subrahmanya 的用例

## 你可以从中学到什么

### 信息传达分析
- 他们强调了哪些问题
- 如何对竞争对手进行定位
- 哪些 value proposition 有共鸣
- 目标受众分群

### 创意模式
- 有效的视觉风格
- 视频 vs. 静态图的表现
- 配色与品牌
- 版式模式

### 文案公式
- 标题结构
- call-to-action 模式
- 长度与语气
- 情感触发点

### 活动策略
- 节日主题活动
- 产品发布打法
- 新功能官宣战术
- 再营销模式

## 最佳实践

### 法律与道德
✓ 仅用于研究与启发
✓ 不要直接抄袭广告
✓ 尊重知识产权
✓ 用洞察指导原创创意
✗ 不要抄袭文案或窃取设计

### 分析技巧
1. **识别模式**：哪些主题在重复？
2. **长期追踪**：按月归档广告以观察演化
3. **验证假设**：将成功模式改编为你的品牌
4. **按受众切分**：不同目标用不同信息
5. **跨平台对比**：LinkedIn 与 Facebook 的信息传达不同

## 进阶能力

### 趋势追踪
```
Compare [Competitor]'s ads from Q1 vs Q2. 
What messaging has changed?
```

### 多竞品分析
```
Extract ads from [Company A], [Company B], [Company C]. 
What are the common patterns? Where do they differ?
```

### 行业基准
```
Show me ad patterns across the top 10 project management 
tools. What problems do they all focus on?
```

### 格式分析
```
Analyze video ads vs static image ads from [Competitor]. 
Which gets more engagement? (if data available)
```

## 常见 workflow

### 广告活动规划
1. 提取竞品广告
2. 识别成功模式
3. 记录他们信息传达上的空白
4. 头脑风暴独特切入角度
5. 起草测试用的广告变体

### 定位研究
1. 抓取 5 家竞品的广告
2. 梳理他们的定位
3. 找到尚未被充分覆盖的切入点
4. 打造差异化信息传达
5. 与他们的做法对比测试

### 创意灵感
1. 按主题提取广告
2. 分析视觉模式
3. 记录配色与版式趋势
4. 借鉴成功模式
5. 创作原创变体

## 成功要点

1. **定期监测**：按月检查变化
2. **宽泛调研**：也看相邻竞品
3. **全部留档**：构建参考素材库
4. **验证洞察**：自己做实验
5. **追踪表现**：对启发得到的概念做 A/B test
6. **保持原创**：用于启发，而非抄袭
7. **覆盖多平台**：Facebook、LinkedIn、TikTok 等进行对比

## 输出格式

- **截图**：所有广告保存为图片
- **分析报告**：以 Markdown 汇总洞察
- **表格**：包含广告文案、CTA、主题的 CSV
- **演示稿**：Top 案例的可视化 deck
- **模式库**：按打法分类归档

## 相关用例

- 为自己的活动写更好的广告文案
- 理解市场定位
- 发现自己信息传达上的内容空白
- 挖掘产品的新用例
- 规划产品营销策略
- 为社媒内容找灵感

