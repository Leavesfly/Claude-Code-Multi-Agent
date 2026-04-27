---
name: xlsx
description: "全面的电子表格创建、编辑和分析，支持公式、格式、数据分析和可视化。当 Claude 需要处理电子表格（.xlsx、.xlsm、.csv、.tsv 等）时：(1) 创建带有公式和格式的新电子表格，(2) 读取或分析数据，(3) 在保留公式的同时修改现有电子表格，(4) 在电子表格中进行数据分析和可视化，或 (5) 重新计算公式"
license: Proprietary. LICENSE.txt has complete terms
---

# 输出要求

## 所有 Excel 文件

### 零公式错误
- 所有 Excel 模型交付时**必须**零公式错误（#REF!、#DIV/0!、#VALUE!、#N/A、#NAME?）

### 保留现有模板（修改现有模板时）
- 修改文件时，研究并**完全匹配**现有的格式、风格和约定
- 绝不对已有成熟模式的文件强加标准化格式
- 现有模板约定**始终**优先于本指南

## 财务模型

### 颜色编码标准
除非用户或现有模板另有规定

#### 行业标准颜色约定
- **蓝色文本（RGB: 0,0,255）**：硬编码输入，以及用户在不同场景下会改动的数值
- **黑色文本（RGB: 0,0,0）**：所有公式与计算
- **绿色文本（RGB: 0,128,0）**：从同一工作簿的其他 worksheet 拉取的链接
- **红色文本（RGB: 255,0,0）**：指向其他文件的外部链接
- **黄色背景（RGB: 255,255,0）**：需关注的关键假设或需要更新的单元格

### 数字格式标准

#### 必要的格式规则
- **年份**：格式化为文本字符串（如 "2024" 而非 "2,024"）
- **货币**：使用 $#,##0 格式；**始终**在表头指明单位（如 "Revenue ($mm)"）
- **零值**：用数字格式让所有零显示为 "-"（包括百分比，如 "$#,##0;($#,##0);-"）
- **百分比**：默认 0.0% 格式（一位小数）
- **倍数**：估值倍数（EV/EBITDA、P/E）格式化为 0.0x
- **负数**：使用括号 (123)，不使用负号 -123

### 公式构造规则

#### 假设值放置
- 将所有假设（增长率、利润率、倍数等）放在独立的假设单元格里
- 公式中用单元格引用，而非硬编码数值
- 示例：用 =B5*(1+$B$6) 而非 =B5*1.05

#### 公式错误预防
- 核对所有单元格引用是否正确
- 检查区间是否存在 off-by-one 错误
- 确保所有预测期的公式一致
- 用边界情况测试（零值、负数）
- 确认没有非预期的循环引用

#### 硬编码值的文档要求
- 加注释或在相邻单元格（若位于表格末尾）注明。格式："Source: [System/Document], [Date], [Specific Reference], [URL if applicable]"
- 示例：
  - "Source: Company 10-K, FY2024, Page 45, Revenue Note, [SEC EDGAR URL]"
  - "Source: Company 10-Q, Q2 2025, Exhibit 99.1, [SEC EDGAR URL]"
  - "Source: Bloomberg Terminal, 8/15/2025, AAPL US Equity"
  - "Source: FactSet, 8/20/2025, Consensus Estimates Screen"

# XLSX 创建、编辑与分析

## 概述

用户可能会要求你创建、编辑或分析 .xlsx 文件的内容。不同任务有不同的工具与 workflow 可用。

## 重要前置条件

**公式重算需要 LibreOffice**：你可以假设 LibreOffice 已安装，用于通过 `recalc.py` 脚本重算公式值。该脚本会在首次运行时自动配置 LibreOffice。

## 读取与分析数据

### 用 pandas 做数据分析
对于数据分析、可视化和基本操作，使用 **pandas**，它提供了强大的数据操作能力：

```python
import pandas as pd

# Read Excel
df = pd.read_excel('file.xlsx')  # Default: first sheet
all_sheets = pd.read_excel('file.xlsx', sheet_name=None)  # All sheets as dict

# Analyze
df.head()      # Preview data
df.info()      # Column info
df.describe()  # Statistics

# Write Excel
df.to_excel('output.xlsx', index=False)
```

## Excel 文件 workflow

## 关键：使用公式，不要硬编码数值

**始终使用 Excel 公式，而不是在 Python 中算好数值再硬编码进去。** 这样才能保证表格保持动态、可更新。

### ❌ 错误 —— 硬编码计算后的数值
```python
# Bad: Calculating in Python and hardcoding result
total = df['Sales'].sum()
sheet['B10'] = total  # Hardcodes 5000

# Bad: Computing growth rate in Python
growth = (df.iloc[-1]['Revenue'] - df.iloc[0]['Revenue']) / df.iloc[0]['Revenue']
sheet['C5'] = growth  # Hardcodes 0.15

# Bad: Python calculation for average
avg = sum(values) / len(values)
sheet['D20'] = avg  # Hardcodes 42.5
```

### ✅ 正确 —— 使用 Excel 公式
```python
# Good: Let Excel calculate the sum
sheet['B10'] = '=SUM(B2:B9)'

# Good: Growth rate as Excel formula
sheet['C5'] = '=(C4-C2)/C2'

# Good: Average using Excel function
sheet['D20'] = '=AVERAGE(D2:D19)'
```

这适用于**所有**计算 —— 合计、百分比、比率、差额等。当源数据变化时，表格应能自动重算。

