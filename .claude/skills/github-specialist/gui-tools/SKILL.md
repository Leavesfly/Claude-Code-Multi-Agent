---
name: gui-tools
description: 为各平台 Git 图形界面客户端（GitKraken、Sourcetree、GitHub Desktop）的安装、配置与选型提供指导。对比特性、授权和 workflow。排查 GUI 工具配置与安装问题。当你在安装 Git 图形客户端、配置 Git 可视化工具、配置图形化 commit 工具、在多个选项之间选型或排查配置问题时使用。覆盖 Windows、macOS 与 Linux。所有工具都是可选的，取决于用户偏好。
allowed-tools: Read, Bash, Glob, Grep
---

# Git GUI Tools

Git 的可选图形界面工具。它们为 Git 操作提供可视化界面，让暂存修改、review diff、管理 branch、可视化仓库历史更方便。

## 概述

本 skill 帮你：

- 为你的 workflow 选到合适的 Git GUI 工具
- 在 Windows、macOS 与 Linux 上安装主流 Git GUI 客户端
- 正确配置 GUI 工具
- 理解授权与订阅要求
- 排查常见的配置问题

**重要**：所有 GUI 工具都是**可选**的。它们是命令行界面的补充，并非 Git 使用所必需。许多开发者只用命令行 Git。

## 何时使用本 skill

在以下场景使用本 skill：

- 首次安装 Git GUI 客户端
- 在 GitKraken、Sourcetree、GitHub Desktop 或其他工具之间选型
- 安装后配置 GUI 工具
- 排查 GUI 工具配置（Sourcetree 的 gitignore 等）
- 理解商业 GUI 工具的授权要求
- 寻找特定平台的 GUI 推荐

## 首次使用：检测与安装

在安装新 GUI 工具前，先检查系统上是否已经装了哪些。

**检查已有的 GUI 工具：**

```bash
# Check if GitKraken is installed
which gitkraken           # macOS/Linux
where.exe gitkraken       # Windows PowerShell

# Check if Sourcetree is installed
# Windows: Check "C:\Users\<user>\AppData\Local\SourceTree"
# macOS: Check "/Applications/Sourcetree.app"
test -d "/Applications/Sourcetree.app" && echo "Sourcetree installed" || echo "Sourcetree not found"

# Check if GitHub Desktop is installed
which github-desktop      # macOS/Linux (as 'github')
where.exe GitHubDesktop   # Windows PowerShell

# Check built-in Git GUI tools (come with Git)
git gui --version         # Git GUI (built-in with Git)
gitk --version            # gitk (built-in repository browser)
```

**如果还没装任何 GUI 工具，继续看下方"快速开始"。**

## 快速开始

**Windows - 快速安装：**

```powershell
# GitKraken (cross-platform, modern UI, requires license for private repos)
winget install --id Axosoft.GitKraken -e --source winget

# Sourcetree (free, Atlassian, Windows/macOS only)
winget install --id Atlassian.Sourcetree -e --source winget

# GitHub Desktop (free, simple, GitHub-focused)
winget install --id GitHub.GitHubDesktop -e --source winget

# Fork (free evaluation, fast and modern)
winget install --id Fork.Fork -e --source winget
```

**macOS - 快速安装：**

```bash
# GitKraken (via Homebrew Cask)
brew install --cask gitkraken

# Sourcetree (via Homebrew Cask)
brew install --cask sourcetree

# GitHub Desktop (via Homebrew Cask)
brew install --cask github

# Fork (via Homebrew Cask)
brew install --cask fork
```

**Linux - 快速安装：**

```bash
# GitKraken (download .deb or .rpm from website)
# https://www.gitkraken.com/download

# GitHub Desktop (community fork)
# https://github.com/shiftkey/desktop

# GitAhead (open source, lightweight)
# https://github.com/gitahead/gitahead
```

---

## 工具对比速查

| 工具 | 平台 | 授权 | 适合场景 |
| --- | --- | --- | --- |
| **GitKraken** | Windows、macOS、Linux | 免费（public repo）、Pro（private） | 团队、偏好视觉的用户、复杂 workflow |
| **Sourcetree** | Windows、macOS | 免费 | Atlassian 用户、功能丰富的 GUI |
| **GitHub Desktop** | Windows、macOS、Linux（社区 fork） | 免费 | GitHub 用户、追求简洁 |
| **Fork** | Windows、macOS | 免费试用 | Sourcetree 的现代替代 |
| **Git GUI/gitk** | 所有平台（随 Git 自带） | 免费（GPL） | 轻量、免安装 |
| **GitAhead** | Windows、macOS、Linux | 免费（MIT） | 轻量、开源 |

**📖 详细对比：** 参见 [references/tool-comparison.md](references/tool-comparison.md)，包含完整特性分析、授权细节和官方文档链接。

---

## 各平台安装

各平台详细安装说明见：

