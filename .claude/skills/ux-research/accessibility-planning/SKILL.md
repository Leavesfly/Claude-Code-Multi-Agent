---
name: accessibility-planning
description: 规划无障碍合规性 - WCAG 2.2、Section 508、EN 301 549、包容性设计原则、审计规划和修复策略。
allowed-tools: Read, Glob, Grep, Task
---

# Accessibility Planning（无障碍规划）

在开发开始前规划包容性设计和 WCAG 合规性。

## When to Use This Skill（何时使用此 skill）

在以下情况使用此 skill：

- **无障碍规划任务** - 处理无障碍合规性规划 - WCAG 2.2、Section 508、EN 301 549、包容性设计原则、审计规划和修复策略
- **规划或设计阶段** - 需要无障碍规划方法的指导
- **最佳实践** - 希望遵循既定的模式和标准

## MANDATORY: Skill Loading First（强制要求：首先加载 skill）

在回答任何无障碍问题之前：

2. 使用官方 WCAG 和无障碍标准
3. 所有指导必须基于经过验证的无障碍实践

## Accessibility Standards（无障碍标准）

### WCAG 2.2 Overview（WCAG 2.2 概述）

WCAG（Web Content Accessibility Guidelines，Web 内容可访问性指南）是国际公认的 Web 无障碍标准。

| Level（级别） | Description（描述） | Typical Requirement（典型要求） |
|-------|-------------|---------------------|
| **A** | Minimum（最低） | 移除基本无障碍障碍 |
| **AA** | Standard（标准） | 解决大多数常见障碍 |
| **AAA** | Enhanced（增强） | 最高级别的无障碍性 |

**目标：** 大多数组织以 **WCAG 2.2 Level AA** 为目标，这是法律合规的常见要求。

### Compliance Frameworks（合规框架）

不同地区和行业有不同的无障碍法规和标准：

| Standard（标准） | Region（地区） | Applies To（适用范围） |
|----------|--------|------------|
| **WCAG 2.2** | Global（全球） | Web content（Web 内容） |
| **Section 508** | USA（美国） | Federal agencies, contractors（联邦机构、承包商） |
| **EN 301 549** | Europe（欧洲） | Public sector, ICT products（公共部门、ICT 产品） |
| **ADA** | USA（美国） | Public accommodations（公共场所） |
| **AODA** | Ontario, Canada（加拿大安大略省） | Businesses, organizations（企业、组织） |

## WCAG 2.2 Principles (POUR)（WCAG 2.2 四大原则）

WCAG 2.2 基于四个核心原则（简称 POUR），所有 Web 内容都必须满足这些原则才能实现无障碍访问。

### 1. Perceivable（可感知）

用户必须能够感知内容。这意味着信息不能仅通过一种感官呈现，需要提供多种感知方式。

| Guideline（指南） | Key Requirements（关键要求） |
|-----------|-----------------|
| **1.1 Text Alternatives（文本替代）** | 图像的 alt text（替代文本）、音频的字幕 |
| **1.2 Time-based Media（基于时间的媒体）** | 字幕、音频描述、转录文本 |
| **1.3 Adaptable（可适应）** | 通过语义化 HTML 构建结构、有意义的顺序 |
| **1.4 Distinguishable（可区分）** | 颜色对比度、调整文本大小、音频控制 |

### 2. Operable（可操作）

用户必须能够操作界面。所有交互功能都应该可以通过键盘和其他输入设备使用。

| Guideline（指南） | Key Requirements（关键要求） |
|-----------|-----------------|
| **2.1 Keyboard Accessible（键盘可访问）** | 所有功能可通过键盘操作 |
| **2.2 Enough Time（充足的时间）** | 可调整计时、暂停/停止内容 |
| **2.3 Seizures（癫痫发作）** | 无每秒闪烁超过 3 次的内容 |
| **2.4 Navigable（可导航）** | 跳过链接、页面标题、焦点顺序 |
| **2.5 Input Modalities（输入方式）** | 指针手势、目标大小、运动 |

### 3. Understandable（可理解）

内容和操作必须易于理解。用户应该能够理解信息和界面的操作方式。

| Guideline（指南） | Key Requirements（关键要求） |
|-----------|-----------------|
| **3.1 Readable（可读）** | 语言标识、缩写 |
| **3.2 Predictable（可预测）** | 一致的导航、无意外变化 |
| **3.3 Input Assistance（输入辅助）** | 错误识别、标签、建议 |

