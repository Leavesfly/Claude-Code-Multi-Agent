---
name: docx
description: "全面的文档创建、编辑和分析，支持跟踪更改、评论、格式保留和文本提取。当 Claude 需要处理专业文档（.docx 文件）时：(1) 创建新文档，(2) 修改或编辑内容，(3) 处理跟踪更改，(4) 添加评论，或任何其他文档任务"
license: Proprietary. LICENSE.txt has complete terms
---

# DOCX 创建、编辑与分析

## 概述

用户可能会要求你创建、编辑或分析 .docx 文件的内容。一个 .docx 文件本质上是一个 ZIP 档案，里面包含可读可改的 XML 文件和其他资源。不同任务有不同的工具与 workflow 可用。

## Workflow 决策树

### 读取/分析内容
使用下方的 "Text extraction" 或 "Raw XML access" 章节

### 创建新文档
使用 "Creating a new Word document" workflow

### 编辑现有文档
- **你自己的文档 + 简单修改**
  使用 "Basic OOXML editing" workflow

- **他人的文档**
  使用 **"Redlining workflow"**（默认推荐）

- **法律、学术、商业或政府文档**
  使用 **"Redlining workflow"**（强制）

## 读取与分析内容

### 文本提取
如果只需要读取文档中的文本内容，应该用 pandoc 把文档转成 markdown。Pandoc 对保留文档结构支持很好，并且能显示 tracked changes：

```bash
# Convert document to markdown with tracked changes
pandoc --track-changes=all path-to-file.docx -o output.md
# Options: --track-changes=accept/reject/all
```

### 原始 XML 访问
以下场景需要访问原始 XML：comments、复杂格式、文档结构、嵌入媒体、以及元数据。对这些特性，你需要先解包（unpack）文档，然后读取它的原始 XML 内容。

#### 解包文件
`python ooxml/scripts/unpack.py <office_file> <output_directory>`

#### 关键文件结构
* `word/document.xml` —— 主文档内容
* `word/comments.xml` —— 在 document.xml 中被引用的 comments
* `word/media/` —— 嵌入的图像和媒体文件
* Tracked changes 使用 `<w:ins>`（插入）和 `<w:del>`（删除）标签

## 创建新 Word 文档

从零创建新 Word 文档时，使用 **docx-js**，它允许你用 JavaScript/TypeScript 创建 Word 文档。

### Workflow
1. **强制 —— 完整读取整个文件**：完整读取 [`docx-js.md`](docx-js.md)（约 500 行），从头到尾。**不要对该文件设置任何范围限制。** 在开始创建文档前，读取完整内容以获取详细语法、关键格式规则与最佳实践。
2. 使用 Document、Paragraph、TextRun 等组件创建 JavaScript/TypeScript 文件（你可以假设依赖都装好了；如果没装，参见下方 dependencies 章节）
3. 用 Packer.toBuffer() 导出为 .docx

## 编辑现有 Word 文档

编辑现有 Word 文档时，使用 **Document library**（一个用于 OOXML 操作的 Python 库）。该库会自动处理基础设施搭建，并提供文档操作相关的方法。对于复杂场景，你也可以通过该库直接访问底层 DOM。

### Workflow
1. **强制 —— 完整读取整个文件**：完整读取 [`ooxml.md`](ooxml.md)（约 600 行），从头到尾。**不要对该文件设置任何范围限制。** 读取完整内容以获取 Document library API 和直接编辑文档文件的 XML 模式。
2. 解包文档：`python ooxml/scripts/unpack.py <office_file> <output_directory>`
3. 基于 Document library 创建并运行一个 Python 脚本（参见 ooxml.md 中的 "Document Library" 章节）
4. 打包最终文档：`python ooxml/scripts/pack.py <input_directory> <office_file>`

Document library 同时提供了常用操作的高阶方法以及面向复杂场景的 DOM 直接访问能力。

## 文档评审的 Redlining workflow

本 workflow 让你先用 markdown 规划全面的 tracked changes，再在 OOXML 中落地。**关键**：要做到完整的 tracked changes，你必须系统地落地**所有**修改。

**批处理策略**：把相关修改归为 3-10 个一批。这能让 debug 可控，同时保持效率。每批测试通过后再推进下一批。

**原则：最小化、精准的编辑**
落地 tracked changes 时，只标注真正发生变化的文本。重复标注未变的文本会让修改难以 review 也不专业。把替换拆成：[未变文本] + [删除] + [插入] + [未变文本]。对未变部分，保留原 run 的 RSID —— 方法是从原文中提取出 `<w:r>` 元素并复用。

示例 —— 把一句话中的 "30 days" 改为 "60 days"：
```python
# BAD - Replaces entire sentence
'<w:del><w:r><w:delText>The term is 30 days.</w:delText></w:r></w:del><w:ins><w:r><w:t>The term is 60 days.</w:t></w:r></w:ins>'

# GOOD - Only marks what changed, preserves original <w:r> for unchanged text
'<w:r w:rsidR="00AB12CD"><w:t>The term is </w:t></w:r><w:del><w:r><w:delText>30</w:delText></w:r></w:del><w:ins><w:r><w:t>60</w:t></w:r></w:ins><w:r w:rsidR="00AB12CD"><w:t> days.</w:t></w:r>'
```

