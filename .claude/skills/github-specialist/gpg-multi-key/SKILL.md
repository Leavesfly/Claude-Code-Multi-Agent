---
name: gpg-multi-key
description: 面向咨询顾问、CI/CD 自动化和企业团队的进阶 GPG 多 key 管理策略。配置、设置、运行并执行多 key GPG workflow。在以下场景使用：管理多个 GPG key（个人 + 自动化、每个客户独立 key、企业 key），配置 CI/CD commit 签名，实施按客户隔离 key 策略，使用 Git conditional includes，设置自动签名，或把 GPG key 策略从单 key 扩展到多 key。
allowed-tools: Read, Bash, Glob, Grep
---

# GPG Multi-Key

面向不同场景的多 GPG key 管理进阶策略：个人开发、CI/CD 自动化、多客户咨询工作和企业环境。

## 概述

多 key GPG 策略能够实现：

- **关注点分离：** 个人 key、自动化 key、客户 key 各自独立
- **风险隔离：** 某一场景下的 key 被泄露不会影响其他场景
- **身份管理：** 为不同客户使用不同邮箱
- **安全灵活性：** 各场景可采用不同的 passphrase/过期策略

本 skill 覆盖从单 key（基础配置）到复杂多 key 架构的扩展。

## 何时使用本 skill

在以下场景使用：

- 配置带 GPG 签名的 CI/CD 自动化（GitHub Actions、GitLab CI）
- 作为咨询顾问管理多个客户身份
- 实施按客户隔离 key 的策略
- 配置 Git conditional includes 以自动切换 key
- 为成长中的团队规划 GPG key 策略
- 在安全性（passphrase 保护）与自动化（无 passphrase）之间取得平衡
- 从单 key 迁移到多 key 配置
- 理解不同 key 类型的安全取舍

## 前置条件

使用多 key GPG 策略前：

- **已安装 GPG：** 运行 `gpg --version`（应显示 GnuPG 2.2+）
- **已安装 Git：** 运行 `git --version`（conditional includes 需要 Git 2.23+）
- **熟悉 GPG 基础：** 单 key 配置基础参见 `git:gpg-signing` skill
- **CI/CD 场景：** GitHub/GitLab repo 访问权限和 Secrets 管理权限
- **咨询顾问场景：** 有条理的目录结构（推荐 `~/Clients/`）

## 策略选择指南

### 快速决策矩阵

