---
name: gpg-signing
description: GPG commit 签名完整指南。配置、设置并排查 GPG commit 签名。修复 GPG 签名错误、配置 passphrase 缓存、验证 commit 签名。在处理 Git commit 签名、GPG key、commit 校验、签名校验、GPG 配置，或遇到 GPG 签名错误时使用。覆盖 Windows（Gpg4win）、macOS（GPG Suite）、Linux（gnupg）和 WSL 的安装与设置。
allowed-tools: Read, Bash, Glob, Grep
---

# Git GPG 签名

在各平台上设置、配置和排查 GPG commit 签名的完整指南。

## 目录

- [概述](#概述)
- [何时使用本技能](#何时使用本-skill)
- [快速开始](#快速开始)
- [各平台设置](#各平台设置)
- [密钥生成](#key-生成)
- [Git 配置](#git-配置)
- [口令缓存](#passphrase-缓存)
- [GitHub 集成](#github-集成)
- [故障排查](#故障排查)
- [GPG 签名方式对比](#gpg-签名方式对比)
- [安全最佳实践](#安全最佳实践)
- [资源](#资源)
- [相关技能](#相关-skill)

## 概述

Git commit 签名通过密码学证明来验证 commit 确实来自于你本人。本技能帮助你：

- 在 Windows、macOS、Linux 上安装和配置 GPG 工具
- 使用合理的密钥管理方式设置 commit 签名
- 配置口令缓存以获得更流畅的工作流程
- 排查常见 GPG 签名问题
- 理解安全权衡和最佳实践

## 何时使用本 skill

在以下场景使用本技能：

- 首次设置 GPG commit 签名
- 配置 GPG 工具（Gpg4win、GPG Suite、gnupg）
- 排查 GPG 签名错误（"gpg: signing failed"、"inappropriate ioctl" 等）
- 排查 keyboxd daemon 启动问题（Windows 竞态条件）
- 确定 Git 使用的是哪个 GPG 安装
- 理解 Windows daemon 启动行为与官方设计的对比
- 配置口令缓存以减少提示次数
- 理解 GPG、SSH 和 S/MIME 签名方式的差异
- 向 GitHub/GitLab/Bitbucket 添加 GPG 密钥
- 解决 GitHub 上 "Unverified" commit 问题
- 处理 `.gnupg/gpg-agent.conf` 配置

## 快速开始

**基础配置（单个个人密钥）：**

```bash
# 1. Install GPG (see Platform-Specific Setup below)

# 2. Generate key
gpg --full-generate-key
# Select: (9) ECC (sign and encrypt) → Curve 25519
# Expiration: 0 (no expiration)
# Passphrase: Strong 20+ character passphrase

# 3. Get key ID
gpg --list-secret-keys --keyid-format=long
# Look for "sec   ed25519/<KEY_ID>"

# 4. Configure Git
git config --global user.signingkey <KEY_ID>
git config --global commit.gpgsign true

# 5. Export public key and add to GitHub
gpg --armor --export <KEY_ID>
# Paste at: https://github.com/settings/keys
```

## 各平台设置

### Windows：安装 Gpg4win

**详细 Windows 设置**见 [references/windows-setup.md](references/windows-setup.md)。

**快速安装：**

```powershell
# 选项 1：下载安装程序
# https://gpg4win.org/thanks-for-download.html

# 选项 2：使用 winget
winget install --id GnuPG.Gpg4win -e --source winget

# 配置 Git 使用 Gpg4win（如果安装在其他位置，请调整路径）
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```

### macOS：GPG Suite / Homebrew

```bash
# 选项 1：Homebrew（推荐）
brew install gnupg

# 选项 2：GPG Suite
# 下载地址：https://gpgtools.org/

# 配置 Git（如需要）
git config --global gpg.program $(which gpg)
```

### Linux：GnuPG

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install gnupg

# Fedora/RHEL
sudo dnf install gnupg2

# Arch
sudo pacman -S gnupg
```

### WSL：参考 Linux 的说明

**WSL 用户：** 按上面 Linux 的设置步骤操作（WSL 运行的是 Linux）。如有 WSL 专属问题，请参见故障排查章节。

## 密钥生成

### 个人开发密钥（交互式）

```bash
# 交互式生成密钥
gpg --full-generate-key

# 按照提示操作：
# 1. 密钥类型：(9) ECC（签名和加密）*默认*
# 2. 曲线：(1) Curve 25519 *默认*
# 3. 过期时间：0 = 永不过期（或 2y 表示 2 年）
# 4. 真实姓名：您的姓名
# 5. 邮箱：your.verified@email.com
# 6. 注释：（可选，可以留空）
# 7. 口令：强口令，至少 20 个字符

# 列出密钥以获取 KEY_ID
gpg --list-secret-keys --keyid-format=long
# 查找 "sec   ed25519/<KEY_ID>"
```

**推荐算法：** 使用基于 Curve25519 的 EdDSA（Ed25519）—— 现代、快速、密钥体积小、安全性优秀

**替代方案：** RSA 4096 位（传统、兼容性广泛）

### 导出密钥

```bash
# 导出公钥（用于 GitHub）
gpg --armor --export <KEY_ID>

# 导出私钥（仅用于备份 - 请妥善保管！）
gpg --armor --export-secret-keys <KEY_ID>
```

## Git 配置

### 启用 commit 签名

```bash
# 全局配置（所有仓库）
git config --global user.signingkey <KEY_ID>
git config --global commit.gpgsign true
git config --global tag.gpgSign true

# 仓库级配置（特定仓库）
cd /path/to/repo
git config user.signingkey <KEY_ID>
git config commit.gpgsign true
```

### 验证配置

```bash
# 检查已配置的密钥
git config --global user.signingkey

# 测试签名
git commit --allow-empty -m "Test GPG signing"

# 验证签名
git log --show-signature -1
```

## 口令缓存

GPG agent 会缓存口令以减少输入提示次数。

**检查配置是否存在：**

```bash
ls ~/.gnupg/gpg-agent.conf
```

**创建或编辑** `~/.gnupg/gpg-agent.conf`：

```conf
# 空闲 8 小时后清除缓存的口令
default-cache-ttl 28800

# 无论是否使用，最长缓存 24 小时
max-cache-ttl 86400

# 允许 pinentry 缓存口令
allow-preset-passphrase
```

**应用变更：**

```bash
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

**完整缓存配置**，包括安全场景、测试流程和排查，见 [references/passphrase-caching.md](references/passphrase-caching.md)。

### Windows 专属：双 GPG 安装问题

**Windows 用户特别注意：** Windows 通常会有**两套独立的 GPG 安装**（Gpg4win 和 Git Bash GPG），二者的配置文件位置不同：

- **Gpg4win：** `%APPDATA%\gnupg\gpg-agent.conf`（通常是 `C:\Users\<Username>\AppData\Roaming\gnupg\gpg-agent.conf`）
- **Git Bash GPG：** `~/.gnupg/gpg-agent.conf`（解析为 `C:\Users\<Username>\.gnupg\gpg-agent.conf`）

**常见问题：** Git 配置使用 Gpg4win，却去配置了 `~/.gnupg/gpg-agent.conf`（Git Bash GPG）。结果：**配置被静默忽略，缓存不生效**。

**确认 Git 使用的是哪个 GPG：**

```bash
# 检查 Git 的 GPG 配置
git config --global gpg.program

# 如果输出是 "C:/Program Files (x86)/GnuPG/bin/gpg.exe"：
# → 使用：%APPDATA%\gnupg\gpg-agent.conf

# 验证 GPG 的配置目录
gpgconf --list-dirs homedir
```

**Windows 配置必须包含 pinentry-program：**

```conf
# 示例：%APPDATA%\gnupg\gpg-agent.conf
default-cache-ttl 28800
max-cache-ttl 86400
allow-preset-passphrase

# Windows 必需配置：
pinentry-program C:/Program Files (x86)/GnuPG/bin/pinentry-basic.exe
```

**详见：** [Windows Setup Guide - Dual GPG Installations](references/windows-setup.md)。

### 推荐缓存时长

| 场景 | default-cache-ttl | max-cache-ttl | 原因 |
| --- | --- | --- | --- |
| **高安全** | 900（15 分钟） | 3600（1 小时） | 频繁提示，时间窗口短 |
| **平衡** | 3600（1 小时） | 28800（8 小时） | 每小时提示一次，一天结束时过期 |
| **便利** | 28800（8 小时） | 86400（24 小时） | 工作日只提示一次，每天过期 |

## GitHub 集成

### 向 GitHub 添加 GPG 密钥

1. **导出公钥：**

   ```bash
   gpg --armor --export <KEY_ID>
   ```

2. **添加到 GitHub：**
   - 访问：[https://github.com/settings/keys](https://github.com/settings/keys)
   - 点击 "New GPG key"
   - 粘贴 ASCII 编码的公钥
   - 点击 "Add GPG key"

3. **验证邮箱匹配：**
   - Git 邮箱必须与 GPG 密钥的邮箱一致
   - 邮箱必须在 GitHub 账号中已验证

   ```bash
   # 检查 Git 邮箱
   git config --global user.email

   # 检查 GPG 密钥邮箱
   gpg --list-keys <KEY_ID>
   ```

### 排查 "Unverified" commit

**可能原因：**

1. **公钥未添加到 GitHub** → 在 Settings → SSH and GPG keys 中添加
2. **邮箱不匹配** → 确保 Git 邮箱 = GPG 密钥邮箱 = GitHub 已验证邮箱
3. **密钥已过期** → 延长过期时间或生成新密钥
4. **密钥已吊销** → 生成新密钥（吊销操作无法撤销）

## 故障排查

**完整排查指南**见 [references/troubleshooting.md](references/troubleshooting.md)。

### "gpg: signing failed: No secret key"

**原因：** Git 配置使用的密钥 ID 在密钥环中不存在。

**解决方法：**

```bash
# 列出可用密钥
gpg --list-secret-keys --keyid-format=long

# 验证 Git 配置与可用密钥匹配
git config --global user.signingkey

# 如果不匹配，更新配置
git config --global user.signingkey <CORRECT_KEY_ID>
```

### "gpg: signing failed: Inappropriate ioctl for device"

**原因：** GPG agent 无法提示输入口令（终端问题）。

**解决方法（Linux/macOS/WSL）：**

```bash
# 设置 GPG_TTY 环境变量
export GPG_TTY=$(tty)

# 添加到 shell 配置文件（例如 ~/.bashrc、~/.zshrc）
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
```

**解决方法（Windows 配合 Gpg4win）：**

```bash
# 使用图形界面 pinentry（调整路径以匹配您的 Gpg4win 安装）
echo 'pinentry-program "C:/Program Files (x86)/GnuPG/bin/pinentry-basic.exe"' >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```

### "error: gpg failed to sign the data"

**调试步骤：**

```bash
# 直接测试 GPG 签名
echo "test" | gpg --clearsign

# 检查 GPG agent 状态
gpgconf --list-components

# 检查密钥过期时间
gpg --list-keys --keyid-format=long <KEY_ID>

# 重启 GPG agent
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent

# 再次尝试 commit
git commit -m "test"
```

### 每次 commit 都提示输入口令（缓存不工作）

**Windows 用户：** 这通常是由双 GPG 安装问题引起。详细诊断见 [Windows Setup - Passphrase Caching Troubleshooting](references/windows-setup.md) 和 [Troubleshooting - Windows Caching Issue](references/troubleshooting.md)。

**可能原因：**

1. **配置文件位置错误（Windows）：**

   ```bash
   # 验证 Git 使用的是哪个 GPG
   git config --global gpg.program

   # 检查 GPG 的配置目录
   gpgconf --list-dirs homedir

   # 确保配置存在于正确位置
   # Windows (Gpg4win): %APPDATA%\gnupg\gpg-agent.conf
   # Windows (Git Bash): ~/.gnupg/gpg-agent.conf
   # Linux/macOS/WSL: ~/.gnupg/gpg-agent.conf
   ```

2. **gpg-agent.conf 未被加载：**

   ```bash
   # 重启 agent 以加载配置
   gpgconf --kill gpg-agent
   gpgconf --launch gpg-agent

   # 验证配置已加载
   gpgconf --list-options gpg-agent | grep cache-ttl
   ```

3. **缓存 TTL 设置过低** → 在 `~/.gnupg/gpg-agent.conf` 中增加缓存时间

4. **同时运行多个 GPG agent：**

   ```bash
   # 终止所有 agent
   pkill gpg-agent

   # 启动单个 agent
   gpgconf --launch gpg-agent
   ```

## GPG 签名方式对比

| 方式 | 最适合 | 配置复杂度 | 密钥管理 |
| --- | --- | --- | --- |
| **GPG** | 功能完整、支持过期/吊销 | 中等 | 最灵活 |
| **SSH** | 最简单，复用现有 SSH 密钥 | 低 | 功能有限 |
| **S/MIME** | 使用 X.509 证书的企业场景 | 高 | 组织管理 |

### SSH 签名（替代方式）

**什么时候使用 SSH 而不是 GPG：**

- 简单个人项目
- 已经配置好 SSH 密钥
- 不需要过期/吊销功能
- 希望尽量减少配置

**设置方法：**

```bash
# 配置 Git 使用 SSH 签名
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true

# 将 SSH 密钥作为"签名密钥"添加到 GitHub
```

**局限性：**

- 不支持密钥过期
- 没有吊销追踪
- 需要 Git 2.34+（2021 年发布）

## 安全最佳实践

**完整安全指引**见 [references/security-best-practices.md](references/security-best-practices.md)。

**快速建议：**

- ✅ 个人密钥使用口令保护（至少 20 个字符）
- ✅ 配置缓存以平衡安全与便利（4-8 小时）
- ✅ 考虑密钥过期：个人项目不过期，企业工作设置 1 年
- ✅ 将私钥备份到加密保险库或密码管理器的安全文档中
- ⚠️ 绝不将未加密的密钥放在 Git 仓库、云存储或邮件中

## 资源

### 参考文档

- [Quick Command Reference](references/quick-reference.md) —— 常用 GPG 和 Git 签名命令速查
- [Windows Setup Guide](references/windows-setup.md) —— Gpg4win 安装、Git Bash 路径、安全考量
- [Passphrase Caching Configuration](references/passphrase-caching.md) —— 缓存策略、安全权衡
- [Security Best Practices](references/security-best-practices.md) —— 口令保护、密钥过期、备份策略
- [Troubleshooting Guide](references/troubleshooting.md) —— 常见问题与解决方案
- [GPG Agent Configuration Example](references/gpg-agent-config-example.conf) —— 配置文件示例
- [Test Scenarios](references/test-scenarios.md) —— skill 激活验证场景

### 官方文档

- [GitHub: Managing commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification)
- [GnuPG Documentation](https://www.gnupg.org/documentation/)
- [Git Documentation: Signing Your Work](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)

## 相关技能

- **git-commit**：完整 git commit 工作流，含 Conventional Commits 格式、安全协议和署名要求
- **config**：Git 高级配置，包括别名、凭证和性能调优
- **gpg-multi-key**：面向咨询顾问、CI/CD 自动化和企业团队的多密钥管理

## 版本历史

- v1.1.1（2025-11-25）：渐进式披露优化 —— 将测试场景抽取到 reference 文件，移除冗余的快速命令参考章节，SKILL.md 缩减至 500 行以内
- v1.1.0（2025-11-22）：排查增强 —— 新增 keyboxd daemon 竞态条件排查（T7777/T7829）、动态 GPG 检测工作流、Windows 启动最佳实践与官方设计对比现实的分析
- v1.0.5（2025-11-19）：优化 —— 为 SKILL.md 添加目录，将基本命令参考抽取到 quick-reference.md，行数从 530 减至 522
- v1.0.4（2025-11-17）：渐进式披露改进 —— 将安全最佳实践抽取到独立 reference 文件
- v1.0.3（2025-11-17）：对照官方文档验证 —— 修正 tag.gpgSign 大小写，更新最后验证日期
- v1.0.2（2025-11-12）：文档增强 —— 为长 reference 文件添加目录，记录测试场景
- v1.0.1（2025-11-13）：新增对 git-commit skill 的交叉引用
- v1.0.0（2025-01-09）：首次发布，从仓库文档迁移

## 最后更新

**日期：** 2025-11-28
**模型：** claude-opus-4-5-20251101
