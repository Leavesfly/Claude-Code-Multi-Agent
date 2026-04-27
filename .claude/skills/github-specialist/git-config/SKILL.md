---
name: git-config
description: 全面的 Git 配置指南，涵盖全局设置、别名、性能调优、凭据管理、维护、.gitattributes、克隆快捷方式和故障排除。当需要在基本设置之外配置 Git、优化 Git 性能、设置别名、管理凭据（GitHub CLI、Windows Credential Manager）、配置行尾策略、设置 .gitattributes、启用 Git 维护或排查配置问题时使用。提供适用于 Windows、macOS 和 Linux 的跨平台指导。
allowed-tools: Read, Bash, Glob, Grep
---

# Git 配置

在基本安装之外配置 Git 的全面指南。本 skill 涵盖全局配置、性能优化、别名、凭据管理、维护和高级配置主题。

## 目录

- [概述](#概述)
- [何时使用](#何时使用此-skill)
- [快速开始](#快速开始)
- [配置文件](#配置文件位置)
- [全局设置](#全局配置设置)
- [Rerere](#管理-rerere-重用已记录的解决方案)
- [行尾](#行尾策略纵深防御)
- [别名](#别名)
- [克隆快捷方式](#克隆快捷方式)
- [仓库级配置](#仓库级配置)
- [维护](#维护)
- [Git 属性](#git-属性)
- [凭据](#git-凭据管理)
- [故障排除](#故障排除)
- [测试场景](#测试场景)

## 概述

本 skill 帮助你：

- 全局配置 Git 以获得最佳性能和工作流
- 设置强大的别名以简化常见操作
- 安全管理 Git 凭据（GitHub CLI、Windows Credential Manager）
- 配置 `.gitattributes` 以控制行尾
- 启用 Git 维护以提高仓库性能
- 理解仓库级与全局配置的区别
- 排查常见配置问题

**有关基本的 Git 安装和设置**，请参阅 **setup** skill。

## 示例用例

以下类型的问题会触发此 skill：

- "如何为常见操作设置 Git 别名？"
- "配置 Git 使用 GitHub CLI 进行身份验证"
- "我的 Git 在大仓库中很慢 - 如何优化性能？"
- "为跨平台开发设置 .gitattributes"
- "启用 Git 后台维护"
- "排查 'refusing to allow OAuth App to create workflow' 错误"
- "在 Windows 上配置 Git 凭据助手"
- "为常用仓库设置克隆快捷方式"
- "配置 rerere 以记住合并冲突解决方案"

## 何时使用此 Skill

在以下情况下使用此 skill：

- 在基本用户身份之外配置 Git
- 为常见 Git 操作设置别名
- 优化 Git 性能（fsmonitor、untrackedCache、并行操作）
- 管理 Git 凭据（HTTPS、SSH、令牌范围）
- 设置 GitHub CLI 作为凭据助手
- 配置 `.gitattributes` 以实现跨平台行尾控制
- 启用 Git 后台维护
- 为常用仓库设置克隆快捷方式
- 排查凭据问题或行尾错误
- 理解 Git 配置层次结构（系统/全局/本地）

## 快速开始

**立即应用的最具影响力的配置设置：**

```bash
# Performance improvements
git config --global core.fsmonitor true
git config --global core.untrackedCache true
git config --global fetch.parallel 8
git config --global checkout.workers 8

# Better pull/rebase workflow
git config --global pull.rebase true
git config --global rebase.autoStash true
git config --global rebase.updateRefs true

# Auto-prune deleted remote branches
git config --global fetch.prune true
git config --global fetch.pruneTags true

# Better merge conflict resolution
git config --global merge.conflictstyle zdiff3

# Auto-setup remote tracking on first push
git config --global push.autoSetupRemote true

# Better diff algorithm
git config --global diff.algorithm histogram
git config --global diff.colorMoved zebra

# Line ending safety check
git config --global core.safecrlf warn
```

---

## 快速参考

按类别组织的常见 Git 配置设置，便于快速查找（性能、pull/rebase、fetch、push、diff、merge、行尾、排序、维护）。

**📚 完整快速参考表：** [references/quick-reference.md](references/quick-reference.md)

快速参考表提供了所有常见设置的复制粘贴命令，包含类别组织和用途说明。

---

## 配置文件位置

有关 Git 配置文件位置、层次结构和查看配置的详细信息，请参阅 [references/configuration-basics.md](references/configuration-basics.md)。

**快速摘要：**

- **系统**：`/etc/gitconfig`（或 Windows 上的 Git 安装目录）- 所有用户
- **全局**：`~/.gitconfig` 或 `~/.config/git/config` - 当前用户
- **本地**：`.git/config` - 仅单个仓库
- **优先级**：本地 > 全局 > 系统

**查看配置：**

```bash
# View all configuration with source files
git config --list --show-origin

# View specific value
git config user.name
```

---

## 全局配置设置

这些设置是跨平台的，推荐所有用户使用。除非需要仓库特定的覆盖，否则应在**全局**级别设置。

**最必要的设置（快速开始）：**

```bash
# Pull & rebase (cleaner history)
git config --global pull.rebase true
git config --global rebase.autoStash true

# Fetch (auto-prune deleted branches/tags)
git config --global fetch.prune true
git config --global fetch.pruneTags true
git config --global fetch.parallel 8

# Push (auto-setup remote tracking)
git config --global push.autoSetupRemote true

# Diff & merge (better algorithms and conflict display)
git config --global diff.algorithm histogram
git config --global diff.colorMoved zebra
git config --global merge.conflictstyle zdiff3

# Performance (filesystem monitoring)
git config --global core.fsmonitor true
git config --global core.untrackedCache true

# Sorting (newest first)
git config --global branch.sort -committerdate
git config --global tag.sort -taggerdate
```

**📚 完整全局配置指南：** [references/global-configuration.md](references/global-configuration.md)

涵盖主题：Pull 和 rebase 策略、fetch 策略、push 策略、checkout/switch 策略、commit 设置、status 设置、diff 设置、merge 设置、rerere（冲突解决方案记忆）、颜色设置、排序、log 设置、性能设置、submodule 策略、杂项设置和高级配置选项。

---

## 管理 rerere（重用已记录的解决方案）

Git 的 rerere 功能会记住你如何解决合并冲突，并可以自动重新应用这些解决方案。

**启用 rerere：**

```bash
git config --global rerere.enabled true
git config --global rerere.autoUpdate true
```

**有关 rerere 工作原理、安全注意事项和管理命令的详细信息**，请参阅 [references/configuration-basics.md](references/configuration-basics.md#managing-rerere-reuse-recorded-resolution)

## 行尾策略（纵深防御）

**跨平台行尾管理采用分层方法：**

1. **系统级（autocrlf）**：平台特定默认值（Windows：`true`，macOS/Linux：`input`）
2. **全局级（safecrlf）**：警告 MIXED 行尾（实际问题）但允许合法转换
3. **仓库级（.gitattributes）**：每种文件类型的显式控制

**纵深防御：** 这些设置是**互补的，而非冲突的**：

- `autocrlf` 自动处理转换
- `safecrlf=warn` 警告问题（混合行尾）但不会阻止 .gitattributes 转换
- `.gitattributes` 提供每个仓库的细粒度控制

**重要提示：** 当你有 `.gitattributes` 时不要使用 `safecrlf=true` - 它会阻止合法转换并导致持续错误！

**平台特定的 autocrlf 设置：**

- **Windows**：`git config --global core.autocrlf true`（checkout 时将 LF→CRLF，commit 时将 CRLF→LF）
- **macOS/Linux**：`git config --global core.autocrlf input`（commit 时将 CRLF→LF，checkout 时不转换）
- **WSL**：`git config --global core.autocrlf input`（与 Linux 相同）

**全局 safecrlf 设置（所有平台）：**

```bash
# Warn about mixed line endings (safeguard against problems)
# Use 'warn' not 'true' - allows .gitattributes to work without constant errors
git config --global core.safecrlf warn
```

**有关详细的 .gitattributes 设置和全面的行尾指导**，请参阅 **line-endings** skill。

---

## 别名

别名为常见 Git 操作提供快捷方式。类别包括：信息和检查、导航、分支操作、暂存/取消暂存、提交、历史编辑、远程操作和标签推送工作流。

**最常见的别名：**

```bash
# Status and branch info
git config --global alias.st "status -sb"
git config --global alias.br "branch -vv"

# Modern branch switching
git config --global alias.co "switch"
git config --global alias.cob "switch -c"

# Quick amend
git config --global alias.amend "commit --amend --no-edit"

# Safe force push
git config --global alias.pfwl "push --force-with-lease"
```

**📚 完整别名指南：** [references/aliases.md](references/aliases.md)

涵盖主题：信息和检查别名、导航实用程序、分支操作、暂存/取消暂存快捷方式、提交助手、历史编辑（危险！）、远程操作、标签推送工作流、带有 `url.insteadOf` 的克隆快捷方式、组织/个人简写模式和自定义模板。

---

## 克隆快捷方式

Git 的 `url.insteadOf` 功能为克隆 URL 创建简写前缀。例如：`git clone gh:username/repo` 而不是 `git clone git@github.com:username/repo.git`。

**快速示例：**

```bash
# Universal GitHub shorthand
git config --global url."git@github.com:".insteadOf "gh:"
# Usage: git clone gh:microsoft/vscode
```

**📚 完整克隆快捷方式指南：** [references/aliases.md#clone-shortcuts](references/aliases.md#clone-shortcuts)

涵盖主题：通用 GitHub 简写、组织/个人快捷方式、企业 Git 服务器、自定义命名空间模板和使用示例。

---

## 仓库级配置

**这些设置是特定于上下文的** - 在需要时在单个仓库中使用它们，而不是作为全局默认值。

**何时使用仓库级配置：**

- 仓库特定要求（submodules、性能、大小）
- 为特定工作流覆盖全局设置
- 该仓库的团队特定约定

**示例：**

```bash
# Enable submodule recursion in a specific repo
git config submodule.recurse true

# Disable ahead/behind in a massive repo (performance)
git config status.aheadBehind false

# Override line ending strategy (with .gitattributes)
git config core.autocrlf false
```

---

## 维护

Git 可以执行后台维护以保持仓库快速运行（commit-graph 更新、增量重新打包、松散对象清理）。

**快速设置：**

```bash
# Enable background maintenance once for your account
git maintenance start

# Register each active repo for maintenance
git maintenance register

# Enable commit-graph generation globally
git config --global gc.writeCommitGraph true
```

**常用命令：**

```bash
# Run maintenance now
git maintenance run

# See which repos are registered
git config --global --get-all maintenance.repo

# Unregister a repo
git maintenance unregister
```

**📚 完整维护指南：** [references/global-configuration.md#maintenance](references/global-configuration.md#maintenance)

涵盖主题：后台维护任务、调度（每小时/每天/每周）、任务配置、禁用完整 gc、maintenance.auto 设置、每项任务启用、排查维护问题。

---

## Git 属性

`.gitattributes` 提供对每种文件类型行尾的显式控制。这是跨平台团队的推荐方法。

**最佳实践：** 在 `.gitattributes` 中控制行尾，并使用适合平台的 `autocrlf` 设置和 `safecrlf=warn`。

**快速示例：**

```gitattributes
# Auto-detect text files and normalize line endings
* text=auto

# Force LF for scripts/config (shell and CI tools expect LF)
*.sh      text eol=lf
*.yml     text eol=lf
*.json    text eol=lf

# Force CRLF for Windows-specific files
*.ps1     text eol=crlf
*.cmd     text eol=crlf
*.bat     text eol=crlf

# Mark binaries
*.png binary
*.jpg binary
*.pdf binary
```

**规范化具有混合行尾的现有仓库：**

```bash
git add --renormalize .
git commit -m "Normalize line endings"
```

**📚 有关全面的 .gitattributes 设置、Git LFS 模式和行尾策略**，请参阅 **line-endings** skill。

---

## Git 凭据管理

Git 使用凭据助手来存储和检索远程操作（clone、fetch、push）的身份验证凭据。了解凭据管理对于安全、高效的 Git 工作流至关重要。

**有关全面的凭据管理指导**，请参阅 [references/credential-management.md](references/credential-management.md)。

**快速概述：**

- **Windows**：默认凭据助手是 Windows Credential Manager（`manager`）
- **macOS**：默认是 macOS Keychain（`osxkeychain`）
- **Linux**：默认是内存缓存（`cache`）或 `libsecret` 用于持久存储
- **GitHub 用户**：推荐混合设置（gh CLI 用于 GitHub，操作系统凭据管理器用于其他）

**GitHub 的快速设置（混合方法）：**

```bash
# 1. Authenticate with gh CLI
gh auth login --scopes "repo,read:org,workflow" --git-protocol https

# 2. Configure gh CLI as credential helper for GitHub only
gh auth setup-git

# 3. Verify hybrid setup
git config --list | grep credential
# Should show:
# - credential.helper=manager (or osxkeychain/cache - OS default)
# - credential.https://github.com.helper=!gh auth git-credential (GitHub-specific)
```

**常见凭据问题：**

- "refusing to allow OAuth App to create or update workflow" → 需要 `workflow` 范围
- Git 仍使用旧/错误的凭据 → 清除操作系统凭据缓存
- SSH 与 HTTPS 混淆 → 使用 `git remote -v` 检查远程 URL

有关详细的故障排除、凭据助手选项、安全最佳实践和平台特定指导，请参阅 [references/credential-management.md](references/credential-management.md)。

---

## 故障排除

有关全面的故障排除指导，请参阅 [references/troubleshooting.md](references/troubleshooting.md)。

**快速链接：**

- [行尾错误](references/troubleshooting.md#line-ending-errors) - "LF would be replaced by CRLF" 警告
- [凭据问题](references/troubleshooting.md#credential-issues) - OAuth 错误、过时凭据、SSH 与 HTTPS
- [性能问题](references/troubleshooting.md#performance-problems) - 缓慢的 git status、fetch 超时
- [常见现实场景](references/troubleshooting.md#common-real-world-scenarios) - 企业代理、WSL 问题、rerere 问题

---

## 相关 Skills

- **setup**：基本的 Git 安装和初始配置
- **line-endings**：全面的行尾配置、`.gitattributes`、Git LFS
- **gpg-signing**：GPG commit 签名设置和故障排除
- **gui-tools**：Git GUI 客户端安装和配置

---

## 测试场景

有关验证 skill 激活和响应质量的测试场景，请参阅 [references/test-scenarios.md](references/test-scenarios.md)。

**覆盖范围：** 9 个场景，涵盖基本用法（别名、性能、维护）、高级用法（凭据、行尾、仓库配置）、故障排除（凭据问题、rerere）和快捷方式（克隆快捷方式、url.insteadOf）。

---

## 版本历史

- v1.3.0 (2025-11-25)：审计合规性改进
  - 将测试场景提取到 references/test-scenarios.md
  - 将 SKILL.md 从 539 行减少到约 475 行（低于 500 行建议）
  - 改进了渐进式披露合规性
- v1.2.0 (2025-11-12)：基于审计建议的质量改进
  - 将配置文件位置提取到 references/configuration-basics.md
  - 将管理 rerere 提取到 references/configuration-basics.md
  - 将故障排除提取到 references/troubleshooting.md
  - 添加了多模型测试说明
  - 添加了常见的现实使用场景
  - 将 SKILL.md 行数从 959 行减少到约 700 行（提高性能）
  - 为所有参考文件添加了目录以改善导航
  - 创建了包含全面故障排除指导的 references/troubleshooting.md
- v1.1.0 (2025-11-12)：基于审计反馈的可用性增强
  - 添加了示例用例部分，包含具体的激活示例
  - 添加了快速参考表以快速查找常见设置
  - 提高了自主激活可靠性
- v1.0.0 (2025-11-09)：初始发布，包含全面的 Git 配置指导

---

## 官方文档

- [Git Configuration Documentation](https://git-scm.com/docs/git-config)
- [Git Attributes Documentation](https://git-scm.com/docs/gitattributes)
- [GitHub CLI Manual](https://cli.github.com/manual/)
- [Git Credential Helpers](https://git-scm.com/docs/gitcredentials)
- [Git Maintenance](https://git-scm.com/docs/git-maintenance)

---

## 最后更新

**日期：** 2025-11-28
**模型：** claude-opus-4-5-20251101