### 4. Robust（鲁棒性）

内容必须与 assistive technology（辅助技术）兼容。随着技术的发展，内容应该能够被不同的用户代理（包括辅助技术）可靠地解析。

| Guideline（指南） | Key Requirements（关键要求） |
|-----------|-----------------|
| **4.1 Compatible（兼容）** | 有效的标记、组件的 name/role/value |

## WCAG 2.2 New Success Criteria（WCAG 2.2 新增成功标准）

WCAG 2.2 版本引入了多项新的成功标准，重点关注焦点可见性、拖动操作替代方案、目标尺寸和身份验证可访问性等方面。

| Criterion（标准） | Level（级别） | Description（描述） |
|-----------|-------|-------------|
| **2.4.11 Focus Not Obscured (Min)** | AA | 聚焦元素不完全被隐藏 |
| **2.4.12 Focus Not Obscured (Enhanced)** | AAA | 聚焦元素不部分被隐藏 |
| **2.4.13 Focus Appearance** | AAA | 可见焦点指示器要求 |
| **2.5.7 Dragging Movements** | AA | 拖动的单指针替代方案 |
| **2.5.8 Target Size (Minimum)** | AA | 最小 24x24 CSS 像素 |
| **3.2.6 Consistent Help** | A | 帮助机制位于一致位置 |
| **3.3.7 Redundant Entry** | A | 不要求重新输入相同信息 |
| **3.3.8 Accessible Authentication (Min)** | AA | 身份验证无需认知功能测试 |
| **3.3.9 Accessible Authentication (Enhanced)** | AAA | 扩展的身份验证要求 |

## Accessibility Audit Planning（无障碍审计规划）

无障碍审计是确保产品符合 WCAG 标准的关键步骤。通过系统化的测试和评估，可以发现并修复潜在的无障碍问题。

### Audit Types（审计类型）

根据项目阶段和资源情况，可以选择不同类型的审计方式：

| Type（类型） | Scope（范围） | When（时机） |
|------|-------|------|
| **Quick Audit（快速审计）** | High-traffic pages, key flows（高流量页面、关键流程） | Sprint review（Sprint 评审） |
| **Comprehensive Audit（全面审计）** | Full WCAG checklist（完整 WCAG 检查清单） | Before launch（发布前） |
| **Continuous Monitoring（持续监控）** | Automated scans（自动扫描） | Ongoing（持续进行） |
| **User Testing（用户测试）** | With assistive technology users（与辅助技术用户一起） | Periodically（定期） |

### Audit Checklist Template（审计检查清单模板）

以下是一个完整的无障碍审计计划模板，可根据项目需求进行调整：

```markdown
# Accessibility Audit Plan（无障碍审计计划）

## Scope（范围）
- **Product/Site:** [Name]（产品/站点：[名称]）
- **Pages/Components:** [List or "All"]（页面/组件：[列表或"全部"]）
- **Standard:** WCAG 2.2 Level AA（标准：WCAG 2.2 Level AA）
- **Timeline:** [Dates]（时间线：[日期]）

## Testing Matrix（测试矩阵）

### Automated Testing（自动化测试）
- [ ] axe DevTools scan（axe DevTools 扫描）
- [ ] WAVE evaluation（WAVE 评估）
- [ ] Lighthouse accessibility audit（Lighthouse 无障碍审计）
- [ ] Pa11y CI integration（Pa11y CI 集成）

### Manual Testing（手动测试）
- [ ] Keyboard-only navigation（仅键盘导航）
- [ ] Screen reader testing (NVDA, VoiceOver, JAWS)（屏幕阅读器测试）
- [ ] High contrast mode（高对比度模式）
- [ ] Browser zoom (200%)（浏览器缩放 200%）
- [ ] Color blindness simulation（色盲模拟）

### Assistive Technology Matrix（辅助技术矩阵）

| AT（辅助技术） | Browser（浏览器） | OS（操作系统） | Tester（测试者） |
|----|---------|----|----|
| NVDA | Chrome | Windows | [Name] |
| JAWS | Chrome | Windows | [Name] |
| VoiceOver | Safari | macOS | [Name] |
| VoiceOver | Safari | iOS | [Name] |
| TalkBack | Chrome | Android | [Name] |

## Pages/Components to Test（待测试的页面/组件）

| Priority（优先级） | Page/Component（页面/组件） | URL/Location（URL/位置） | Status（状态） |
|----------|----------------|--------------|--------|
| High（高） | Homepage（首页） | / | Pending（待处理） |
| High（高） | Login（登录） | /login | Pending（待处理） |
| High（高） | Checkout（结账） | /checkout | Pending（待处理） |
| Medium（中） | Product listing（产品列表） | /products | Pending（待处理） |
| Medium（中） | Forms（表单） | /contact | Pending（待处理） |

## Issue Tracking（问题跟踪）
- **Tool:** [Jira/GitHub Issues/etc.]（工具：[Jira/GitHub Issues/等]）
- **Label:** accessibility（标签：accessibility）
- **Severity Levels:** Critical, Major, Minor, Enhancement（严重级别：严重、主要、次要、增强）

## Deliverables（交付物）
- [ ] Issue log with WCAG mapping（带 WCAG 映射的问题日志）
- [ ] Prioritized remediation plan（优先修复计划）
- [ ] VPAT/ACR (if required)（VPAT/ACR，如需要）
- [ ] Executive summary（执行摘要）
```

