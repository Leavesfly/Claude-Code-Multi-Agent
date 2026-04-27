# 配置机制

Git 行结束配置机制的详细解释。

## 目录

- [core.autocrlf](#coreautocrlf)
- [core.eol](#coreeol)
- [core.safecrlf](#coresafecrlf)
- [优先级层次结构](#precedence-hierarchy)
- [配置级别](#configuration-levels)

## 核心.autocrlf

Controls automatic line ending conversion.

**Values:**

|价值|结账时的行为|Commit 上的行为|使用案例|
| --- | --- | --- | --- |
|`真实`|换行 → 换行|回车换行→换行|Windows 开发人员（Git 代表 Windows 默认值）|
|`输入`|无转换|回车换行→换行|Mac/Linux 开发者|
|`假`|无转换|无转换|通过 .gitattributes 手动控制|

**“autocrlf=true”如何工作（Windows）：**

```text
Repository (LF)
     ↓ git checkout
  Convert LF → CRLF
     ↓
Working Directory (CRLF)
     ↓ git add/commit
  Convert CRLF → LF
     ↓
Repository (LF)
```

** `autocrlf=input` 的工作原理 (Mac/Linux)：**

```text
Repository (LF)
     ↓ git checkout
  No conversion
     ↓
Working Directory (LF)
     ↓ git add/commit
  Convert CRLF → LF (if present)
     ↓
Repository (LF)
```

## 核心文件

当“.gitattributes”未指定并且“autocrlf”不适用时，工作目录的后备行结束样式。

**数值：**

- `native` - 使用平台默认值（Windows 上的 CRLF，Unix 上的 LF）**[推荐使用 autocrlf=false]**
- `lf` - 始终使用 LF
- `crlf` - 始终使用 CRLF

**适用时：**

- 仅当 core.autocrlf=false 时使用
- 仅适用于具有“text”属性但没有显式“eol”属性的文件
- 被 `.gitattributes` `eol` 属性覆盖

## 核心.safecrlf

安全检查以防止不可逆的行结束转换。

**数值：**

- `true` - 拒绝会丢失数据的提交（严格）
- `warn` - 警告潜在的数据丢失，但允许 commit **[推荐]**
- `false` - 没有安全检查（危险）

**警告示例：**

```bash
$ git add file.txt
warning: in the working copy of 'file.txt', CRLF will be replaced by LF the next time Git touches it
```

这是信息性的 - Git 正在根据您的配置规范化文件。

## 优先级层次结构

**优先级顺序（从最高到最低）：**

1. **`.gitattributes` `eol` 属性** - 显式每个文件规则（最高优先级）
2. **`.gitattributes` `text` 属性** - 启用规范化
3. **`core.autocrlf`** - 自动转换（仅当未指定`eol`时）
4. **`core.eol`** - 后备默认值（仅当 `autocrlf=false` 并且没有属性时）

**关键发现：** `.gitattributes` `eol` 属性 **始终覆盖** `core.autocrlf`。

**例子：**

```bash
# Config: core.autocrlf=true
# .gitattributes: *.md text eol=lf

# Result: Markdown files will have LF in working directory
# The .gitattributes eol=lf takes precedence over autocrlf=true
```

## 配置级别

Git 配置可以设置为三个级别：

```bash
# System-wide (all users, all repos)
git config --system core.autocrlf true
# File: C:/Program Files/Git/etc/gitconfig (Windows)
# File: /etc/gitconfig (Unix)

# User/Global (current user, all repos)
git config --global core.autocrlf true
# File: ~/.gitconfig or ~/.config/git/config

# Repository/Local (current repo only)
git config --local core.autocrlf true
# File: .git/config (in repository root)
```

**优先级：**本地 > 全局 > 系统（本地设置覆盖全局/系统）

**最佳实践：** 在 **全局级别** 设置行结束配置，以确保存储库之间的一致性，仅在例外情况下使用 **本地级别**。
