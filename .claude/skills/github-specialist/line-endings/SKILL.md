---
name: line-endings
description: 面向跨平台开发团队的 Git line ending 配置完整指南。在配置 line ending、设置 .gitattributes、排查 line ending 问题、理解 core.autocrlf/core.eol/core.safecrlf、使用 Git LFS、在 repo 中规范化 line ending，或解决跨平台 line ending 冲突时使用。覆盖 Windows、macOS、Linux 和 WSL。包含决策树、workflow、最佳实践和真实场景。
allowed-tools: Read, Bash, Glob, Grep
---

# Git Line Endings

面向跨平台开发团队的 Git line ending 配置完整指南。

## 何时使用本 skill

在以下场景使用：

- **Line ending 任务** —— 配置 line ending、设置 .gitattributes、排查 line ending 问题、理解 core.autocrlf/core.eol/core.safecrlf、使用 Git LFS、在 repo 中规范化 line ending、或解决跨平台 line ending 冲突。覆盖 Windows、macOS、Linux 和 WSL
- **规划或设计** —— 需要关于 line ending 处理方式的指引
- **最佳实践** —— 想遵循已成型的模式与标准

**最后验证：** 2025-11-25
**最后审计：** 2025-11-25（完整 Type A 审计 —— 79/80 分，内容已通过 MCP server 对照 Git 官方文档验证）

## 概述

### 什么是 line ending？

line ending 是文本文件中标记行尾的不可见字符：

- **LF（Line Feed）**：`\n` —— Unix、Linux、macOS 使用（1 字节）
- **CRLF（Carriage Return + Line Feed）**：`\r\n` —— Windows 使用（2 字节）
- **CR（Carriage Return）**：`\r` —— 旧版 Mac OS 9 及更早（今天很少见）

### 为什么 line ending 很重要

**问题：**

```bash
# Windows developer creates file with CRLF
echo "#!/bin/bash" > script.sh

# Linux developer pulls and tries to run it
./script.sh
# Error: /bin/bash^M: bad interpreter: No such file or directory
```

**常见问题：**

1. shell script 无法执行（Unix 需要 LF，CRLF 会破坏 shebang 行）
2. 巨量 diff（仅 line ending 不同也会让每行都显示为改动）
3. 文件损坏（被当作文本处理的二进制文件 line ending 被篡改）
4. 构建失败（Makefile、配置文件可能需要特定的 line ending）
5. Git 冲突（line ending 差异引发 merge 冲突）

### Git 如何处理 line ending

Git 提供三种机制：

1. **配置项**（`core.autocrlf`、`core.eol`、`core.safecrlf`）—— 自动转换
2. **`.gitattributes` 文件** —— 按文件或按模式的显式规则（优先级最高）
3. **手动规范化**（`git add --renormalize`）—— 一次性修复

**Git 的设计哲学：**

- 仓库（index）：规范化的 line ending（通常是 LF）
- 工作目录：平台对应的 line ending（Windows 上 CRLF，Unix 上 LF）
- checkout 和 commit 时自动转换

---

## 快速开始

### 两种配置方式

#### 方式 1：传统（推荐）

**按平台配置，并带自动规范化：**

```bash
# Windows
git config --global core.autocrlf true
git config --global core.safecrlf warn

# Mac/Linux
git config --global core.autocrlf input
git config --global core.safecrlf warn
```

**优点：**

- ✅ 没有 .gitattributes 也能正常工作（外部 repo 安全）
- ✅ 是 Git for Windows 的默认（Windows 上零配置）
- ✅ 自动规范化可防止混合 line ending
- ✅ 行业标准

**何时使用：** 你在自己无法掌控的 repo 里工作（开源、客户、供应商 repo）

#### 方式 2：现代显式

**所有地方用同一套配置，依赖 .gitattributes：**

```bash
# All platforms
git config --global core.autocrlf false
git config --global core.eol native
git config --global core.safecrlf warn
```

**优点：**

- ✅ 所有地方同一套配置（团队一致性）
- ✅ 显式、可预测
- ✅ 现代最佳实践

**缺点：**

- ⚠️ **必须有 .gitattributes** —— 没有就会出问题
- ⚠️ 对外部 repo 不安全