## 常见 workflow
1. **选工具**：pandas 处理数据，openpyxl 处理公式/格式
2. **创建/加载**：新建工作簿或加载已有文件
3. **修改**：增删改数据、公式和格式
4. **保存**：写入文件
5. **重算公式（使用公式时强制执行）**：用 recalc.py 脚本
   ```bash
   python recalc.py output.xlsx
   ```
6. **验证并修复错误**：
   - 脚本会返回包含错误详情的 JSON
   - 如果 `status` 为 `errors_found`，查看 `error_summary` 找出具体错误类型和位置
   - 修复识别出的错误后再重算
   - 常见错误：
     - `#REF!`：无效的单元格引用
     - `#DIV/0!`：除以零
     - `#VALUE!`：公式中数据类型错误
     - `#NAME?`：公式名未识别

### 创建新 Excel 文件

```python
# Using openpyxl for formulas and formatting
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment

wb = Workbook()
sheet = wb.active

# Add data
sheet['A1'] = 'Hello'
sheet['B1'] = 'World'
sheet.append(['Row', 'of', 'data'])

# Add formula
sheet['B2'] = '=SUM(A1:A10)'

# Formatting
sheet['A1'].font = Font(bold=True, color='FF0000')
sheet['A1'].fill = PatternFill('solid', start_color='FFFF00')
sheet['A1'].alignment = Alignment(horizontal='center')

# Column width
sheet.column_dimensions['A'].width = 20

wb.save('output.xlsx')
```

### 编辑现有 Excel 文件

```python
# Using openpyxl to preserve formulas and formatting
from openpyxl import load_workbook

# Load existing file
wb = load_workbook('existing.xlsx')
sheet = wb.active  # or wb['SheetName'] for specific sheet

# Working with multiple sheets
for sheet_name in wb.sheetnames:
    sheet = wb[sheet_name]
    print(f"Sheet: {sheet_name}")

# Modify cells
sheet['A1'] = 'New Value'
sheet.insert_rows(2)  # Insert row at position 2
sheet.delete_cols(3)  # Delete column 3

# Add new sheet
new_sheet = wb.create_sheet('NewSheet')
new_sheet['A1'] = 'Data'

wb.save('modified.xlsx')
```

## 重算公式

openpyxl 创建或修改的 Excel 文件里，公式是以字符串形式存在的，并不包含计算后的数值。用自带的 `recalc.py` 脚本重算公式：

```bash
python recalc.py <excel_file> [timeout_seconds]
```

示例：
```bash
python recalc.py output.xlsx 30
```

该脚本会：
- 首次运行时自动配置 LibreOffice macro
- 重算所有 sheet 中的所有公式
- 扫描所有单元格，识别 Excel 错误（#REF!、#DIV/0! 等）
- 返回带有详细错误位置与计数的 JSON
- 同时支持 Linux 和 macOS

## 公式校验清单

确保公式正确运行的快速检查：

### 必做校验
- [ ] **抽测 2-3 个引用**：搭完整模型前先验证能拉到正确的值
- [ ] **列映射**：确认 Excel 列号对应（如第 64 列 = BL，不是 BK）
- [ ] **行偏移**：注意 Excel 行从 1 开始（DataFrame 第 5 行 = Excel 第 6 行）

### 常见坑
- [ ] **NaN 处理**：用 `pd.notna()` 检查空值
- [ ] **最右侧列**：FY 数据常在第 50 列以后
- [ ] **多处匹配**：搜索所有出现的位置，不只是第一处
- [ ] **除以零**：公式里用 `/` 前先检查分母（#DIV/0!）
- [ ] **错误引用**：核对所有单元格引用指向了预期单元格（#REF!）
- [ ] **跨 sheet 引用**：跨表链接使用正确格式（Sheet1!A1）

### 公式测试策略
- [ ] **小步快跑**：先在 2-3 个单元格上测试公式，再批量应用
- [ ] **核对依赖**：确认公式引用的所有单元格都存在
- [ ] **测边界值**：覆盖零、负数和超大数值

### 解读 recalc.py 输出
脚本返回带错误详情的 JSON：
```json
{
  "status": "success",           // or "errors_found"
  "total_errors": 0,              // Total error count
  "total_formulas": 42,           // Number of formulas in file
  "error_summary": {              // Only present if errors found
    "#REF!": {
      "count": 2,
      "locations": ["Sheet1!B5", "Sheet1!C10"]
    }
  }
}
```

## 最佳实践

### 库的选择
- **pandas**：适合数据分析、批量操作和简单的数据导出
- **openpyxl**：适合复杂格式、公式以及 Excel 专有特性

### 使用 openpyxl
- 单元格索引从 1 开始（row=1, column=1 指代 A1）
- 用 `data_only=True` 读取计算后的值：`load_workbook('file.xlsx', data_only=True)`
- **警告**：用 `data_only=True` 打开再保存会把公式替换成值，公式会永久丢失
- 处理大文件：读取用 `read_only=True`，写入用 `write_only=True`
- 公式会被保留但不会被求值 —— 用 recalc.py 更新值

### 使用 pandas
- 指定数据类型避免推断问题：`pd.read_excel('file.xlsx', dtype={'id': str})`
- 大文件只读特定列：`pd.read_excel('file.xlsx', usecols=['A', 'C', 'E'])`
- 正确处理日期：`pd.read_excel('file.xlsx', parse_dates=['date_column'])`

## 代码风格准则
**重要**：生成 Excel 操作相关的 Python 代码时：
- 写精简的代码，避免不必要的注释
- 避免啰嗦的变量名和冗余操作
- 避免不必要的 print 语句

**对于 Excel 文件本身**：
- 给有复杂公式或重要假设的单元格加注释
- 记录硬编码数值的数据来源
- 为关键计算和模型 section 加说明