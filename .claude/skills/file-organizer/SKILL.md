---
name: file-organizer
description: 通过理解上下文、查找重复项、建议更好的结构并自动化清理任务，智能地组织您计算机上的文件和文件夹。减少认知负担，无需手动操作即可保持数字工作区整洁。
---

# File Organizer（文件整理助手）

本 skill 扮演你的个人整理助手，帮你在电脑里维持干净、有条理的文件结构，免去持续手动整理带来的心智负担。

## 何时使用本 skill

- 你的 Downloads 文件夹一团乱
- 文件分散在各处导致找不到
- 有重复文件在占空间
- 你的文件夹结构不再合理
- 想养成更好的整理习惯
- 开新项目时需要一个好结构
- 归档旧项目前需要清理

## 本 skill 能做什么

1. **分析当前结构**：审视文件夹和文件，了解你有哪些东西
2. **找出重复**：识别系统中的重复文件
3. **建议组织方式**：基于内容提出合理的文件夹结构
4. **自动化清理**：在你批准后移动、重命名和整理文件
5. **保留上下文**：基于文件类型、日期和内容做出明智决定
6. **减少杂乱**：识别你可能不再需要的旧文件

## 使用方式

### 从 home 目录进入

```
cd ~
```

然后运行 Claude Code 并请求帮助：

```
Help me organize my Downloads folder
```

```
Find duplicate files in my Documents folder
```

```
Review my project directories and suggest improvements
```

### 针对性整理任务

```
Organize these downloads into proper folders based on what they are
```

```
Find duplicate files and help me decide which to keep
```

```
Clean up old files I haven't touched in 6+ months
```

```
Create a better folder structure for my [work/projects/photos/etc]
```

## 操作说明

当用户请求文件整理帮助时：

1. **了解范围**
   
   提出澄清性问题：
   - 需要整理哪个目录？（Downloads、Documents、整个 home 目录？）
   - 主要问题是什么？（找不到东西、重复、太乱、没结构？）
   - 有需要避开的文件或文件夹吗？（正在进行的项目、敏感数据？）
   - 整理的力度如何？（保守 vs. 全面清理）

2. **分析当前状态**
   
   审视目标目录：
   ```bash
   # Get overview of current structure
   ls -la [target_directory]
   
   # Check file types and sizes
   find [target_directory] -type f -exec file {} \; | head -20
   
   # Identify largest files
   du -sh [target_directory]/* | sort -rh | head -20
   
   # Count file types
   find [target_directory] -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn
   ```
   
   总结发现：
   - 文件和文件夹总数
   - 文件类型分布
   - 大小分布
   - 日期范围
   - 明显的整理问题

3. **识别整理模式**
   
   基于这些文件，确定合理的分组：
   
   **按类型**：
   - Documents（PDF、DOCX、TXT）
   - Images（JPG、PNG、SVG）
   - Videos（MP4、MOV）
   - Archives（ZIP、TAR、DMG）
   - Code/Projects（包含代码的目录）
   - Spreadsheets（XLSX、CSV）
   - Presentations（PPTX、KEY）
   
   **按用途**：
   - 工作 vs. 个人
   - 活跃 vs. 归档
   - 项目专属
   - 参考资料
   - 临时/草稿文件
   
   **按日期**：
   - 当前年/月
   - 往年
   - 非常老（适合归档）

4. **查找重复**
   
   当被请求时，搜索重复文件：
   ```bash
   # Find exact duplicates by hash
   find [directory] -type f -exec md5 {} \; | sort | uniq -d
   
   # Find files with same name
   find [directory] -type f -printf '%f\n' | sort | uniq -d
   
   # Find similar-sized files
   find [directory] -type f -printf '%s %p\n' | sort -n
   ```
   
   对每组重复：
   - 列出所有文件路径
   - 显示大小和修改时间
   - 建议保留哪一份（通常是最新或命名最好的）
   - **重要**：删除前始终请求确认

5. **提出整理方案**
   
   在做改动前，先给出清晰的方案：
   
   ```markdown
   # Organization Plan for [Directory]
   
   ## Current State
   - X files across Y folders
   - [Size] total
   - File types: [breakdown]
   - Issues: [list problems]
   
   ## Proposed Structure
   
   ```
   [Directory]/
   ├── Work/
   │   ├── Projects/
   │   ├── Documents/
   │   └── Archive/
   ├── Personal/
   │   ├── Photos/
   │   ├── Documents/
   │   └── Media/
   └── Downloads/
       ├── To-Sort/
       └── Archive/
   ```
   
   ## Changes I'll Make
   
   1. **Create new folders**: [list]
   2. **Move files**:
      - X PDFs → Work/Documents/
      - Y images → Personal/Photos/
      - Z old files → Archive/
   3. **Rename files**: [any renaming patterns]
   4. **Delete**: [duplicates or trash files]
   
   ## Files Needing Your Decision
   
   - [List any files you're unsure about]
   
   Ready to proceed? (yes/no/modify)
   ```

6. **执行整理**
   
   获得批准后，有条理地整理：
   
   ```bash
   # Create folder structure
   mkdir -p "path/to/new/folders"
   
   # Move files with clear logging
   mv "old/path/file.pdf" "new/path/file.pdf"
   
   # Rename files with consistent patterns
   # Example: "YYYY-MM-DD - Description.ext"
   ```
   
   **重要规则**：
   - 删除任何东西前都要确认
   - 记录所有移动，以便必要时回滚
   - 保留原有的修改时间
   - 优雅地处理文件名冲突
   - 遇到意外情况时停下来询问