| 场景 | 所需 key 数 | Passphrase | 过期 | 参考 |
| --- | --- | --- | --- | --- |
| **仅个人开发** | 1 个 key | 是 | 无（或 2 年） | 基础配置 |
| **个人 + CI/CD** | 2 个 key | 个人：是，自动化：否 | 无 | [场景 2](#场景-2个人--cicd-自动化) |
| **多客户咨询顾问** | 3+ 个 key | 全部：是 | 1 年 | [场景 3](#场景-3多客户咨询顾问) |
| **企业团队** | 1 个 key | 是 | 1 年 | [场景 4](#场景-4企业环境) |

### 快速决策流程图

```text
你是 GPG 新用户吗？
├─ 是 → 先使用 gpg-signing skill
└─ 否 → 继续下面

需要多个 key 吗？
├─ 否 → 使用 gpg-signing skill（场景 1）
└─ 是 → 继续下面

哪种情况适合你？
├─ 个人项目 + CI/CD 自动化 → 场景 2（个人 + CI/CD）
├─ 多客户（咨询顾问/承包商） → 场景 3（多客户）
├─ 企业/受监管环境 → 场景 4（企业）
└─ 不确定？ → 详见 references/scenarios.md 的对比
```

## 场景 1：基础配置（单个人 key）

**基础单 key 配置**请改用 `git:gpg-signing` skill。

**如果你是 GPG 新手**，请先使用 `git:gpg-signing` skill 完成：

- 安装和配置 GPG
- 生成你的第一个签名 key
- 设置 Git commit 签名
- 理解 passphrase 缓存

当你需要多个 key（CI/CD、客户、企业）时再回到本 skill。

## 场景 2：个人 + CI/CD 自动化

**双 key 策略：**

- **Key 1：个人开发** —— 带 passphrase 保护，用于交互式 commit
- **Key 2：CI/CD 自动化** —— 不带 passphrase（无人值守运行所需），存在 GitHub Secrets

**快速设置：**

1. 生成自动化 key（不带 passphrase）
2. 导出私钥：`gpg --armor --export-secret-keys <KEY_ID> > automation-private.asc`
3. 存到 GitHub Secrets → GPG_PRIVATE_KEY
4. 配置 GitHub Actions 导入并使用该 key

**安全取舍：** 自动化 key 没有 passphrase（无人值守运行所需），但与个人 key 隔离，并以加密形式存在 GitHub Secrets。

**完整 workflow：** 参见 [references/scenarios.md#scenario-2-personal--cicd](references/scenarios.md)

## 场景 3：多客户咨询顾问

**每客户一个 key 的策略：**

- 每个客户使用不同的 GPG key
- 按目录自动切换 key（Git conditional includes）
- 身份隔离（客户 A 的 commit ≠ 客户 B 的 commit）

**目录结构：**

```text
~/Clients/
├── ClientA/
│   ├── .gitconfig-clienta
│   └── projects/
└── ClientB/
    ├── .gitconfig-clientb
    └── projects/
```

**自动切换：**

全局 `~/.gitconfig` 使用 `includeIf` 按目录加载各客户配置。

**快速设置：**

1. 为每个客户生成 key（推荐 1 年过期）
2. 在 `~/.gitconfig` 中配置 conditional includes
3. 创建带客户专属 key 的 `.gitconfig-clienta`
4. 测试自动切换：`cd ~/Clients/ClientA && git config user.signingkey`

**完整 workflow：** 参见 [references/scenarios.md#scenario-3-multi-client-consultant](references/scenarios.md)

## 场景 4：企业环境

**遵循企业策略：**

你的组织会规定：

- key 算法与长度（RSA 4096、Ed25519）
- 过期时间（通常 6-12 个月）
- passphrase 要求
- 备份和 revocation 流程
- 是否需要硬件安全 key（YubiKey/Nitrokey）

**典型要求：**

- **算法：** RSA 4096 位（兼容性）或 Ed25519（现代）
- **过期：** 1 年（按策略轮换）
- **passphrase：** 必须（按复杂度规则）
- **revocation 证书：** 交给 IT 存档
- **备份：** IT 托管或批准的保险库

**完整 workflow：** 参见 [references/scenarios.md#scenario-4-enterprise-environment](references/scenarios.md)

## Git 如何选择使用哪个 key

**key 选择顺序：**

1. **repo 级别的本地配置**（具体 repo 中的 `.git/config`）
2. **conditional includes**（带 `includeIf` 的 `.gitconfig`）
3. **全局配置**（`~/.gitconfig`）

## 多 key 管理

### 列出所有 key

```bash
# List all secret (private) keys
gpg --list-secret-keys --keyid-format=long
```

### 向 GitHub 添加多个 key

GitHub 允许每个账号添加无限数量的 GPG key。

**步骤：**

1. 导出每个公钥：`gpg --armor --export <KEY_ID>`
2. 添加到 GitHub：Settings → SSH and GPG keys → New GPG key
3. 粘贴 ASCII-armored 公钥
4. 对每个 key 重复上述步骤

**邮箱验证：** 在 GitHub 账号中添加并验证所有工作邮箱。使用未验证邮箱签名的 commit 会显示为 "Unverified"。

## 快速命令参考

常见多 key 操作：

```bash
# List all keys
gpg --list-secret-keys --keyid-format=long

# Check which key Git is using
git config user.signingkey
git config user.email

# Switch key for specific repo
cd /path/to/repo
git config user.signingkey <KEY_ID>

# Test key switching with conditional includes
cd ~/Clients/ClientA/project
git config user.signingkey  # Should show CLIENT_A_KEY_ID

# Verify commit signature
git log --show-signature -1

# Export key for backup
gpg --armor --export-secret-keys <KEY_ID> > backup.asc
```

## 备份与恢复

**必要备份：**

- 私钥（全部 key —— 个人、自动化、客户）
- revocation 证书（每个 key 一份）
- passphrase（存在密码管理器里，**不要**和备份放一起）

**快速备份：**

```bash
# Export all private keys (encrypted)
gpg --armor --export-secret-keys > gpg-all-keys-backup.asc
gpg --symmetric --cipher-algo AES256 gpg-all-keys-backup.asc

# Secure storage locations
# ✅ Password manager (1Password, Bitwarden)
# ✅ Encrypted vault (VeraCrypt, Cryptomator)
# ✅ Hardware encrypted USB (in safe)
```

**恢复：**

```bash
# Decrypt and import
gpg --decrypt gpg-all-keys-backup.asc.gpg > gpg-all-keys-backup.asc
gpg --import gpg-all-keys-backup.asc
```

**完整流程：** 参见 [references/backup-recovery.md](references/backup-recovery.md)

## 安全最佳实践

### Passphrase 保护策略

| key 类型 | Passphrase | 原因 |
| --- | --- | --- |
| **个人** | 是（20+ 字符） | 纵深防御、笔电安全 |
| **自动化** | 否 | 无人值守的 CI/CD 必需 |
| **每客户** | 是（20+ 字符） | 客户数据需要高安全性 |
| **企业** | 是（按策略） | 合规要求 |

### 过期策略

| 场景 | 过期 | 原因 |
| --- | --- | --- |
| **个人项目** | 不过期 | GitHub 默认，手动轮换 |
| **自动化** | 不过期 | 简洁，被泄露时再轮换 |
| **每客户** | 1 年 | 企业最佳实践，强制复核 |
| **企业** | 按策略 | 通常 6 个月到 1 年 |

### key 分离的好处

**为什么使用多个 key：**

- ✅ 自动化 key 被泄露不影响个人 key
- ✅ 客户 key 被泄露仅影响该客户
- ✅ 各场景可采用不同安全策略（passphrase、过期）
- ✅ 身份隔离（每个客户使用正确邮箱）
- ✅ 可以单独 revoke/轮换某个 key，不影响其他

## 安全取舍汇总

| 决策 | 安全性 | 便利性 | 建议 |
| --- | --- | --- | --- |
| **个人 key 使用 passphrase** | ✅ 高 | ⚠️ 需要输入 | ✅ 始终使用 |
| **自动化 key 使用 passphrase** | ❌ 单因子 | ✅ 无人值守 | ✅ CI/CD 必需 |
| **key 过期** | ✅ 限制暴露 | ⚠️ 轮换开销 | ⚠️ 看场景 |
| **每客户独立 key** | ✅ 隔离 | ❌ 管理开销 | ✅ 咨询顾问适用 |
| **硬件 key（YubiKey）** | ✅ 最高安全性 | ❌ 物理依赖 | ⚠️ 仅企业 |

**总原则：** 在不妨碍完成任务的前提下，采用最强安全措施。

## 常见问题速答

### 什么时候该用多个 key 而不是单个 key？

以下情况使用多个 key：

- **个人 + CI/CD：** 需要无人值守签名（自动化 key 不能有 passphrase）
- **多客户：** 每个客户使用不同邮箱/身份（咨询顾问场景）
- **企业：** 组织策略要求轮换或隔离 key
- **不需要：** 单个项目 → 继续用一个 key（使用 gpg-signing skill）

### 自动化 key 和个人 key 有什么区别？

| 方面 | 个人 key | 自动化 key |
| --- | --- | --- |
| **Passphrase** | 是（保护） | 否（无人值守 CI/CD 必需） |
| **存储位置** | 本地 GPG keyring | GitHub/GitLab Secrets |
| **用途** | 交互式开发 | 仅自动化 workflow |
| **泄露风险** | 影响个人 commit | 仅影响自动化 commit |

### 如何在 key 之间切换？

**自动（推荐）：** 使用 Git conditional includes 按目录切换
**手动：** 对特定 repo 使用 `git config user.signingkey <KEY_ID>`
**全局：** 对所有 repo 使用 `git config --global user.signingkey <KEY_ID>`

### 把自动化 key 存到 GitHub Secrets 安全吗？

安全，但有注意事项：

- ✅ GitHub Secrets 在静态和传输中都有加密
- ✅ 仅授权团队成员可见
- ⚠️ key 没有 passphrase（单因子）
- ✅ 缓解方式：使用与个人 key 分离的 key（隔离）

## 资源

### 内部 reference

- [Multi-Key Scenarios Comparison](references/scenarios.md)
- [Real-World Implementation Example](references/implementation-example.md)
- [Backup and Recovery Guide](references/backup-recovery.md)

### 官方文档

- [GitHub: Managing commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification)
- [Git Conditional Includes](https://git-scm.com/docs/git-config#_conditional_includes)
- [GnuPG Documentation](https://www.gnupg.org/documentation/)

## 相关 skill

- **gpg-signing** —— 基础单 key 配置（场景 1）的前置
- **setup** —— Git 安装与配置的前置
- **config** —— 进阶 Git 配置相关主题（conditional includes）

## 版本历史

- v1.0.0（2025-01-09）：首次发布，从 repo 文档迁移
- v1.0.1（2025-11-12）：文档增强 —— 新增测试场景、相关 skill 章节
- v1.0.2（2025-11-12）：审计改进 —— 用执行类动词增强 description
- v1.0.3（2025-11-25）：MCP 验证审计 —— 全部命令和 workflow 已验证

## 最后更新

**日期：** 2025-11-28
**模型：** claude-opus-4-5-20251101

**已测试工具版本：**

- GnuPG：2.2+
- Git：2.23+
- GitHub CLI：2.0+（用于自动化场景）

所有配置已对照 gnupg.org、git-scm.com 和 docs.github.com 的官方文档验证。
