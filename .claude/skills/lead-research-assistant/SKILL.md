---
name: lead-research-assistant
description: 通过分析您的业务、搜索目标公司并提供可行的联系策略，为您的产品或服务识别高质量潜在客户。非常适合销售、业务开发和营销专业人士。
---

# Lead Research Assistant（潜在客户研究助手）

本 skill 通过分析你的产品/服务、理解你的理想客户画像（ICP）并提供可行动的触达策略，帮你识别并甄别高质量 lead。

## 何时使用本 skill

- 为你的产品/服务寻找潜在客户
- 构建合作伙伴触达公司清单
- 识别销售触达的目标客户
- 调研匹配你理想客户画像的公司
- 为业务拓展活动做准备

## 本 skill 能做什么

1. **理解你的业务**：分析产品/服务、value proposition、目标市场
2. **识别目标公司**：基于以下维度找出匹配 ICP 的公司：
   - 行业与领域
   - 公司规模与所在地
   - 所用的技术栈与工具
   - 成长阶段与融资情况
   - 你的产品所解决的痛点
3. **对 lead 排序**：基于 fit score 与相关性对公司排名
4. **提供触达策略**：为每个 lead 建议个性化的触达方式
5. **信息补全**：收集决策者与公司背景的相关信息

## 用法

### 基本用法

直接描述你的产品/服务和你在找什么：

```
I'm building [product description]. Find me 10 companies in [location/industry] 
that would be good leads for this.
```

### 结合你的 codebase

为获得更好的结果，在你的产品源码目录下运行：

```
Look at what I'm building in this repository and identify the top 10 companies 
in [location/industry] that would benefit from this product.
```

### 进阶用法

更精准的调研：

```
My product: [description]
Ideal customer profile:
- Industry: [industry]
- Company size: [size range]
- Location: [location]
- Current pain points: [pain points]
- Technologies they use: [tech stack]

Find me 20 qualified leads with contact strategies for each.
```

## 操作说明

当用户请求 lead 调研时：

1. **理解产品/服务**
   - 如果在代码目录里，分析 codebase 理解产品
   - 针对 value proposition 提澄清问题
   - 识别关键特性与收益
   - 理解它解决什么问题

2. **定义理想客户画像（ICP）**
   - 确定目标行业与领域
   - 识别公司规模区间
   - 考虑地域偏好
   - 理解相关痛点
   - 记录技术方面的要求

3. **调研与识别 lead**
   - 搜索匹配准则的公司
   - 寻找需求信号（招聘信息、技术栈、近期新闻）
   - 关注成长指标（融资、扩张、招聘）
   - 识别具有互补型产品/服务的公司
   - 核对预算相关的信号

4. **排序与评分**
   - 为每个 lead 打 fit score（1-10）
   - 考虑以下因素：
     - 与 ICP 的契合度
     - 即时需求信号
     - 预算可得性
     - 竞争格局
     - 时机信号

5. **提供可行动的输出**

   对每个 lead 提供：
   - **公司名** 与 website
   - **为什么契合**：基于对方业务的具体理由
   - **Priority Score**：1-10 附带说明
   - **Decision Maker**：目标角色/职位（如 "VP of Engineering"）
   - **Contact Strategy**：个性化触达建议
   - **Value Proposition**：你的产品如何解决其具体问题
   - **Conversation Starters**：触达中可提到的具体话题
   - **LinkedIn URL**：若可得，便于加联系

6. **格式化输出**

   用清晰、易扫读的格式呈现结果：

   ```markdown
   # Lead Research Results
   
   ## Summary
   - Total leads found: [X]
   - High priority (8-10): [X]
   - Medium priority (5-7): [X]
   - Average fit score: [X]
   
   ---
   
   ## Lead 1: [Company Name]
   
   **Website**: [URL]
   **Priority Score**: [X/10]
   **Industry**: [Industry]
   **Size**: [Employee count/revenue range]
   
   **Why They're a Good Fit**:
   [2-3 specific reasons based on their business]
   
   **Target Decision Maker**: [Role/Title]
   **LinkedIn**: [URL if available]
   
   **Value Proposition for Them**:
   [Specific benefit for this company]
   
   **Outreach Strategy**:
   [Personalized approach - mention specific pain points, recent company news, or relevant context]
   
   **Conversation Starters**:
   - [Specific point 1]
   - [Specific point 2]
   
   ---
   
   [Repeat for each lead]
   ```

7. **提供下一步**
   - 建议把结果保存为 CSV 供 CRM 导入
   - 主动起草个性化触达消息
   - 基于时机给出优先级建议
   - 针对 top lead 建议进一步调研

## 示例

### 示例 1：来自 Lenny's Newsletter

**User**："I'm building a tool that masks sensitive data in AI coding assistant queries. Find potential leads."

**Output**：生成一份排序后的公司清单，这些公司：
- 使用 AI coding assistant（Copilot、Cursor 等）
- 处理敏感数据（fintech、healthcare、legal）
- GitHub repo 里有使用 coding agent 的证据
- 可能在代码中意外暴露了敏感数据
- 附带相关决策者的 LinkedIn URL

### 示例 2：本地企业

**User**："I run a consulting practice for remote team productivity. Find me 10 companies in the Bay Area that recently went remote."

**Output**：识别出这样一批公司：
- 近期发布了远程工作岗位
- 宣布了 remote-first 政策
- 正在招募分布式团队
- 显示出远程工作痛点信号
- 并为每家提供个性化触达策略

## 获得最佳结果的建议

- **具体描述**你的产品及其独特价值
- 若适用，**在 codebase 目录下运行**以自动获取上下文
- **提供上下文**：你的 ICP 是什么
- **设定约束**：行业、地域或公司规模
- 对有希望的 lead **请求进一步调研**以得到更深洞察

## 相关用例

- 识别完 lead 后起草个性化触达邮件
- 构建可直接导入 CRM 的合格潜客 CSV
- 详细调研特定公司
- 分析竞品客户构成
- 识别合作伙伴机会