## Common Accessibility Issues（常见无障碍问题）

在实际开发中，以下是一些最常见的无障碍问题及其修复方法。这些问题按严重程度分类，帮助团队优先处理关键问题。

### Critical (Level A failures)（严重问题 - Level A 失败）

这些问题会完全阻止某些用户访问内容，必须立即修复：

| Issue（问题） | WCAG | Fix（修复方法） |
|-------|------|-----|
| **Missing alt text（缺少替代文本）** | 1.1.1 | 添加描述性的 alt 属性 |
| **No keyboard access（无键盘访问）** | 2.1.1 | 确保所有交互元素可获得焦点 |
| **Missing form labels（缺少表单标签）** | 1.3.1 | 将标签与输入框关联 |
| **Empty buttons/links（空按钮/链接）** | 4.1.2 | 添加可访问名称 |
| **Auto-playing media（自动播放媒体）** | 1.4.2 | 提供暂停/停止控制 |

### Major (Level AA failures)（主要问题 - Level AA 失败）

这些问题会显著影响用户体验，应在下一个 Sprint 中修复：

| Issue（问题） | WCAG | Fix（修复方法） |
|-------|------|-----|
| **Low color contrast（颜色对比度低）** | 1.4.3 | 文本 4.5:1，大文本 3:1 |
| **No skip navigation（无跳过导航）** | 2.4.1 | 添加跳到主内容的跳过链接 |
| **Missing page titles（缺少页面标题）** | 2.4.2 | 唯一且描述性的标题 |
| **Focus not visible（焦点不可见）** | 2.4.7 | 可见的焦点指示器 |
| **Reflow issues（重排问题）** | 1.4.10 | 在 400% 缩放时响应式 |

### .NET/Blazor Accessibility Patterns（.NET/Blazor 无障碍模式）

```csharp
// Accessible button component
@code {
    [Parameter] public string AriaLabel { get; set; } = "";
    [Parameter] public bool IsDisabled { get; set; }
    [Parameter] public RenderFragment? ChildContent { get; set; }
    [Parameter] public EventCallback OnClick { get; set; }
}

<button
    type="button"
    aria-label="@AriaLabel"
    aria-disabled="@IsDisabled.ToString().ToLower()"
    disabled="@IsDisabled"
    @onclick="HandleClick"
    class="btn">
    @ChildContent
</button>

@code {
    private async Task HandleClick()
    {
        if (!IsDisabled)
        {
            await OnClick.InvokeAsync();
        }
    }
}
```

```csharp
// Accessible form field
<div class="form-group">
    <label for="@Id" class="@(IsRequired ? "required" : "")">
        @Label
        @if (IsRequired)
        {
            <span aria-hidden="true">*</span>
            <span class="visually-hidden">(required)</span>
        }
    </label>

    <input
        id="@Id"
        name="@Name"
        type="@Type"
        @bind="Value"
        aria-required="@IsRequired.ToString().ToLower()"
        aria-invalid="@(!string.IsNullOrEmpty(ErrorMessage)).ToString().ToLower()"
        aria-describedby="@(HasError ? $"{Id}-error" : HintId)"
        class="@(HasError ? "input-error" : "")" />

    @if (!string.IsNullOrEmpty(Hint))
    {
        <span id="@HintId" class="hint">@Hint</span>
    }

    @if (HasError)
    {
        <span id="@($"{Id}-error")" class="error" role="alert">
            @ErrorMessage
        </span>
    }
</div>
```