- **Windows**：[references/windows-installation.md](references/windows-installation.md)
- **macOS**：[references/macos-installation.md](references/macos-installation.md)
- **Linux**：[references/linux-installation.md](references/linux-installation.md)

---

## 内置工具（所有平台）

Git 自带两个 GUI 工具：

- **gitk** —— 用于可视化历史的仓库浏览器
- **Git GUI** —— 用于暂存修改的 commit 工具

```bash
# Open gitk (history browser)
gitk --all

# Open Git GUI (commit tool)
git gui
```

**📖 详细用法：** 参见 [references/built-in-tools.md](references/built-in-tools.md)，包含命令、特性和使用场景。

---

## 选对的工具

**快速推荐：**

- **新手** → GitHub Desktop
- **GitHub 用户** → GitHub Desktop 或 GitKraken
- **Bitbucket 用户** → Sourcetree
- **高级用户** → Fork、Tower 或 SmartGit
- **Linux 用户** → GitAhead、SmartGit 或内置工具
- **偏好视觉的用户** → GitKraken
- **命令行纯粹主义者** → 不需要（或用 gitk 做快速可视化）

**📖 详细指导：** 参见 [references/choosing-right-tool.md](references/choosing-right-tool.md)，包含完整决策标准。

---

## 安装后的配置

安装完 GUI 工具后：

1. **配置 Git identity**（如未配置）
2. **添加 SSH keys**（用于 SSH clone）
3. **在 Git 托管方完成认证**（GitHub、GitLab、Bitbucket）
4. **配置 diff/merge 工具**（可选）

**📖 详细步骤：** 参见 [references/configuration.md](references/configuration.md)，包含完整的安装后配置。

---

## 故障排查

**常见问题：**

- **GUI 工具识别不到仓库** —— 在设置里添加仓库目录
- **认证失败** —— 检查 credential helper，刷新 auth token
- **merge 冲突无法解决** —— 配置 merge 工具或改用命令行
- **性能慢** —— 开启 Git 性能特性，考虑使用 Git LFS

**📖 详细解决方案：** 参见 [references/troubleshooting.md](references/troubleshooting.md)，包含完整的故障排查指南。

---

## 相关 skill

- **setup**：基础 Git 安装与配置
- **config**：完整的 Git 配置（credentials、性能、alias）
- **line-endings**：行尾配置与 Git LFS
- **gpg-signing**：GPG commit 签名设置

---

## 测试场景

### 场景 1：工具安装

- Query："Install GitKraken on Windows"
- 期望：给出 winget 命令及授权信息

### 场景 2：工具选型指导

- Query："Choose between Sourcetree and GitHub Desktop for Bitbucket"
- 期望：基于对比表推荐 Sourcetree 并给出理由

### 场景 3：故障排查

- Query："Troubleshoot Sourcetree global gitignore configuration"
- 期望：给出故障排查章节中的配置修复步骤

### 场景 4：特定平台安装

- Query："Set up Fork on macOS"
- 期望：给出安装命令与授权说明

### 场景 5：对比请求

- Query："Compare all Linux GUI options"
- 期望：列出 GitAhead、SmartGit 和社区版 GitHub Desktop，并给出各自优缺点

---

## 多模型测试说明

**测试状态**：

| 模型 | 状态 | 测试日期 | 备注 |
| --- | --- | --- | --- |
| **Claude Sonnet** | ✅ VERIFIED | 2025-11-25 | skill 能正确激活，给出合适的工具推荐与各平台指导 |
| **Claude Haiku** | 🔄 PENDING | — | 建议测试：激活可靠性、推荐的精简度 |
| **Claude Opus** | 🔄 PENDING | — | 建议测试：过度解释的风险、跨平台复杂度 |

**最后更新**：2025-11-28

---

## 参考

**详细指南：**

- [references/tool-comparison.md](references/tool-comparison.md) —— 完整工具对比矩阵
- [references/built-in-tools.md](references/built-in-tools.md) —— Git GUI 与 gitk 用法
- [references/choosing-right-tool.md](references/choosing-right-tool.md) —— 按使用场景的决策指引
- [references/configuration.md](references/configuration.md) —— 安装后配置
- [references/windows-installation.md](references/windows-installation.md) —— Windows 安装指南
- [references/macos-installation.md](references/macos-installation.md) —— macOS 安装指南
- [references/linux-installation.md](references/linux-installation.md) —— Linux 安装指南
- [references/troubleshooting.md](references/troubleshooting.md) —— 常见问题排查

## 版本历史

- **v1.0.0**（2025-12-26）：首次发布

---

> **💡 给 AI Agent 的提示：** 帮助用户选择或安装 Git GUI 工具时，可以用可用的 MCP server 获取实时信息：
>
> - **价格更新**：用 `perplexity` MCP 查询 "GitKraken 2025 pricing plans"，或用 `firecrawl` 直接抓取价格页
> - **授权变更**：用 `ref` MCP 查阅官方文档的授权更新
> - **平台可用性**：用 `web_search` 验证某个工具在特定平台或地区的可用性