7. **提供总结和维护建议**
   
   整理之后：
   
   ```markdown
   # Organization Complete! ✨
   
   ## What Changed
   
   - Created [X] new folders
   - Organized [Y] files
   - Freed [Z] GB by removing duplicates
   - Archived [W] old files
   
   ## New Structure
   
   [Show the new folder tree]
   
   ## Maintenance Tips
   
   To keep this organized:
   
   1. **Weekly**: Sort new downloads
   2. **Monthly**: Review and archive completed projects
   3. **Quarterly**: Check for new duplicates
   4. **Yearly**: Archive old files
   
   ## Quick Commands for You
   
   ```bash
   # Find files modified this week
   find . -type f -mtime -7
   
   # Sort downloads by type
   [custom command for their setup]
   
   # Find duplicates
   [custom command]
   ```
   
   Want to organize another folder?
   ```

## 示例

### 示例 1：整理 Downloads（灵感来自 Justin Dielmann）

**User**: "My Downloads folder is a mess with 500+ files. Help me organize it."

**过程**：
1. 分析 Downloads 文件夹
2. 找到模式：工作文档、个人照片、安装包、零散 PDF
3. 提出结构：
   - Downloads/
     - Work/
     - Personal/
     - Installers/（DMG、PKG 文件）
     - Archive/
     - ToSort/（需要决策的东西）
4. 请求确认
5. 基于内容和名字智能移动文件
6. 结果：500 个文件 → 5 个整理好的文件夹

### 示例 2：查找并移除重复

**User**: "Find duplicate files in my Documents and help me decide which to keep."

**输出**：
```markdown
# Found 23 Sets of Duplicates (156 MB total)

## Duplicate Set 1: "proposal.pdf"
- `/Documents/proposal.pdf` (2.3 MB, modified: 2024-03-15)
- `/Documents/old/proposal.pdf` (2.3 MB, modified: 2024-03-15)
- `/Desktop/proposal.pdf` (2.3 MB, modified: 2024-03-10)

**Recommendation**: Keep `/Documents/proposal.pdf` (most recent in correct location)
Delete the other 2 copies?

[Continue for all duplicates...]
```

### 示例 3：重构 Projects 文件夹

**User**: "Review my ~/Projects directory and suggest improvements."

**输出**：
```markdown
# Analysis of ~/Projects

## Current Structure Issues
- Mix of active and archived projects (3+ years old)
- No consistent naming convention
- Some projects at root, others in random subfolders
- Duplicate folders (project-name, project-name-old, project-name-v2)

## Proposed Structure

```
Projects/
├── Active/
│   ├── client-work/
│   ├── side-projects/
│   └── learning/
├── Archive/
│   ├── 2022/
│   ├── 2023/
│   └── 2024/
└── Templates/
```

## Specific Changes
1. Move 12 projects not touched since 2022 → Archive/
2. Consolidate 4 duplicate project folders
3. Rename projects to consistent format: "client-name-project-name"
4. Create Archive for old work

Want me to implement this?
```

### 示例 4：按日期整理照片

**User**: "Organize my photo folders by year and month."

**输出**：创建如下结构：
```
Photos/
├── 2023/
│   ├── 01-January/
│   ├── 02-February/
│   └── ...
├── 2024/
│   ├── 01-January/
│   └── ...
└── Unsorted/
```

然后基于 EXIF 数据或文件修改时间移动照片。

## 常见整理任务

### Downloads 清理
```
Organize my Downloads folder - move documents to Documents, 
images to Pictures, keep installers separate, and archive files 
older than 3 months.
```

### Project 整理
```
Review my Projects folder structure and help me separate active 
projects from old ones I should archive.
```

### 重复清理
```
Find all duplicate files in my Documents folder and help me 
decide which ones to keep.
```

### Desktop 清理
```
My Desktop is covered in files. Help me organize everything into 
my Documents folder properly.
```

### 照片整理
```
Organize all photos in this folder by date (year/month) based 
on when they were taken.
```

### 工作/个人分离
```
Help me separate my work files from personal files across my 
Documents folder.
```

## Pro Tips（实用技巧）

1. **从小处着手**：从一个乱七八糟的文件夹（如 Downloads）开始，建立信任
2. **定期维护**：每周对 Downloads 做一次清理
3. **一致命名**：重要文件用 "YYYY-MM-DD - Description" 格式
4. **大胆归档**：把旧项目移到 Archive，而不是删除
5. **让活跃的独立开**：在活跃工作和归档工作之间保持清晰边界
6. **相信流程**：让 Claude 来承担"东西该放哪"的认知负担

## 最佳实践

### 文件夹命名
- 使用清晰、有描述性的名字
- 避免空格（用连字符或下划线）
- 要具体："client-proposals" 而不是 "docs"
- 用前缀排序："01-current"、"02-archive"

### 文件命名
- 加日期："2024-10-17-meeting-notes.md"
- 有描述性："q3-financial-report.xlsx"
- 不要在文件名里带版本号（用版本控制代替）
- 去掉下载残留："document-final-v2 (1).pdf" → "document.pdf"

### 何时归档
- 6 个月以上没动过的项目
- 可能以后需要参考的已完成工作
- 迁移到新系统后的旧版本
- 不太想删但又想清理出来的文件（先归档）

## 相关用例

- 新电脑上配置整理结构
- 为备份/归档准备文件
- 存储清理前的预整理
- 整理团队共享文件夹
- 搭建新项目目录结构