```csharp
// Skip link component
<a href="#main-content" class="skip-link">
    Skip to main content
</a>

<style>
    .skip-link {
        position: absolute;
        top: -40px;
        left: 0;
        background: #000;
        color: #fff;
        padding: 8px;
        z-index: 100;
    }

    .skip-link:focus {
        top: 0;
    }
</style>
```

## Color Contrast Requirements（颜色对比度要求）

颜色对比度是无障碍设计中的关键要素，确保文本和 UI 元素对所有用户都清晰可见。WCAG 规定了不同元素类型的最低对比度要求。

### Contrast Ratios（对比度比率）

以下是 WCAG 2.2 规定的对比度要求，适用于各种文本和 UI 元素：

| Element Type（元素类型） | WCAG AA | WCAG AAA |
|--------------|---------|----------|
| Normal text (<18pt)（普通文本） | 4.5:1 | 7:1 |
| Large text (≥18pt or ≥14pt bold)（大文本） | 3:1 | 4.5:1 |
| UI components & graphics（UI 组件和图形） | 3:1 | 3:1 |

### Contrast Checker（对比度检查器）

以下是一个 C# 实现的对比度检查器，可用于计算两种颜色之间的对比度比率：

```csharp
public class ContrastChecker
{
    public record ContrastResult(
        double Ratio,
        bool PassesAA,
        bool PassesAAA,
        bool PassesLargeTextAA,
        bool PassesLargeTextAAA
    );

    public ContrastResult Calculate(string foreground, string background)
    {
        var fgLuminance = GetRelativeLuminance(foreground);
        var bgLuminance = GetRelativeLuminance(background);

        var lighter = Math.Max(fgLuminance, bgLuminance);
        var darker = Math.Min(fgLuminance, bgLuminance);

        var ratio = (lighter + 0.05) / (darker + 0.05);
        ratio = Math.Round(ratio, 2);

        return new ContrastResult(
            Ratio: ratio,
            PassesAA: ratio >= 4.5,
            PassesAAA: ratio >= 7,
            PassesLargeTextAA: ratio >= 3,
            PassesLargeTextAAA: ratio >= 4.5
        );
    }

    private static double GetRelativeLuminance(string hex)
    {
        var rgb = ParseHex(hex);
        var r = GetLuminanceComponent(rgb.R);
        var g = GetLuminanceComponent(rgb.G);
        var b = GetLuminanceComponent(rgb.B);

        return 0.2126 * r + 0.7152 * g + 0.0722 * b;
    }

    private static double GetLuminanceComponent(byte value)
    {
        var sRGB = value / 255.0;
        return sRGB <= 0.03928
            ? sRGB / 12.92
            : Math.Pow((sRGB + 0.055) / 1.055, 2.4);
    }

    private static (byte R, byte G, byte B) ParseHex(string hex)
    {
        hex = hex.TrimStart('#');
        return (
            Convert.ToByte(hex[..2], 16),
            Convert.ToByte(hex[2..4], 16),
            Convert.ToByte(hex[4..6], 16)
        );
    }
}
```

## Keyboard Navigation Requirements（键盘导航要求）

键盘导航是无障碍设计的核心要素，确保所有用户都能通过键盘操作界面。这对于无法使用鼠标的用户至关重要。

### Focus Order（焦点顺序）

正确的焦点顺序对于键盘用户的导航体验至关重要。以下是焦点管理的最佳实践和实现示例：

```csharp
// Tab order management
public class FocusManager
{
    private readonly IJSRuntime _js;

    public async Task SetFocusTo(string elementId)
    {
        await _js.InvokeVoidAsync("focusElement", elementId);
    }

    public async Task TrapFocusInModal(string modalId)
    {
        await _js.InvokeVoidAsync("trapFocus", modalId);
    }

    public async Task RestoreFocus(string previousElementId)
    {
        await _js.InvokeVoidAsync("focusElement", previousElementId);
    }
}
```

```javascript
// Focus trap for modals
window.trapFocus = function(modalId) {
    const modal = document.getElementById(modalId);
    const focusableElements = modal.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );

    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];

    modal.addEventListener('keydown', function(e) {
        if (e.key === 'Tab') {
            if (e.shiftKey && document.activeElement === firstElement) {
                lastElement.focus();
                e.preventDefault();
            } else if (!e.shiftKey && document.activeElement === lastElement) {
                firstElement.focus();
                e.preventDefault();
            }
        }
    });

    firstElement.focus();
};
```

