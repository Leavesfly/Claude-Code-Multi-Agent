---
name: pptx
description: "演示文稿创建、编辑和分析。当 Claude 需要处理演示文稿（.pptx 文件）时：(1) 创建新演示文稿，(2) 修改或编辑内容，(3) 处理布局，(4) 添加评论或演讲者备注，或任何其他演示文稿任务"
license: Proprietary. LICENSE.txt has complete terms
---

# PPTX 创建、编辑与分析

## 概述

用户可能会要求你创建、编辑或分析 .pptx 文件的内容。.pptx 文件本质上是一个包含 XML 文件和其他资源的 ZIP 压缩包，你可以读取或编辑这些内容。针对不同的任务，你有不同的工具和工作流程可用。

## 阅读与分析内容

### 文本提取
如果你只需要阅读演示文稿的文本内容，应该将文档转换为 Markdown 格式：

```bash
# 将文档转换为 Markdown
python -m markitdown path-to-file.pptx
```

### 原始 XML 访问
在以下情况下需要原始 XML 访问权限：评论、演讲者备注、幻灯片布局、动画、设计元素和复杂格式。对于这些功能中的任何一个，你都需要解包演示文稿并读取其原始 XML 内容。

#### 解包文件
`python ooxml/scripts/unpack.py <office_file> <output_dir>`

**注意**：unpack.py 脚本位于项目根目录的 `skills/pptx/ooxml/scripts/unpack.py`。如果该路径下不存在此脚本，请使用 `find . -name "unpack.py"` 来定位它。

#### 关键文件结构
* `ppt/presentation.xml` - 主演示文稿元数据和幻灯片引用
* `ppt/slides/slide{N}.xml` - 单个幻灯片内容（slide1.xml、slide2.xml 等）
* `ppt/notesSlides/notesSlide{N}.xml` - 每张幻灯片的演讲者备注
* `ppt/comments/modernComment_*.xml` - 特定幻灯片的评论
* `ppt/slideLayouts/` - 幻灯片的布局模板
* `ppt/slideMasters/` - 母版幻灯片模板
* `ppt/theme/` - 主题和样式信息
* `ppt/media/` - 图像和其他媒体文件

#### 字体与颜色提取
**当需要模仿示例设计时**：始终先使用以下方法分析演示文稿的排版和颜色：
1. **读取主题文件**：检查 `ppt/theme/theme1.xml` 中的颜色（`<a:clrScheme>`）和字体（`<a:fontScheme>`）
2. **采样幻灯片内容**：检查 `ppt/slides/slide1.xml` 中的实际字体使用情况（`<a:rPr>`）和颜色
3. **搜索模式**：使用 grep 在所有 XML 文件中查找颜色（`<a:solidFill>`、`<a:srgbClr>`）和字体引用

## 从头创建 PowerPoint 演示文稿（不使用模板）

从头创建新的 PowerPoint 演示文稿时，使用 **html2pptx** 工作流将 HTML 幻灯片转换为 PowerPoint，以实现精确定位。

### 设计原则

**关键**：在创建任何演示文稿之前，分析内容并选择合适的设计元素：
1. **考虑主题内容**：这个演示文稿是关于什么的？它暗示了什么基调、行业或情绪？
2. **检查品牌标识**：如果用户提到公司/组织，考虑他们的品牌颜色和身份
3. **匹配调色板与内容**：选择反映主题的配色方案
4. **说明你的方法**：在编写代码之前解释你的设计选择

**要求**：
- ✅ 在编写代码**之前**说明基于内容的设计方法
- ✅ 仅使用网络安全字体：Arial、Helvetica、Times New Roman、Georgia、Courier New、Verdana、Tahoma、Trebuchet MS、Impact
- ✅ 通过大小、粗细和颜色创建清晰的视觉层次
- ✅ 确保可读性：强对比度、适当大小的文本、干净的对齐方式
- ✅ 保持一致性：跨幻灯片重复模式、间距和视觉语言

#### 调色板选择

**创造性地选择颜色**：
- **超越默认值**：什么颜色真正匹配这个特定主题？避免自动驾驶式的选择。
- **考虑多个角度**：主题、行业、情绪、能量水平、目标受众、品牌标识（如果提到）
- **勇于尝试**：尝试意想不到的组合——医疗保健演示不必是绿色，金融不必是海军蓝
- **构建你的调色板**：选择 3-5 种相互配合的颜色（主导颜色 + 辅助色调 + 强调色）
- **确保对比度**：文本必须在背景上清晰可读