**何时使用：** 你能掌控所有 repo，并能保证完善的 .gitattributes

### 推荐

如果你在混合环境中工作（内部 + 外部 repo），**使用方式 1**。详细对比见 [Configuration Approaches](references/configuration-approaches.md)。

---

## 决策树

完整决策树覆盖所有场景（repo 掌控度、平台选择、文件类型、故障排查），见 [Decision Tree](references/decision-tree.md)。

**快速决策：**

1. **你能掌控所有 repo 吗？** 不能 → 使用方式 1（传统）
2. **什么平台？** Windows：`autocrlf=true`；macOS/Linux：`autocrlf=input`
3. **有 .gitattributes 吗？** 有 → 两种方式都行；没有 → 只能用方式 1

---

## 理解 .gitattributes

`.gitattributes` 是 repo 级别的文件，显式声明 Git 如何处理特定文件。

**最小 .gitattributes：**

```gitattributes
# Auto-detect text files and normalize to LF in repository
* text=auto
```

**完整 .gitattributes：**

```gitattributes
# Default: auto-detect and normalize
* text=auto

# Documentation - LF everywhere
*.md text eol=lf
*.txt text eol=lf

# Shell scripts - MUST be LF (Unix requirement)
*.sh text eol=lf
*.bash text eol=lf

# PowerShell scripts - CRLF (Windows standard)
*.ps1 text eol=crlf
*.cmd text eol=crlf
*.bat text eol=crlf

# Configuration files - LF (cross-platform)
*.json text eol=lf
*.yml text eol=lf
.gitignore text eol=lf
.gitattributes text eol=lf

# Binary files - never convert
*.png binary
*.jpg binary
*.pdf binary
*.zip binary
*.exe binary
```

**为什么用 .gitattributes：**

- 显式规则提交到 repo
- 所有开发者得到相同行为
- 覆盖本地配置
- 自文档化

完整模式与 attribute 参考见 [.gitattributes Guide](references/gitattributes-guide.md)。

---

## 各平台配置

### Windows

```bash
# Check current config (should be default from Git for Windows)
git config --global --get core.autocrlf
# Expected: true

# If not set, configure explicitly
git config --global core.autocrlf true
git config --global core.safecrlf warn
```

**行为：**

- 工作目录中的文件：CRLF（Windows 标准）
- repo 中的文件：LF（跨平台标准）
- checkout 和 commit 时自动转换

Windows、macOS、Linux 和 WSL 的详细设置指南见 [Platform-Specific Configuration](references/platform-specific.md)。

---

## 常见问题与故障排查

### 问题：shell script 无法执行（`^M: bad interpreter`）

**错误：**

```bash
$ ./script.sh
bash: ./script.sh: /bin/bash^M: bad interpreter: No such file or directory
```

**根因：** shell script 带 CRLF line ending。Unix shell 需要 LF。

**立即修复：**

```bash
# Convert CRLF to LF
dos2unix script.sh

# Or with sed
sed -i 's/\r$//' script.sh

# Make executable
chmod +x script.sh
```

**永久修复（添加到 .gitattributes）：**

```gitattributes
# Shell scripts MUST have LF
*.sh text eol=lf
*.bash text eol=lf
```

```bash
# Normalize the script
git add --renormalize script.sh
git commit -m "Fix line endings in shell scripts"
```

### 问题：Git 把每一行都显示为改动

**根因：** line ending 变了（CRLF ↔ LF）。

**修复：**

```bash
# Check current line endings
git ls-files --eol file.txt

# Normalize to repository standard
git add --renormalize file.txt
git commit -m "Normalize line endings for file.txt"
```

完整的问题排查见 [Troubleshooting](references/troubleshooting.md)。

---

## 命令参考

**核心命令的快速参考：**

```bash
# View configuration
git config --list --show-origin | grep -E "autocrlf|eol|safecrlf"

# Check file attributes
git check-attr -a README.md

# Check line ending status
git ls-files --eol README.md

# Normalize files
git add --renormalize .

# Test line ending behavior
git ls-files --eol | grep "w/crlf" | grep "eol=lf"  # Find mismatches
```

完整命令列表见 [Commands Reference](references/commands-reference.md)。

---