### Required Keyboard Interactions（必需的键盘交互）

| Component（组件） | Keys（按键） | Action（操作） |
|-----------|------|--------|
| Button（按钮） | Enter, Space | Activate（激活） |
| Link（链接） | Enter | Navigate（导航） |
| Checkbox（复选框） | Space | Toggle（切换） |
| Radio（单选按钮） | Arrow keys（方向键） | Navigate options（导航选项） |
| Select（下拉选择） | Arrow keys, Enter | Navigate, select（导航、选择） |
| Tab panel（标签面板） | Arrow keys（方向键） | Switch tabs（切换标签） |
| Menu（菜单） | Arrow keys, Enter, Esc | Navigate, select, close（导航、选择、关闭） |
| Modal（模态框） | Esc | Close（关闭） |

## VPAT (Voluntary Product Accessibility Template)（自愿产品无障碍模板）

VPAT 是一份文档，说明产品或服务如何符合无障碍标准。它通常用于向客户和采购机构展示产品的无障碍合规性。

### VPAT Structure（VPAT 结构）

以下是 VPAT 报告的标准结构和示例内容：

```markdown
# [Product Name] Accessibility Conformance Report（[产品名称] 无障碍合规报告）
## WCAG 2.2 Edition

**Report Date:** [Date]（报告日期：[日期]）
**Product Version:** [Version]（产品版本：[版本]）
**Contact:** [Email]（联系方式：[邮箱]）

### Table 1: Success Criteria, Level A（表 1：成功标准，Level A）

| Criteria（标准） | Conformance Level（合规级别） | Remarks（备注） |
|----------|-------------------|---------|
| 1.1.1 Non-text Content（非文本内容） | Supports（支持） | All images have alt text（所有图像都有替代文本） |
| 1.2.1 Audio-only and Video-only（仅音频和仅视频） | Not Applicable（不适用） | No pre-recorded media（无预录制媒体） |
| 1.3.1 Info and Relationships（信息和关系） | Partially Supports（部分支持） | [Explanation]（[说明]） |
| ... | ... | ... |

### Table 2: Success Criteria, Level AA（表 2：成功标准，Level AA）
[Continue pattern]（继续此模式）

### Conformance Levels（合规级别）
- **Supports:** Fully meets criterion（完全符合标准）
- **Partially Supports:** Some aspects don't meet criterion（某些方面不符合标准）
- **Does Not Support:** Majority doesn't meet criterion（大部分不符合标准）
- **Not Applicable:** Criterion doesn't apply（标准不适用）
```

## Remediation Planning（修复规划）

发现无障碍问题后，需要制定系统化的修复计划。优先级矩阵帮助团队确定哪些问题应该首先处理，以确保最大的影响。

### Priority Matrix（优先级矩阵）

根据问题的严重程度和对用户的影响，将修复任务分为不同优先级：

| Priority（优先级） | Criteria（标准） | Timeline（时间线） |
|----------|----------|----------|
| **P0 - Critical（严重）** | Level A failures, blocks functionality（Level A 失败，阻碍功能） | Immediate（立即） |
| **P1 - High（高）** | Level AA failures, significant barriers（Level AA 失败，重大障碍） | Sprint 1（第一个 Sprint） |
| **P2 - Medium（中）** | Level AA improvements, minor barriers（Level AA 改进，轻微障碍） | Sprint 2-3（第二至三个 Sprint） |
| **P3 - Low（低）** | Best practices, enhancements（最佳实践、增强功能） | Backlog（待办事项列表） |

### Remediation Tracking（修复跟踪）

以下是用于跟踪无障碍问题的 C# 数据模型示例：

```csharp
public class AccessibilityIssue
{
    public Guid Id { get; init; }
    public required string Title { get; init; }
    public required string Description { get; init; }
    public required WcagCriterion Criterion { get; init; }
    public required WcagLevel Level { get; init; }
    public required IssueStatus Status { get; init; }
    public required Priority Priority { get; init; }
    public required string AffectedComponent { get; init; }
    public string? Remediation { get; init; }
    public string? AssignedTo { get; init; }
    public DateOnly? TargetDate { get; init; }
}

public record WcagCriterion(string Number, string Name);

public enum WcagLevel { A, AA, AAA }

public enum Priority { Critical, High, Medium, Low }

public enum IssueStatus
{
    Open,
    InProgress,
    InReview,
    Closed,
    WontFix
}
```