**示例调色板**（使用这些来激发创意——选择一个、调整它或创建自己的）：

1. **Classic Blue**：深海军蓝 (#1C2833)、石板灰 (#2E4053)、银色 (#AAB7B8)、米白 (#F4F6F6)
2. **Teal & Coral**：青绿 (#5EA8A7)、深青绿 (#277884)、珊瑚红 (#FE4447)、白色 (#FFFFFF)
3. **Bold Red**：红色 (#C0392B)、亮红 (#E74C3C)、橙色 (#F39C12)、黄色 (#F1C40F)、绿色 (#2ECC71)
4. **Warm Blush**：淡紫灰 (#A49393)、腮红粉 (#EED6D3)、玫瑰粉 (#E8B4B8)、奶油色 (#FAF7F2)
5. **Burgundy Luxury**：酒红 (#5D1D2E)、深红 (#951233)、铁锈红 (#C15937)、金色 (#997929)
6. **Deep Purple & Emerald**：紫色 (#B165FB)、深蓝 (#181B24)、翡翠绿 (#40695B)、白色 (#FFFFFF)
7. **Cream & Forest Green**：奶油色 (#FFE1C7)、森林绿 (#40695B)、白色 (#FCFCFC)
8. **Pink & Purple**：粉红 (#F8275B)、珊瑚橙 (#FF574A)、玫瑰粉 (#FF737D)、紫色 (#3D2F68)
9. **Lime & Plum**：酸橙绿 (#C5DE82)、梅子紫 (#7C3A5F)、珊瑚橙 (#FD8C6E)、蓝灰 (#98ACB5)
10. **Black & Gold**：金色 (#BF9A4A)、黑色 (#000000)、米白 (#F4F6F6)
11. **Sage & Terracotta**：鼠尾草绿 (#87A96B)、赤陶色 (#E07A5F)、奶油色 (#F4F1DE)、炭灰 (#2C2C2C)
12. **Charcoal & Red**：炭灰 (#292929)、红色 (#E33737)、浅灰 (#CCCBCB)
13. **Vibrant Orange**：橙色 (#F96D00)、浅灰 (#F2F2F2)、炭灰 (#222831)
14. **Forest Green**：黑色 (#191A19)、绿色 (#4E9F3D)、深绿 (#1E5128)、白色 (#FFFFFF)
15. **Retro Rainbow**：紫色 (#722880)、粉红 (#D72D51)、橙色 (#EB5C18)、琥珀色 (#F08800)、金色 (#DEB600)
16. **Vintage Earthy**：芥末黄 (#E3B448)、鼠尾草绿 (#CBD18F)、森林绿 (#3A6B35)、奶油色 (#F4F1DE)
17. **Coastal Rose**：旧玫瑰色 (#AD7670)、海狸棕 (#B49886)、蛋壳色 (#F3ECDC)、灰烬绿 (#BFD5BE)
18. **Orange & Turquoise**：浅橙色 (#FC993E)、灰绿松石色 (#667C6F)、白色 (#FCFCFC)

#### 视觉细节选项

**几何图案**：
- 对角线分割线代替水平线
- 非对称列宽（30/70、40/60、25/75）
- 旋转的标题文本（90° 或 270°）
- 圆形/六边形图像框架
- 角落的三角形装饰形状
- 重叠的形状以增加深度感

**边框与框架处理**：
- 单侧厚边框（10-20pt）
- 对比色的双线边框
- 角括号代替完整框架
- L 形边框（顶部+左侧或底部+右侧）
- 标题下方的下划线装饰（3-5pt 粗）

**排版处理**：
- 极端大小对比（72pt 标题 vs 11pt 正文）
- 全大写标题配合宽字母间距
- 超大号展示字体的编号部分
- 数据/统计/技术内容使用等宽字体（Courier New）
- 密集信息使用压缩字体（Arial Narrow）
- 描边文本以强调重点

**图表与数据样式**：
- 单色图表配合单一强调色突出关键数据
- 水平条形图代替垂直条形图
- 点图代替条形图
- 极简网格线或完全无网格线
- 直接在元素上的数据标签（无图例）
- 关键指标的超大号数字

**布局创新**：
- 全出血图像配合文本覆盖
- 侧边栏列（20-30% 宽度）用于导航/上下文
- 模块化网格系统（3×3、4×4 区块）
- Z 字形或 F 字形内容流
- 彩色形状上的浮动文本框
- 杂志风格的多列布局

**背景处理**：
- 占据幻灯片 40-60% 的纯色块
- 渐变填充（仅垂直或对角线）
- 分割背景（两种颜色，对角线或垂直）
- 边缘到边缘的色带
- 负空间作为设计元素

### 布局提示
**创建包含图表或表格的幻灯片时：**
- **双列布局（首选）**：使用横跨全宽的标题，然后在其下方使用两列——一列用于文本/要点，另一列用于特色内容。这样可以提供更好的平衡，使图表/表格更易读。使用 flexbox 配合不等列宽（例如 40%/60% 分割）以优化每种内容类型的空间。
- **全幻灯片布局**：让特色内容（图表/表格）占据整个幻灯片以获得最大影响力和可读性
- **切勿垂直堆叠**：不要在单列中将图表/表格放在文本下方——这会导致可读性差和布局问题

### 工作流程
1. **强制要求 - 完整阅读文件**：从头到尾完整阅读 [`html2pptx.md`](html2pptx.md)。**读取此文件时绝不要设置任何范围限制。** 阅读完整的文件内容以获取详细的语法、关键格式化规则和最佳实践，然后再继续创建演示文稿。
2. 为每张幻灯片创建具有适当尺寸的 HTML 文件（例如 16:9 比例为 720pt × 405pt）
   - 使用 `<p>`、`<h1>`-`<h6>`、`<ul>`、`<ol>` 表示所有文本内容
   - 对图表/表格将添加到的区域使用 `class="placeholder"`（渲染为灰色背景以提高可见性）
   - **关键**：首先使用 Sharp 将渐变和图标光栅化为 PNG 图像，然后在 HTML 中引用
   - **布局**：对于包含图表/表格/图像的幻灯片，使用全幻灯片布局或双列布局以提高可读性
3. 创建并运行使用 [`html2pptx.js`](scripts/html2pptx.js) 库的 JavaScript 文件，将 HTML 幻灯片转换为 PowerPoint 并保存演示文稿
   - 使用 `html2pptx()` 函数处理每个 HTML 文件
   - 使用 PptxGenJS API 将图表和表格添加到占位符区域
   - 使用 `pptx.writeFile()` 保存演示文稿
4. **视觉验证**：生成缩略图并检查布局问题
   - 创建缩略图网格：`python scripts/thumbnail.py output.pptx workspace/thumbnails --cols 4`
   - 仔细阅读并检查缩略图图像是否存在以下问题：
     - **文本截断**：文本被标题栏、形状或幻灯片边缘截断
     - **文本重叠**：文本与其他文本或形状重叠
     - **定位问题**：内容过于靠近幻灯片边界或其他元素
     - **对比度问题**：文本与背景之间的对比度不足
   - 如果发现问题，调整 HTML 边距/间距/颜色并重新生成交互演示文稿
   - 重复直到满意为止
5. **最终质量检查**：
   - 打开生成的 .pptx 文件并手动检查每张幻灯片
   - 验证所有文本是否正确显示且未被截断
   - 确认颜色、字体和布局符合预期
   - 测试所有交互元素（如果有）

## 使用模板创建演示文稿

当用户提供了要使用的现有 .pptx 模板时，使用此工作流程。

### 工作流程
1. **分析模板**：解包 .pptx 文件并检查其结构
   - 识别可用的布局和它们的占位符
   - 注意主题颜色、字体和设计元素
   - 理解母版幻灯片和布局层次结构
2. **规划内容映射**：决定每张幻灯片将使用哪个布局
   - 将内容类型映射到适当的布局
   - 注意任何需要在模板中创建的新布局
3. **使用 Python-pptx 创建演示文稿**：
   ```python
   from pptx import Presentation
   
   # 加载模板
   prs = Presentation('template.pptx')
   
   # 使用特定布局添加幻灯片
   title_slide_layout = prs.slide_layouts[0]  # 标题布局通常是索引 0
   slide = prs.slides.add_slide(title_slide_layout)
   
   # 填充占位符
   title = slide.shapes.title
   subtitle = slide.placeholders[1]
   title.text = "演示文稿标题"
   subtitle.text = "副标题或日期"
   
   # 保存演示文稿
   prs.save('output.pptx')
   ```
4. **验证输出**：打开生成的 .pptx 文件并确保所有内容正确呈现

## 编辑现有演示文稿

### 工作流程
1. **解包演示文稿**：`python ooxml/scripts/unpack.py presentation.pptx output_dir`
2. **识别要修改的文件**：
   - 文本内容：`ppt/slides/slide{N}.xml`
   - 演讲者备注：`ppt/notesSlides/notesSlide{N}.xml`
   - 评论：`ppt/comments/modernComment_*.xml`
   - 主题/样式：`ppt/theme/theme1.xml`
3. **进行更改**：
   - 直接编辑 XML 文件
   - 替换图像：更新 `ppt/media/` 中的文件并更新引用
   - 修改主题：编辑 `ppt/theme/theme1.xml` 中的颜色和字体
4. **重新打包演示文稿**：
   ```bash
   cd output_dir
   zip -r ../modified_presentation.pptx *
   ```

### 常见编辑任务

#### 更新文本内容
编辑 `ppt/slides/slide{N}.xml` 文件。查找包含文本的 `<a:t>` 元素：
```xml
<a:r>
  <a:rPr lang="en-US" sz="1800">
    <a:solidFill>
      <a:srgbClr val="000000"/>
    </a:solidFill>
  </a:rPr>
  <a:t>这是可编辑的文本</a:t>
</a:r>
```

#### 修改演讲者备注
编辑 `ppt/notesSlides/notesSlide{N}.xml` 文件。结构与幻灯片类似，但包含演讲者的笔记而非幻灯片内容。

#### 添加或修改评论
评论存储在 `ppt/comments/modernComment_*.xml` 文件中。每个评论文件对应一张有评论的幻灯片。

#### 更改主题颜色
编辑 `ppt/theme/theme1.xml` 并修改 `<a:clrScheme>` 部分：
```xml
<a:clrScheme name="Office">
  <a:dk1>
    <a:sysClr val="windowText" lastClr="000000"/>
  </a:dk1>
  <a:lt1>
    <a:sysClr val="window" lastClr="FFFFFF"/>
  </a:lt1>
  <!-- 更多颜色定义 -->
</a:clrScheme>
```

#### 替换图像
1. 用新图像替换 `ppt/media/` 中的图像文件
2. 更新引用该图像的 XML 文件中的关系 ID

## 高级技巧

### 使用 Sharp 进行图像处理
Sharp 是一个高性能的 Node.js 图像处理库。在处理演示文稿时使用它：

```javascript
const sharp = require('sharp');

// 将 SVG 转换为 PNG（PowerPoint 不支持 SVG）
await sharp('icon.svg')
  .png()
  .toFile('icon.png');

// 调整图像大小
await sharp('image.jpg')
  .resize(400, 300)
  .toFile('resized.jpg');

// 创建带有渐变的背景
await sharp({
  create: {
    width: 720,
    height: 405,
    channels: 4,
    background: { r: 28, g: 40, b: 51, alpha: 1 }
  }
})
  .toFile('background.png');
```

### 自动化设计决策
当为用户创建设计时：

1. **分析输入**：
   - 演示文稿的主题是什么？
   - 是否有现有的品牌指南或颜色？
   - 目标受众是谁？
   - 语气应该是正式的还是随意的？

2. **做出明确的选择**：
   - 从示例调色板中选择或创建自定义调色板
   - 选择补充的字体组合
   - 决定布局模式（保守型、现代型、大胆型）
   - 记录你的推理以便用户可以理解

3. **实施并验证**：
   - 创建初始版本
   - 生成缩略图进行视觉检查
   - 根据需要进行调整
   - 提供替代方案（如果适用）

### 处理复杂布局
对于具有多个元素的复杂幻灯片：

1. **从线框图开始**：在代码之前绘制布局草图
2. **使用 CSS Grid 或 Flexbox**：实现精确的定位
3. **分层内容**：先放置背景元素，然后是主要内容，最后是装饰元素
4. **测试响应式设计**：确保布局在不同尺寸下都能正常工作

## 故障排除

### 常见问题

**问题**：文本在 PowerPoint 中被截断
**解决方案**：增加 HTML 中的边距或减小字体大小。确保文本框有足够的填充。

**问题**：图像未显示或变形
**解决方案**：检查图像尺寸和纵横比。使用 Sharp 在导入前调整图像大小。

**问题**：颜色看起来不正确
**解决方案**：验证十六进制颜色代码。某些颜色可能需要调整为 sRGB 色彩空间。

**问题**：图表未正确对齐
**解决方案**：确保占位符在 HTML 中具有明确的尺寸。使用绝对定位进行精确放置。

**问题**：字体未正确渲染
**解决方案**：坚持使用网络安全字体。如果使用自定义字体，请确保它们嵌入在 PowerPoint 中。

### 调试技巧

1. **检查中间 HTML**：在转换之前查看 HTML 文件以识别问题
2. **使用浏览器开发工具**：在 HTML 中检查 CSS 布局
3. **逐个元素验证**：一次添加一个元素以隔离问题
4. **比较工作示例**：参考已知能正常工作的演示文稿

## 最佳实践

### 性能
- 优化图像大小以减少文件大小
- 限制每张幻灯片的动画和过渡效果数量
- 对重复元素使用母版幻灯片
- 避免过高分辨率的图像（通常 150 DPI 足够）

### 可访问性
- 确保文本与背景的对比度足够（至少 4.5:1）
- 为图像提供替代文本
- 使用语义化 HTML 结构
- 避免仅依靠颜色传达信息

### 一致性
- 在所有幻灯片中使用相同的字体层次结构
- 保持统一的间距和对齐方式
- 在整个演示文稿中重复颜色方案
- 使用一致的图像样式和处理方式

### 维护
- 为复杂的演示文稿记录设计决策
- 保留源 HTML 文件以便将来编辑
- 注释自定义脚本和修改
- 版本控制演示文稿迭代

## 工具参考

### html2pptx
将 HTML 幻灯片转换为 PowerPoint 的主要工具。

**用法**：
```javascript
const html2pptx = require('./scripts/html2pptx.js');

async function createPresentation() {
  const pptx = new PptxGenJS();
  
  // 处理每张幻灯片
  for (let i = 1; i <= numSlides; i++) {
    const html = fs.readFileSync(`slide${i}.html`, 'utf-8');
    await html2pptx(html, pptx);
  }
  
  // 保存
  await pptx.writeFile({ fileName: 'output.pptx' });
}
```

### Sharp
用于图像处理和转换的高性能 Node.js 库。

**常见操作**：
- SVG 转 PNG
- 调整图像大小
- 创建渐变和背景
- 图像格式转换

### Python-pptx
用于以编程方式创建和操作 PowerPoint 文件的 Python 库。

**最佳用途**：
- 使用模板创建演示文稿
- 批量生成幻灯片
- 数据驱动的演示文稿
- 自动化报告

### Markitdown
用于从 Office 文档提取文本内容的工具。

**用法**：
```bash
python -m markitdown presentation.pptx
```

## 示例场景

### 场景 1：创建公司季度汇报
1. 分析内容：财务数据、团队更新、未来目标
2. 选择专业调色板（Classic Blue 或 Charcoal & Red）
3. 使用保守的布局，重点是清晰度
4. 为图表和数据可视化使用双列布局
5. 包含带有公司品牌的标题幻灯片

### 场景 2：设计创意作品集
1. 分析内容：视觉作品、案例研究、创意过程
2. 选择大胆的调色板（Retro Rainbow 或 Deep Purple & Emerald）
3. 使用全出血图像和最小文本
4. 实施独特的布局模式（不对称、旋转元素）
5. 强调视觉冲击力而非文本密度

### 场景 3：编辑现有模板
1. 解包提供的模板
2. 识别要更新的元素（颜色、字体、图像）
3. 进行有针对性的 XML 编辑
4. 替换媒体资产
5. 重新打包并验证

## 资源

- [PptxGenJS 文档](https://gitbrent.github.io/PptxGenJS/)
- [Python-pptx 文档](https://python-pptx.readthedocs.io/)
- [Sharp 文档](https://sharp.pixelplumbing.com/)
- [Office Open XML 文档](https://docs.microsoft.com/en-us/office/open-xml/)