## 最佳实践小结

1. **在你掌控的 repo 里始终使用 .gitattributes**
   - 显式的 line ending 规则
   - 团队一致性
   - 自文档化

2. **在入职材料中记录各平台配置**
   - Windows：确认 `autocrlf=true`
   - macOS/Linux：必须设置 `autocrlf=input`

3. **设置 core.safecrlf=warn 以保安全**
   - 对 line ending 转换发出警告
   - 尽早发现问题

4. **用 git ls-files --eol 测试**
   - 定期检查不一致
   - 找出混合的 line ending

5. **添加 .gitattributes 时做规范化**
   - 运行 `git add --renormalize .`
   - 审查并提交变更

详细指引见 [Best Practices](references/best-practices.md)。

---

## Git Large File System（LFS）

Git LFS 把大型二进制文件与主 repo 分开存储，以防 repo 膨胀。它与 line ending 配置相互独立，但可以一起使用。

完整的安装、配置、使用时机、GitHub 限制和迁移策略见 [Git LFS Guide](references/git-lfs.md)。

---

## 参考

**配置与设置：**

- **[Decision Tree](references/decision-tree.md)** —— 覆盖所有场景的完整决策流程图
- **[Configuration Mechanics](references/configuration-mechanics.md)** —— Core.autocrlf、core.eol、core.safecrlf 详解
- **[Configuration Approaches](references/configuration-approaches.md)** —— 方式 1 vs 方式 2 对比
- **[Platform-Specific Configuration](references/platform-specific.md)** —— Windows、macOS、Linux、WSL 设置指南

**实施：**

- **[.gitattributes Guide](references/gitattributes-guide.md)** —— 完整模式与 attribute 参考
- **[Git LFS Guide](references/git-lfs.md)** —— 安装、配置、迁移、GitHub 限制
- **[Workflows & Scenarios](references/workflows-scenarios.md)** —— 新 repo、已有 repo、外部 repo、团队入职
- **[Real-World Examples](references/real-world-examples.md)** —— 实际示例和案例

**排查与支持：**

- **[Troubleshooting](references/troubleshooting.md)** —— 常见问题与详细解决方案
- **[Commands Reference](references/commands-reference.md)** —— 带示例的完整命令列表
- **[Best Practices](references/best-practices.md)** —— 详细最佳实践与预防策略

**测试：**

- **[Evaluations](references/evaluations.md)** —— 测试场景、多模型测试、正式评估

---

## 测试与评估

关于完整测试场景、多模型测试说明和正式评估，参见 [Evaluations](references/evaluations.md)。

**评估汇总**：4/4 评估通过（100%）—— 在 Claude Sonnet 4 和 Claude Opus 4.5 上测试

---

## 版本历史

- v2.2.0（2025-11-28）：token 优化 —— 把决策树和评估抽取到 references/ 以便 progressive disclosure，SKILL.md 从 534 行精简至约 400 行
- v2.1.2（2025-11-25）：通过 MCP server 做内容验证 —— 所有技术内容已对照 Git 官方文档验证
- v2.1.1（2025-11-25）：审计改进 —— 修正模型命名规范，新增 Opus 4.5 验证
- v2.1.0（2025-11-17）：质量改进 —— 新增正式评估、多模型测试说明
- v2.0.1（2025-11-17）：内容审计 —— 更新 GitHub LFS 限制，对照 Git 2.51.2 验证
- v2.0.0（2024-11-09）：Hub/spoke 重构 —— 把详细内容抽取到 references/
- v1.0.0（2024-11-09）：首次发布，提供完整的 line ending 指引

---

## 官方文档

- [Git Attributes](https://git-scm.com/docs/gitattributes)
- [Git Config - core.autocrlf](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coreautocrlffalse)
- [Git Config - core.eol](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coreeol)
- [Configuring Git to handle line endings (GitHub)](https://docs.github.com/en/get-started/git-basics/configuring-git-to-handle-line-endings)
- [Git LFS Official Site](https://git-lfs.com/)
- [Git LFS Billing (GitHub)](https://docs.github.com/en/billing/concepts/product-billing/git-lfs)

## 最后更新

**日期：** 2025-11-28
**模型：** claude-opus-4-5-20251101