### Tracked changes workflow

1. **获取 markdown 表示**：把文档转为 markdown，保留 tracked changes：
   ```bash
   pandoc --track-changes=all path-to-file.docx -o current.md
   ```

2. **识别并分组修改**：通读文档，识别所有需要的修改，并归并成逻辑批次：

   **定位方法**（用于在 XML 中找到修改点）：
   - Section/heading 编号（如 "Section 3.2"、"Article IV"）
   - 段落标识（如有编号）
   - 带唯一周边文本的 grep 模式
   - 文档结构（如 "first paragraph"、"signature block"）
   - **不要使用 markdown 行号** —— 它们与 XML 结构不对应

   **批次组织**（每批 3-10 个相关修改）：
   - 按 section：Batch 1: Section 2 amendments、Batch 2: Section 5 updates
   - 按类型：Batch 1: Date corrections、Batch 2: Party name changes
   - 按复杂度：先做简单文本替换，再搞复杂的结构性修改
   - 顺序：Batch 1: Pages 1-3、Batch 2: Pages 4-6

3. **阅读文档并解包**：
   - **强制 —— 完整读取整个文件**：完整读取 [`ooxml.md`](ooxml.md)（约 600 行），从头到尾。**不要对该文件设置任何范围限制。** 重点关注 "Document Library" 和 "Tracked Change Patterns" 章节。
   - **解包文档**：`python ooxml/scripts/unpack.py <file.docx> <dir>`
   - **记下推荐的 RSID**：unpack 脚本会建议一个可用于 tracked changes 的 RSID，复制下来在第 4b 步用。

4. **分批落地修改**：按逻辑（按 section、按类型或按相邻位置）分组，并在同一个脚本中一起实现。这样做：
   - 更方便 debug（批次越小，越容易隔离错误）
   - 支持渐进推进
   - 保持效率（每批 3-10 个修改效果不错）

   **建议的分批方式：**
   - 按文档 section（如 "Section 3 changes"、"Definitions"、"Termination clause"）
   - 按修改类型（如 "Date changes"、"Party name updates"、"Legal term replacements"）
   - 按位置相邻（如 "Changes on pages 1-3"、"Changes in first half of document"）

   对于每批相关修改：

   **a. 把文本映射到 XML**：在 `word/document.xml` 里 grep 这段文本，确认它是如何被拆到各个 `<w:r>` 元素里的。

   **b. 编写并运行脚本**：用 `get_node` 找节点，实现修改，然后 `doc.save()`。模式参见 ooxml.md 的 **"Document Library"** 章节。

   **注意**：写脚本前务必先 grep 一次 `word/document.xml`，拿到当前行号并确认文本内容。每跑一次脚本，行号就会变。

5. **打包文档**：所有批次完成后，把解包目录转回 .docx：
   ```bash
   python ooxml/scripts/pack.py unpacked reviewed-document.docx
   ```

6. **最终校验**：对完整文档做一次全面检查：
   - 把最终文档转为 markdown：
     ```bash
     pandoc --track-changes=all reviewed-document.docx -o verification.md
     ```
   - 核对所有修改是否都落地正确：
     ```bash
     grep "original phrase" verification.md  # Should NOT find it
     grep "replacement phrase" verification.md  # Should find it
     ```
   - 检查是否引入了非预期的改动


## 把文档转成图片

要对 Word 文档做视觉分析，通过两步法转成图片：

1. **DOCX 转 PDF**：
   ```bash
   soffice --headless --convert-to pdf document.docx
   ```

2. **PDF 页面转 JPEG 图片**：
   ```bash
   pdftoppm -jpeg -r 150 document.pdf page
   ```
   会生成 `page-1.jpg`、`page-2.jpg` 等文件。

参数：
- `-r 150`：把分辨率设为 150 DPI（可在质量与体积之间调节）
- `-jpeg`：输出 JPEG 格式（想要 PNG 就用 `-png`）
- `-f N`：起始页（如 `-f 2` 从第 2 页开始）
- `-l N`：结束页（如 `-l 5` 到第 5 页为止）
- `page`：输出文件名前缀

指定范围示例：
```bash
pdftoppm -jpeg -r 150 -f 2 -l 5 document.pdf page  # Converts only pages 2-5
```

## 代码风格准则
**重要**：生成 DOCX 操作相关代码时：
- 写简洁的代码
- 避免啰嗦的变量名和冗余操作
- 避免不必要的 print 语句

## 依赖

需要的依赖（如果没装，请自行安装）：

- **pandoc**：`sudo apt-get install pandoc`（用于文本提取）
- **docx**：`npm install -g docx`（用于创建新文档）
- **LibreOffice**：`sudo apt-get install libreoffice`（用于 PDF 转换）
- **Poppler**：`sudo apt-get install poppler-utils`（用于 pdftoppm 把 PDF 转图片）
- **defusedxml**：`pip install defusedxml`（用于安全的 XML 解析）