## Inclusive Design Principles（包容性设计原则）

包容性设计是一种设计方法论，旨在创建所有人都能使用的产品和服务，无论其能力或情况如何。它强调从边缘用户出发，惠及更广泛的用户群体。

### Microsoft Inclusive Design（微软包容性设计）

微软的包容性设计框架提供了三个核心原则，指导设计师和开发者创建更具包容性的产品：

1. **Recognize exclusion（识别排斥）** - 理解排斥是如何发生的。排斥通常发生在设计师假设所有用户都与自己相似时。
2. **Solve for one, extend to many（为一而解，惠及众人）** - 首先为边缘情况设计。解决一个人的需求往往能惠及许多人。
3. **Learn from diversity（从多样性中学习）** - 纳入多元化的视角。不同背景和能力的人能提供宝贵的洞察。

### Permanent, Temporary, Situational（永久性、暂时性、情境性）

包容性设计的核心理念是认识到障碍可以是永久性的、暂时性的或情境性的。理解这一点有助于设计更灵活的产品：

| Sense（感官） | Permanent（永久性） | Temporary（暂时性） | Situational（情境性） |
|-------|-----------|-----------|-------------|
| Touch（触觉） | One arm（单臂） | Arm injury（手臂受伤） | Holding a baby（抱着婴儿） |
| See（视觉） | Blind（失明） | Cataract（白内障） | Bright sunlight（强烈阳光） |
| Hear（听觉） | Deaf（失聪） | Ear infection（耳部感染） | Noisy environment（嘈杂环境） |
| Speak（言语） | Nonverbal（非言语） | Laryngitis（喉炎） | Heavy accent（浓重口音） |

例如，为单手用户设计的解决方案（如语音输入）也能帮助暂时受伤的人或在忙碌情境下的人。

## Checklist: Accessibility Planning（无障碍规划检查清单）

以下检查清单涵盖了无障碍规划的各个阶段，从策略制定到文档发布。团队应根据项目需求调整此清单。

### Strategy（策略）

在项目开始阶段，需要明确无障碍目标和资源分配：

- [ ] Target compliance level defined (AA recommended)（已定义目标合规级别，推荐 AA）
- [ ] Legal requirements identified（已识别法律要求）
- [ ] Timeline established（已建立时间线）
- [ ] Resources allocated（已分配资源）

### Design Phase（设计阶段）

设计阶段是无障碍实现的关键时期，需要在视觉和交互设计中考虑无障碍需求：

- [ ] Color contrast verified（已验证颜色对比度）
- [ ] Touch targets sized (44x44px minimum)（触摸目标尺寸已设置，最小 44x44px）
- [ ] Focus states designed（已设计焦点状态）
- [ ] Error states accessible（错误状态可访问）
- [ ] Motion preferences considered（已考虑运动偏好）

### Development Phase（开发阶段）

开发阶段需要将设计转化为符合无障碍标准的代码实现：

- [ ] Semantic HTML used（已使用语义化 HTML）
- [ ] ARIA used correctly (sparingly)（ARIA 正确使用，适度使用）
- [ ] Keyboard navigation works（键盘导航正常工作）
- [ ] Focus management implemented（已实现焦点管理）
- [ ] Skip links added（已添加跳过链接）

### Testing Phase（测试阶段）

测试阶段需要验证实现的无障碍功能是否满足标准要求：

- [ ] Automated testing integrated（已集成自动化测试）
- [ ] Manual testing completed（已完成手动测试）
- [ ] Screen reader testing done（已完成屏幕阅读器测试）
- [ ] Keyboard-only testing done（已完成仅键盘测试）
- [ ] Zoom/reflow tested（已测试缩放/重排）

### Documentation（文档）

文档阶段需要记录无障碍实现情况和已知问题：

- [ ] Accessibility statement published（已发布无障碍声明）
- [ ] VPAT completed (if required)（已完成 VPAT，如需要）
- [ ] Known issues documented（已记录已知问题）
- [ ] Contact for feedback provided（已提供反馈联系方式）

## Related Skills（相关 skill）

- `usability-testing` - Inclusive usability testing（包容性可用性测试）
- `design-system-planning` - Accessible components（可访问组件）
- `heuristic-evaluation` - Accessibility heuristics（无障碍启发式评估）
- `service-blueprinting` - Accessible service touchpoints（可访问的服务触点）
