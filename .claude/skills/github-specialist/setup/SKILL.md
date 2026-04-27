---
name: setup
description: 跨平台（Windows、macOS、Linux、WSL）安装 Git 并完成基础配置的完整指南。在首次设置 Git、在新系统上安装 Git、配置用户身份、设置默认 branch、选择 editor、验证安装或排查 Git 安装问题时使用。覆盖各平台专属安装方式、基础必需配置和验证步骤。
allowed-tools: Read, Bash, Glob, Grep
---

# Git Setup

在 Windows、macOS、Linux 和 WSL 环境中安装 Git 并完成关键初始配置的完整指引。

## 目录

- [快速开始](#quick-start) —— Windows、macOS、Linux
- [平台检测](#platform-detection) —— 识别你的平台
- [安装指南](#windows-installation) —— Windows、macOS、Linux、WSL
- [基础配置](#basic-configuration-all-platforms) —— 用户身份、默认 branch、editor
- [验证](#verification) —— 测试 Git 安装
- [配置文件](#configuration-file-locations) —— Git 配置存放位置
- [Reference 加载](#reference-loading-guide) —— reference 如何被加载
- [后续步骤](#next-steps) —— 借助其他 skill 做进阶配置

## 概述

本 skill 帮你：

- 按平台选最合适的方式安装 Git
- 配置关键 Git 设置（用户身份、默认 branch、editor）
- 验证安装是否正常工作
- 理解平台相关的注意事项
- 用推荐默认值快速起步

**进阶配置**（alias、性能调优、凭据管理、维护）请参见 **git-config** skill。

## 何时使用本 skill

在以下场景使用：

- 在新的开发机上安装 Git
- 任意平台上的首次 Git 设置
- 配置用户身份和基础 Git 设置
- 验证 Git 安装是否正常
- 排查 Git 安装问题
- 理解各平台的 Git 安装选项
- 在 Windows、macOS、Linux 或 WSL 上配置 Git

## 快速开始

**最快完成 Git 安装和配置的路径：**

### Windows

```powershell
# Install Git
winget install --id Git.Git -e --source winget

# Configure identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# Verify
git --version
```

### macOS

```bash
# Install Git
brew install git

# Configure identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# Verify
git --version
```

### Linux

```bash
# Install Git (Ubuntu/Debian)
sudo apt update && sudo apt install git

# Configure identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# Verify
git --version
```

---

## 平台检测

帮用户安装 Git 时，用这些环境指标检测平台：

- **Windows**：`$env:OS` 包含 "Windows"、使用 PowerShell 命令、`winget`、形如 `C:\` 的路径
- **macOS**：`uname -s` 返回 "Darwin"、Homebrew（`brew`）、`~/.zshrc`
- **Linux**：`uname -s` 返回 "Linux"、包管理器（`apt`、`dnf`、`pacman`）、`~/.bashrc`
- **WSL**：Linux 内核 + Windows 集成（如 `/mnt/c/`）、可用 `wsl.exe`

基于检测到或用户指明的平台，自动提供对应指引。

---

## Windows 安装

两种安装方式：**Git for Windows Installer**（可完全掌控，推荐）或 **winget**（快速安装）。包含 Windows 特定配置（long path、系统级设置）与故障排查。

📚 **完整指南：** [references/install-windows.md](references/install-windows.md)

涵盖主题：安装选项、验证、Windows 特定配置、Win32 长路径、故障排查、Git Bash 历史问题

---

## macOS 安装

两种安装方式：**Xcode Command Line Tools**（快速、自带）或 **Homebrew**（最新版本，推荐）。包含 macOS 特定配置、shell 集成（Zsh/Bash）和全局 gitignore 设置。

📚 **完整指南：** [references/install-macos.md](references/install-macos.md)

涵盖主题：安装选项、验证、line ending 配置、Zsh/Bash shell 集成、全局 gitignore

---

## Linux 安装

通过包管理器安装：**apt**（Ubuntu/Debian）、**dnf**（Fedora/RHEL）或 **pacman**（Arch Linux）。包含 Linux 特定配置、shell 集成（Bash/Zsh）和全局 gitignore 设置。

📚 **完整指南：** [references/install-linux.md](references/install-linux.md)

涵盖主题：按发行版的安装、验证、line ending 配置、Bash/Zsh shell 集成、全局 gitignore

---

## WSL（Windows Subsystem for Linux）

WSL 运行的是 Linux（默认 Ubuntu），所以 Git 安装与原生 Linux 一致。包含 WSL 特定配置与性能注意事项。

📚 **完整指南：** [references/install-wsl.md](references/install-wsl.md)

涵盖主题：安装（同 Linux）、验证、WSL 特定配置、文件系统性能说明、Windows 集成、凭据共享

---

## 基础配置（所有平台）

安装 Git 后，配置以下关键设置：

### 设置用户身份（必需）

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**重要**：这些信息会被包含在每次 commit 里。使用你的真名和工作/个人邮箱。

### 设置默认 branch 名

```bash
git config --global init.defaultBranch main
```

**说明**：现代标准是 `main`（取代旧的 `master` 约定）。

### 设置首选 editor（可选）

任选其一：

```bash
# VS Code (must be in PATH)
git config --global core.editor "code --wait"

# Notepad (Windows)
git config --global core.editor "notepad"

# Vim
git config --global core.editor "vim"

# Nano (Linux/macOS)
git config --global core.editor "nano"
```

**说明**：若未设置，Git 使用系统默认 editor（Linux/macOS 通常是 Vim，Windows 是 Notepad）。

---

## 验证

安装并配置完成后，验证一切正常工作：

### 检查 Git 版本

```bash
git --version
```

### 验证配置

```bash
# View all configuration with source files
git config --list --show-origin

# View specific values
git config user.name
git config user.email
git config init.defaultBranch
```

### 测试基础 Git 操作

```bash
# Create test repository
mkdir test-repo
cd test-repo
git init

# Create and commit a file
echo "# Test Repository" > README.md
git add README.md
git commit -m "Initial commit"

# View commit history
git log

# Clean up
cd ..
rm -rf test-repo
```

**期望结果**：repo 创建成功、commit 成功、log 里能看到这次 commit。

---

## 配置文件位置

关于配置文件位置、优先级、conditional includes 和排查问题的完整细节，参见 [Configuration Details](references/configuration-details.md)。

**快速参考：**

- **System**：`/etc/gitconfig`（所有用户，需要 sudo）
- **Global**：`~/.config/git/config` 或 `~/.gitconfig`（当前用户）
- **Local**：`.git/config`（当前 repo）
- **优先级**：Local > Global > System

---

## 取消配置项

移除/取消一个配置项：

```bash
# Unset system config value (requires admin/sudo)
git config --system --unset status.aheadbehind

# Unset global/user config value
git config --global --unset status.aheadbehind

# Unset local config value (inside a repo)
git config --unset status.aheadbehind
```

---

## Reference 加载指引

本 skill 中的所有 reference 都基于平台检测或故障排查上下文**按需加载**。这种 progressive disclosure 策略让 skill 仅在需要时才加载相关内容，以保持高效。

### 始终加载（核心）

- 无 —— 所有 reference 均为情境相关，按需加载

### 条件加载

- `references/install-windows.md` —— 当用户在 Windows 平台时加载（安装流程）
- `references/install-macos.md` —— 当用户在 macOS 平台时加载（安装流程）
- `references/install-linux.md` —— 当用户在 Linux 平台时加载（安装流程）
- `references/install-wsl.md` —— 当用户在 WSL 平台时加载（安装流程）
- `references/git-bash-history-troubleshooting.md` —— 当排查 Windows Terminal 下 Git Bash 命令历史问题时加载

**token 效率**：大多数用户走 Quick Start 路径（约 3k token）。平台专属深入内容仅加载对应 reference（整体约 3.5–4k token）。

---

## 后续步骤

完成基础 Git 设置后，可考虑：

1. **进阶配置**：使用 **git-config** skill 进行：
   - 完整的全局配置（性能、alias、维护）
   - 凭据管理（GitHub CLI、Windows Credential Manager）
   - Clone 快捷方式（为常用 repo 省输入）

2. **Line ending 管理**：使用 **line-endings** skill 进行：
   - 理解 line ending 配置
   - 为跨平台团队设置 `.gitattributes`
   - 排查 line ending 问题
   - 为大文件设置 Git LFS

3. **Commit 签名**：使用 **gpg-signing** skill 进行：
   - 配置 GPG commit 签名
   - 生成并管理 GPG key
   - 把 GPG key 添加到 GitHub/GitLab
   - 排查签名问题

4. **GUI 工具**：使用 **gui-tools** skill 进行：
   - 安装 Git GUI 客户端（GitKraken、Sourcetree、GitHub Desktop）
   - 配置 GUI 工具
   - 根据你的 workflow 选择合适的 GUI

---

## 相关 skill

- **config**：完整的 Git 配置（alias、性能、凭据、维护）
- **line-endings**：line ending 配置、`.gitattributes`、Git LFS
- **gpg-signing**：GPG commit 签名的设置和排查
- **gui-tools**：Git GUI 客户端的安装和配置

---

## 测试与评估

关于完整的测试场景、多模型测试说明、正式评估和开发方法论，参见 [Testing and Evaluations](references/testing-evaluations.md)。

**评估汇总**：3/3 评估通过 —— 在 Claude Sonnet 4.5 和 Claude Sonnet 3.5 上测试

---

## 参考

**安装指南：**

- [Windows Installation](references/install-windows.md) —— winget、Git for Windows installer、Win32 长路径
- [macOS Installation](references/install-macos.md) —— Xcode Command Line Tools、Homebrew、Zsh 集成
- [Linux Installation](references/install-linux.md) —— apt、dnf、pacman、Bash/Zsh 集成
- [WSL Installation](references/install-wsl.md) —— 与 Linux 相同、WSL2 性能、凭据共享

**配置：**

- [Configuration Details](references/configuration-details.md) —— 文件位置、优先级、conditional includes、排查

**故障排查：**

- [Git Bash History Troubleshooting](references/git-bash-history-troubleshooting.md) —— Windows Terminal 命令历史问题

**测试：**

- [Testing and Evaluations](references/testing-evaluations.md) —— 测试场景、多模型测试、正式评估

---

## 版本历史

- v1.3.0（2025-11-28）：token 优化 —— 把配置细节和测试抽取到 references/，SKILL.md 从 526 行精简至约 400 行
- v1.2.0（2025-11-25）：完整审计改进 —— 增强 WSL reference，Git 版本更新至 2.47.0+
- v1.1.0（2025-01-11）：新增 Windows Terminal Git Bash 命令历史排查 reference
- v1.0.0（2025-01-06）：首次发布，提供完整的 Git 安装和配置指引

---

## 官方文档

- [Git Official Site](https://git-scm.com/)
- [Git for Windows](https://git-scm.com/install/windows)
- [Getting Started - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
- [Git Configuration Documentation](https://git-scm.com/docs/git-config)

---

## 最后更新

**日期：** 2025-11-28
**模型：** claude-opus-4-5-20251101
