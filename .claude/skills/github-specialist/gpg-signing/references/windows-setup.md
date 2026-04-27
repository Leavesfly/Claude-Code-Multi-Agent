# Windows GPG 设置指南

针对 commit 签名的 Windows 专属 GPG 设置综合指导。

## 目录

- [Gpg4win 安装](#gpg4win-installation)
  - [选项 1:官方安装程序(推荐,可控性高)](#option-1-official-installer-recommended-for-control)
  - [选项 2:winget(快速安装)](#option-2-winget-quick-install)
  - [验证安装](#verify-installation)
  - [配置 Git 使用 Gpg4win](#configure-git-to-use-gpg4win)
- [理解 Windows 上的双重 GPG 安装](#understanding-dual-gpg-installations-on-windows)
  - [两种 GPG 安装](#two-gpg-installations)
  - [为何重要](#why-this-matters)
  - [验证 Git 使用的是哪个 GPG](#verify-which-gpg-git-is-using)
  - [建议:使用 Gpg4win](#recommendation-use-gpg4win)
  - [验证命令](#verification-commands)
  - [在 GPG 安装之间切换](#switching-between-gpg-installations)
  - [用于故障排查的动态 GPG 检测](#dynamic-gpg-detection-for-troubleshooting)
- [理解 Windows 上的 Git Bash 路径](#understanding-git-bash-paths-on-windows)
  - [常见路径映射](#common-path-mappings)
  - [重要安全注意事项](#important-security-considerations)
  - [路径转换工具:cygpath](#path-conversion-utility-cygpath)
- [在 Windows 上生成 GPG 密钥](#gpg-key-generation-on-windows)
  - [使用命令行(推荐)](#using-command-line-recommended)
  - [使用 Kleopatra(GUI)](#using-kleopatra-gui)
- [Windows 专属配置](#windows-specific-configuration)
  - [GPG 主目录权限](#gpg-home-directory-permissions)
  - [Pinentry 配置(可靠缓存所必需)](#pinentry-configuration-required-for-reliable-caching)
  - [环境变量](#environment-variables)
  - [Windows 上的 GPG Agent 启动:官方设计与实际情况](#gpg-agent-startup-on-windows-official-design-vs-practical-reality)
- [常见 Windows 问题](#common-windows-issues)
  - [gpg: cannot open tty 'no tty': No such file or directory](#gpg-cannot-open-tty-no-tty-no-such-file-or-directory)
  - [gpg: signing failed: Screen or window too small](#gpg-signing-failed-screen-or-window-too-small)
  - [密码短语缓存未生效(每次 commit 都提示)](#passphrase-caching-not-working-prompted-every-commit)
  - [Windows 防火墙阻止 GPG Agent](#windows-firewall-blocking-gpg-agent)
  - [重启后 GPG 密钥未持久化](#gpg-keys-not-persisting-after-reboot)
- [Windows 专属安全注意事项](#windows-specific-security-considerations)
  - [BitLocker 磁盘加密](#bitlocker-disk-encryption)
  - [Windows Defender 防病毒](#windows-defender-antivirus)
  - [临时文件清理](#temporary-file-cleanup)
- [与 Git GUI 工具集成](#integration-with-git-gui-tools)
  - [GitKraken](#gitkraken)
  - [GitHub Desktop](#github-desktop)
  - [VS Code](#vs-code)
- [验证和测试](#verification-and-testing)
  - [测试 GPG 安装](#test-gpg-installation)
  - [验证路径](#verify-paths)
- [官方文档](#official-documentation)
- [最后验证时间](#last-verified)

## Gpg4win 安装

### 选项 1:官方安装程序(推荐,可控性高)

下载官方 [Gpg4win](https://gpg4win.org/thanks-for-download.html) 安装程序。

安装程序提供:

- Gpg4win 组件(GnuPG、Kleopatra 证书管理器)
- GUI 密钥管理工具
- 文档和指南
- 与 Windows shell 集成

### 选项 2:winget(快速安装)

```powershell
winget install --id GnuPG.Gpg4win -e --source winget
```

**注意:** winget 使用默认设置(无向导)。

### 验证安装

```powershell
# 检查 GPG 安装
where.exe gpg
# 预期输出: C:\Program Files (x86)\GnuPG\bin\gpg.exe

# 检查版本
gpg --version
```

### 配置 Git 使用 Gpg4win

```powershell
# 在 Git 配置中设置 GPG 程序路径
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"

# 验证配置
git config --global gpg.program
```

## 理解 Windows 上的双重 GPG 安装

**关键:** Windows 系统通常有**两个独立的 GPG 安装**,每个都有自己的密钥环和配置文件位置。理解这一点对成功设置 GPG 至关重要。

### 两种 GPG 安装

#### 1. Gpg4win(推荐)

- **位置:** `C:\Program Files (x86)\GnuPG\`
- **密钥环目录:** `%APPDATA%\gnupg\`(通常为 `C:\Users\<Username>\AppData\Roaming\gnupg\`)
- **配置文件:** `%APPDATA%\gnupg\gpg-agent.conf`
- **用途:** 独立 Windows GPG 安装,附带 GUI 工具(Kleopatra)
- **推荐用于:** Windows 用户,官方 Windows GPG 发行版

#### 2. Git Bash GPG(与 Git for Windows 捆绑)

- **位置:** `/usr/bin/gpg`(在 Git Bash 中),通常为 `C:\Program Files\Git\usr\bin\gpg.exe`
- **密钥环目录:** `~/.gnupg/`(Git Bash home),解析为 `C:\Users\<Username>\.gnupg\`
- **配置文件:** `~/.gnupg/gpg-agent.conf` → `C:\Users\<Username>\.gnupg\gpg-agent.conf`
- **用途:** 为 Git Bash Unix 风格环境捆绑的 GPG
- **不推荐用于:** Git commit 签名(应使用 Gpg4win)

### 为何重要

**每个 GPG 安装维护:**

- ✅ **独立的密钥环** - 在一个安装中生成的密钥在另一个中不可用
- ✅ **独立的配置目录** - 配置文件必须位于正确位置
- ✅ **独立的 GPG agent** - 每个运行自己的密码短语缓存

**常见问题:**

用户在 **Git Bash GPG**(`~/.gnupg/`)中生成密钥并配置密码短语缓存,但 Git 配置为使用 **Gpg4win**(`%APPDATA%\gnupg\`)。结果:

- ❌ 签名 commit 时密钥"未找到"
- ❌ 密码短语缓存配置被忽略
- ❌ 对使用哪个 GPG 感到困惑

### 验证 Git 使用的是哪个 GPG

```bash
# 检查 Git 的 GPG 配置
git config --global gpg.program

# 可能的输出:

# 1. Gpg4win(推荐,显式路径):
# "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
# → 密钥位于: %APPDATA%\gnupg\
# → 配置位于: %APPDATA%\gnupg\gpg-agent.conf

# 2. Git Bash GPG(空白或 /usr/bin/gpg):
# 空白输出或 "/usr/bin/gpg"
# → 密钥位于: C:\Users\<Username>\.gnupg\
# → 配置位于: C:\Users\<Username>\.gnupg\gpg-agent.conf

# 3. 验证配置目录位置
gpgconf --list-dirs homedir
# 这显示活动 GPG 使用的目录
```

### 建议:使用 Gpg4win

**我们建议显式配置 Git 使用 Gpg4win:**

```bash
# 将 GPG 程序设置为 Gpg4win
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```

**为何推荐 Gpg4win:**

- ✅ 官方 Windows GPG 发行版
- ✅ 包含 GUI 密钥管理(Kleopatra)
- ✅ 标准 Windows 安装位置(`%APPDATA%`)
- ✅ 更好的 Windows 集成(pinentry、agent 自动启动)
- ✅ 与 Git Bash 环境分离(更清晰的隔离)

**何时可能使用 Git Bash GPG:**

- 你想要 Unix 风格的 GPG 环境
- 你仅使用 Git Bash 进行开发
- 你需要将密钥与 Windows 系统隔离

### 验证命令

```bash
# 验证 PATH 中的 GPG
where.exe gpg
# 显示系统上所有 GPG 安装

# 列出活动 GPG 中的密钥
gpg --list-secret-keys --keyid-format=long
# 显示活动 GPG 可用的密钥

# 检查配置目录
gpgconf --list-dirs homedir
# 显示活动 GPG 查找配置/密钥的位置

# 验证 Git 的 GPG 设置
git config --global gpg.program
# 显示 Git 将使用哪个 GPG 进行签名
```

### 在 GPG 安装之间切换

**如果需要从 Git Bash GPG 切换到 Gpg4win:**

```bash
# 1. 从 Git Bash GPG 导出密钥
gpg --list-secret-keys --keyid-format=long
# 记录你的 KEY_ID

gpg --armor --export-secret-keys <KEY_ID> > /tmp/my-key.asc

# 2. 配置 Git 使用 Gpg4win
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"

# 3. 将密钥导入 Gpg4win
"C:/Program Files (x86)/GnuPG/bin/gpg.exe" --import /tmp/my-key.asc

# 4. 验证密钥已导入
"C:/Program Files (x86)/GnuPG/bin/gpg.exe" --list-secret-keys --keyid-format=long

# 5. 删除临时密钥文件(安全)
rm -f /tmp/my-key.asc
```

**如果在错误的 GPG 中生成了密钥并希望重新开始:**

```bash
# 在 Gpg4win 中生成新密钥
"C:/Program Files (x86)/GnuPG/bin/gpg.exe" --full-generate-key

# 配置 Git
git config --global user.signingkey <NEW_KEY_ID>
git config --global commit.gpgsign true

# 将新密钥添加到 GitHub
"C:/Program Files (x86)/GnuPG/bin/gpg.exe" --armor --export <NEW_KEY_ID>
```

### 用于故障排查的动态 GPG 检测

**问题陈述:**

在排查 GPG 签名问题(尤其是 daemon 启动问题)时,常见错误是从错误的 GPG 安装启动 GPG agent 或检查密钥。这导致:

- 当 Git 使用 Gpg4win 时从 Git Bash GPG 启动 keyboxd
- 在 Git 使用另一个安装时检查一个安装中的密钥
- 在错误的 GPG 配置目录中进行配置更改

#### 解决方案:始终先检测 Git 的 GPG 配置

**逐步动态检测工作流:**

##### 步骤 1:确定 Git 使用的是哪个 GPG

```bash
# 检查 Git 配置的 GPG
git config --get gpg.program

# 可能的输出:
# <gpg4win-path>/bin/gpg.exe  → Git 使用 Gpg4win
# /usr/bin/gpg                 → Git 使用 Git Bash GPG
# (无输出)                     → Git 使用系统 PATH
```

##### 步骤 2:提取 GPG 二进制路径和目录

```bash
# 如果输出是完整路径(Gpg4win 示例):
GPG_PATH=$(git config --get gpg.program)

# 如果无输出(使用 PATH):
GPG_PATH=$(where.exe gpg | head -1)

# 提取目录路径
GPG_DIR=$(dirname "$GPG_PATH")
```

##### 步骤 3:对所有操作使用该确切的 GPG

```bash
# 从正确的安装启动 keyboxd
"${GPG_DIR}/gpgconf.exe" --launch keyboxd

# 从正确的安装列出密钥
"${GPG_DIR}/gpg.exe" --list-secret-keys --keyid-format=long

# 检查正确安装的配置目录
"${GPG_DIR}/gpgconf.exe" --list-dirs homedir
```

#### 具体示例(Git 使用 Gpg4win)

```bash
# 1. 检查 Git 的 GPG 配置
$ git config --get gpg.program
<gpg4win-path>/bin/gpg.exe

# 2. 设置变量以便重用
$ GPG_PATH=$(git config --get gpg.program)
$ GPG_DIR=$(dirname "$GPG_PATH")

# 3. 从该 GPG 启动 keyboxd
$ "${GPG_DIR}/gpgconf.exe" --launch keyboxd

# 4. 验证 agent 已启动
$ "${GPG_DIR}/gpgconf.exe" --list-components | grep keyboxd
keyboxd:GnuPG Key Storage:<gpg-dir>/keyboxd.exe

# 5. 从该 GPG 列出密钥
$ "${GPG_DIR}/gpg.exe" --list-secret-keys --keyid-format=long
sec   ed25519/ABC123... 2025-01-01 [SC]
      Fingerprint...
uid           [ultimate] Your Name <your.email@example.com>
ssb   cv25519/DEF456... 2025-01-01 [E]

# 6. 检查该 GPG 的配置目录
$ "${GPG_DIR}/gpgconf.exe" --list-dirs homedir
<user-appdata>/gnupg
```

#### 要避免的常见错误

1. ❌ **未先检查 Git 配置就运行命令**
   - 假设 Git 使用 PATH 中的 GPG
   - 当 Git 使用 Git Bash GPG 时使用 Gpg4win 命令(或反之)

2. ❌ **在故障排查脚本中硬编码 GPG 路径**
   - 脆弱:`gpgconf --launch keyboxd`(使用 PATH,可能是错误的)
   - 健壮:先检查 `git config --get gpg.program`,然后使用该 GPG

3. ❌ **在单次故障排查会话中混合使用 GPG 安装**
   - 从 Gpg4win 启动 keyboxd,然后在 Git Bash GPG 中检查密钥
   - 一个安装中的配置更改不会影响另一个

#### 快速参考命令

**用于脚本编写(推荐):**

```bash
# 检测并使用 Git 配置的 GPG
GPG_PATH=$(git config --get gpg.program)
if [ -z "$GPG_PATH" ]; then
    # 如果无显式配置,回退到 PATH
    GPG_PATH=$(where.exe gpg | head -1)
fi
GPG_DIR=$(dirname "$GPG_PATH")

# 现在对所有操作使用 ${GPG_DIR}
"${GPG_DIR}/gpgconf.exe" --launch keyboxd
"${GPG_DIR}/gpg.exe" --list-secret-keys
"${GPG_DIR}/gpgconf.exe" --list-dirs homedir
```

**用于手动故障排查:**

```bash
# 1. 识别 Git 的 GPG
git config --get gpg.program

# 2. 如果是 Gpg4win(示例输出: <gpg4win-path>/bin/gpg.exe):
#    使用 Gpg4win 的 gpgconf/gpg 可执行文件的完整路径

# 3. 如果是 Git Bash GPG(示例输出: /usr/bin/gpg 或无输出):
#    直接使用系统 PATH(gpgconf、gpg 命令)
```

## 理解 Windows 上的 Git Bash 路径

Git Bash 在 Windows 上提供类 Unix 环境,具有 Unix 风格的路径约定。在处理临时文件、脚本和 GPG 密钥时,理解路径映射至关重要。

### 常见路径映射

| Git Bash 路径 | Windows 路径 | 描述 |
| --- | --- | --- |
| `/tmp/` | `C:\Users\YourUsername\AppData\Local\Temp\` | 临时文件目录 |
| `/c/` | `C:\` | C: 盘根目录 |
| `/d/` | `D:\` | D: 盘根目录 |
| `~` | `C:\Users\YourUsername\` | 用户主目录 |
| `/usr/bin/` | `C:\Program Files\Git\usr\bin\` | Git Bash 二进制文件 |
| `~/.gnupg/` | `C:\Users\YourUsername\.gnupg\` | GPG 配置和密钥环 |

### 重要安全注意事项

**`/tmp/` 中的临时文件:**

- ✅ **可以安全清理:** Windows 磁盘清理可以安全删除这些文件
- ⚠️ **并非真正临时:** 文件会持续存在,直到手动删除或由 Windows 清理
- 🔒 **安全风险:** 敏感文件(如私钥)应显式删除

**临时 GPG 文件的最佳实践:**

```bash
# 在其他地方保护密钥后(GitHub Secrets、加密 vault)
rm -f /tmp/private-key.asc

# 验证删除
ls -lah /tmp/ | grep private-key
# 应该返回空
```

### 路径转换工具:`cygpath`

Git Bash 包含 `cygpath` 用于在 Unix 和 Windows 路径之间转换:

```bash
# 将 Unix 路径转换为 Windows 路径
cygpath -w /tmp/
# 输出: C:\Users\YourUsername\AppData\Local\Temp

# 将 Windows 路径转换为 Unix 路径
cygpath -u "C:\Program Files\Git"
# 输出: /c/Program Files/Git

# 获取当前目录的绝对 Windows 路径
cygpath -wa .
```

**常见用例:**

1. **查找临时文件存储位置:**

   ```bash
   # 以 Unix 格式列出文件
   ls /tmp/

   # 获取 Windows 位置以便在资源管理器中查看
   cygpath -w /tmp/
   ```

2. **将路径传递给 Windows 应用程序:**

   ```bash
   # 正确 - 先转换为 Windows 路径
   notepad.exe $(cygpath -w ~/file.txt)

   # 错误 - Windows 应用无法理解 Unix 路径
   notepad.exe ~/file.txt
   ```

3. **调试脚本输出位置:**

   ```bash
   echo "File created at: $(cygpath -w $OUTPUT_FILE)"
   ```

## 在 Windows 上生成 GPG 密钥

### 使用命令行(推荐)

```bash
# 交互式生成密钥
gpg --full-generate-key

# 按照提示:
# 1. 密钥类型: (9) ECC(签名和加密) *默认*
# 2. 曲线: (1) Curve 25519 *默认*
# 3. 过期时间: 0 = 永不过期
# 4. 真实姓名: Your Name
# 5. 电子邮件: your.verified@email.com
# 6. 密码短语: 强密码,20+ 字符

# 列出密钥以获取 KEY_ID
gpg --list-secret-keys --keyid-format=long
```

### 使用 Kleopatra(GUI)

Kleopatra 随 Gpg4win 一起安装,提供图形界面:

1. 从开始菜单打开 Kleopatra
2. 文件 → 新建 OpenPGP 密钥对
3. 输入姓名和电子邮件
4. 高级设置:
   - 密钥材料: Ed25519(Curve 25519)
   - 过期时间: 永不过期(或设置日期)
   - 密码短语: 必需
5. 创建
6. 导出公钥 → 添加到 GitHub

## Windows 专属配置

### GPG 主目录权限

**位置:** `C:\Users\<Username>\.gnupg\`

**安全警告:** 如果看到"unsafe permissions on homedir":

1. 右键单击 `.gnupg` 文件夹 → 属性 → 安全
2. 点击"高级" → "禁用继承"
3. 删除除你的账户外的所有用户
4. 确保你的账户具有"完全控制"权限
5. 应用更改

### Pinentry 配置(可靠缓存所必需)

Pinentry 是提示输入 GPG 密码短语的工具。在 Windows 上,显式配置 pinentry 程序对于可靠的密码短语缓存**至关重要**。

**为何需要此配置:**

- 如果没有显式的 `pinentry-program` 配置,GPG 可能无法正确缓存密码短语
- 导致每次 commit 都提示输入密码短语,即使设置了正确的缓存 TTL
- Windows GPG agent 需要显式的 pinentry 路径才能可靠运行

**推荐配置:**

```bash
# 对于 Gpg4win 用户(大多数 Windows 用户)
# 添加到 %APPDATA%\gnupg\gpg-agent.conf
cat >> "$APPDATA/gnupg/gpg-agent.conf" << 'EOF'
# Windows 必需:指定 pinentry 程序以实现可靠缓存
pinentry-program C:/Program Files (x86)/GnuPG/bin/pinentry-basic.exe
EOF

# 重启 agent 以应用更改
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

**对于 Git Bash GPG 用户(少见):**

```bash
# 添加到 ~/.gnupg/gpg-agent.conf
cat >> ~/.gnupg/gpg-agent.conf << 'EOF'
pinentry-program C:/Program Files (x86)/GnuPG/bin/pinentry-basic.exe
EOF

# 重启 agent
gpgconf --kill gpg-agent
```

**可用的 pinentry 程序:**

- `pinentry-basic.exe` - 简单 GUI 对话框(推荐,默认)
- `pinentry-w32.exe` - Windows 原生对话框
- `pinentry-qt.exe` - 基于 Qt 的 GUI(如果安装了 Qt)
- `pinentry.exe` - 自动选择(可能导致问题)

**验证配置已加载:**

```bash
# 检查是否配置了 pinentry-program
gpgconf --list-options gpg-agent | grep pinentry

# 测试密码短语缓存
git commit --allow-empty -m "Test 1"  # 将提示
git commit --allow-empty -m "Test 2"  # 如果缓存有效,不应提示
```

### 环境变量

**GPG_TTY(用于 Git Bash/WSL):**

```bash
# 添加到 ~/.bashrc
export GPG_TTY=$(tty)

# 或为当前会话设置
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
source ~/.bashrc
```

### Windows 上的 GPG Agent 启动:官方设计与实际情况

**理解设计与现实的差距:**

GnuPG agent(gpg-agent 和 keyboxd)官方设计为按需自动启动,但 Windows 11 经常因竞态条件和 daemon 初始化问题而出现启动失败。

#### 官方设计(应该发生的情况)

**按需自动启动:**

- GnuPG agent 设计为在需要时自动启动
- 官方 GnuPG 文档声明:"无需手动启动"
- 当 GPG 工具(如 Git)尝试使用时,agent 会启动
- 应该透明工作,无需用户干预

**为何这种设计合理:**

- 减少系统资源使用(仅在需要时运行 agent)
- 简化用户体验(无需手动配置)
- 在 Linux 和 macOS 系统上运行良好
- 与桌面环境提供干净集成

#### Windows 现实(实际发生的情况)

**已知问题:**

1. **冷启动连接失败:**
   - **Bug T7777**: "Gpg4win 在冷启动时无法连接到 keyboxd"
   - 影响 GPG4win 4.4.1 和 5.0 beta
   - Windows 重启后的第一次操作失败,挂起 30-60 秒
   - 后续尝试通常成功

2. **根本原因:**
   - **Bug T7829**: "w32: socket 文件已存在时的 daemon 启动和连接竞态"
   - Windows 上 daemon 启动时的竞态条件
   - **状态**: 已在 master 分支修复(commit rGae431b04370f)
   - 将包含在即将发布的 GPG4win 稳定版本中

3. **症状:**
   - `error: gpg failed to sign the data`
   - `can't connect to keyboxd` 错误
   - 重启后第一次 commit 超时 30-60 秒
   - 间歇性失败,需要重试

#### 推荐方法

##### 主要策略:依赖官方设计

信任按需自动启动(官方设计)作为第一道防线:

```bash
# 让 GPG 自动处理 agent 启动
git commit -m "message"

# 如果失败,立即重试
git commit -m "message"  # 第二次尝试通常成功
```

**为何有效:**

- 符合官方 GnuPG 设计
- 避免可能导致冲突的变通方法
- 未来 GPG4win 版本将提高可靠性(T7829 修复)
- 最简单的方法,配置最少

**如果冷启动问题持续:**

当自动启动持续失败时,使用这些轻量级变通方法:

1. **选项 A:在第一次 commit 前打开 Kleopatra**
   - 启动 Kleopatra GUI(开始菜单 → Kleopatra)
   - 作为副作用触发 agent 启动
   - 无需配置更改
   - 可靠运行,GUI 友好

2. **选项 B:手动启动 agent(命令行)**
   - 检测 Git 的 GPG:`git config --get gpg.program`
   - 从该 GPG 的目录启动 keyboxd
   - 示例:`"<gpg-dir>/gpgconf.exe" --launch keyboxd`
   - 详见[动态 GPG 检测](#dynamic-gpg-detection-for-troubleshooting)

3. **选项 C:失败时重试**
   - 如果第一次 commit 因 agent 错误失败,立即重试
   - 第二次尝试通常成功(agent 现在正在运行)
   - 无需配置
   - 适用于偶尔失败

#### 缓存持续时间配置

**通过减少密码短语提示频率来改善用户体验:**

**通过 Kleopatra GUI(Windows 用户推荐):**

1. 打开 Kleopatra(开始菜单 → Kleopatra)
2. 设置 → 配置 Kleopatra → GnuPG 系统 → 私钥
3. 设置缓存超时:
   - **默认缓存 TTL**: 28800 秒(8 小时)
   - **最大缓存 TTL**: 86400 秒(24 小时)
4. 点击确定,重启 GPG agent

**通过 gpg-agent.conf(高级用户):**

编辑 `<user-appdata>/gnupg/gpg-agent.conf`:

```conf
# 缓存密码短语 8 小时无活动
default-cache-ttl 28800

# 无论使用情况如何,最大缓存时间 24 小时
max-cache-ttl 86400
```

重启 agent 以应用:

```bash
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

**关键:** 确保你正在编辑正确 GPG 安装的配置文件。详见[双重 GPG 安装](#understanding-dual-gpg-installations-on-windows)。

#### 任务计划程序启动(仅最后手段)

**不官方推荐** - 仅用于严重的持续问题:

**为何不推荐:**

- 与官方自动启动设计冲突
- 可能导致权限问题或竞态条件
- 绕过官方 daemon 管理
- 创建维护负担(需要管理脚本)
- 未来 GPG4win 版本可能破坏此方法

**何时考虑:**

- 每次重启都出现冷启动问题
- 上述选项 A/B/C 无法解决问题
- 你需要登录时保证 agent 可用
- 愿意接受维护开销

**实现(如果绝对必要):**

创建任务计划程序任务以在登录时启动 keyboxd:

1. 打开任务计划程序(开始 → 任务计划程序)
2. 创建基本任务 → 名称:"GPG Agent Startup"
3. 触发器:"当我登录时"
4. 操作:"启动程序"
5. 程序:`<gpg-dir>/gpgconf.exe`
6. 参数:`--launch keyboxd`
7. 完成,通过重启测试

**重要:** 将 `<gpg-dir>` 替换为从 `git config --get gpg.program` 检测到的实际 GPG 安装路径。

#### 未来展望

**好消息:**

- T7829 修复已合并到 GnuPG master 分支(commit rGae431b04370f)
- 将包含在即将发布的 GPG4win 稳定版本中
- 应显著提高 Windows 可靠性
- 官方自动启动将更可靠地工作

**这意味着什么:**

- 当前变通方法是临时的
- 未来版本可能不需要手动干预
- 与官方设计(自动启动)保持一致具有向前兼容性
- 任务计划程序脚本可能变得不必要

## 常见 Windows 问题

### "gpg: cannot open tty 'no tty': No such file or directory"

**原因:** GPG 无法找到终端以进行密码短语提示。

**解决方案:**

```bash
# 设置 GPG_TTY
export GPG_TTY=$(tty)

# 或使用 GUI pinentry
echo "pinentry-program \"C:\\Program Files (x86)\\GnuPG\\bin\\pinentry-w32.exe\"" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```

### "gpg: signing failed: Screen or window too small"

**原因:** 终端窗口对于 pinentry curses 界面太小。

**解决方案:**

改用 GUI pinentry:

```bash
echo "pinentry-program \"C:\\Program Files (x86)\\GnuPG\\bin\\pinentry-w32.exe\"" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```

### 密码短语缓存未生效(每次 commit 都提示)

**症状:** 你在 `gpg-agent.conf` 中配置了缓存设置,但每次 commit 仍然提示输入密码短语。

**根本原因:** 这几乎总是由于 Windows 双重 GPG 安装问题导致配置了**错误的配置文件位置**。

**诊断:**

```bash
# 1. 验证 Git 使用的是哪个 GPG
git config --global gpg.program

# 2. 检查 GPG 的实际配置目录
gpgconf --list-dirs homedir

# 3. 检查配置文件是否存在于正确位置
# 对于 Gpg4win(如果 Git 使用 C:/Program Files (x86)/GnuPG/bin/gpg.exe):
ls "$APPDATA/gnupg/gpg-agent.conf"

# 对于 Git Bash GPG(如果 Git 使用 /usr/bin/gpg 或空白):
ls ~/.gnupg/gpg-agent.conf

# 4. 验证缓存设置已加载
gpgconf --list-options gpg-agent | grep cache-ttl
# 应显示你配置的值(例如 28800 表示 8 小时)
```

#### 解决方案 1:在正确位置创建配置(最常见)

如果 `git config --global gpg.program` 显示 `C:/Program Files (x86)/GnuPG/bin/gpg.exe`(Gpg4win):

```bash
# 在 Gpg4win 位置创建/编辑配置
cat > "$APPDATA/gnupg/gpg-agent.conf" << 'EOF'
# 缓存密码短语 8 小时
default-cache-ttl 28800
max-cache-ttl 86400
allow-preset-passphrase

# Windows 必需:pinentry 程序
pinentry-program C:/Program Files (x86)/GnuPG/bin/pinentry-basic.exe
EOF

# 重启 GPG agent
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent

# 验证设置已加载
gpgconf --list-options gpg-agent | grep cache-ttl
# 应显示: default-cache-ttl:24:0:... 28800

# 测试缓存
git commit --allow-empty -m "Test 1"  # 将提示
git commit --allow-empty -m "Test 2"  # 不应提示
```

#### 解决方案 2:缺少 pinentry-program 行

即使配置位置正确,如果没有 `pinentry-program`,缓存也可能失败:

```bash
# 将 pinentry-program 添加到你的配置
echo "pinentry-program C:/Program Files (x86)/GnuPG/bin/pinentry-basic.exe" >> "$APPDATA/gnupg/gpg-agent.conf"

# 重启 agent
gpgconf --kill gpg-agent
```

#### 解决方案 3:多个 GPG Agent 正在运行

```bash
# 终止所有 GPG agent
taskkill /F /IM gpg-agent.exe

# 启动单个 agent
gpgconf --launch gpg-agent
```

**验证:**

应用解决方案后,验证缓存是否有效:

```bash
# 第一次 commit 将提示输入密码短语
git commit --allow-empty -m "Cache test 1"

# 第二次 commit 不应提示(使用缓存的密码短语)
git commit --allow-empty -m "Cache test 2"

# 如果仍然提示,检查配置是否已加载
gpgconf --list-options gpg-agent | grep -E "(cache-ttl|pinentry)"
```

**另见:** [密码短语缓存参考](passphrase-caching.md)了解详细的缓存配置指导。

### Windows 防火墙阻止 GPG Agent

**症状:** GPG agent 无法启动或无法通信。

**解决方案:**

1. Windows 安全中心 → 防火墙和网络保护
2. 允许应用通过防火墙
3. 添加:`C:\Program Files (x86)\GnuPG\bin\gpg-agent.exe`
4. 允许专用网络和公用网络

### 重启后 GPG 密钥未持久化

**原因:** 密钥存储在临时驱动器或密钥环位置不正确。

**验证密钥环位置:**

```bash
# 应在用户主目录中
gpgconf --list-dirs homedir
# 预期: C:\Users\<Username>\AppData\Roaming\gnupg
```

**如果不正确,设置 GNUPGHOME:**

```bash
# 添加到环境变量
setx GNUPGHOME "C:\Users\<Username>\.gnupg"
```

## Windows 专属安全注意事项

### BitLocker 磁盘加密

**推荐:** 在包含 `.gnupg` 目录的驱动器上启用 BitLocker。

- 静态 GPG 密钥受驱动器加密保护
- 密码短语保护的密钥添加第二层(纵深防御)
- 如果笔记本电脑丢失或被盗,这一点很重要

**检查 BitLocker 状态:**

```powershell
manage-bde -status C:
```

**启用 BitLocker:**

1. 设置 → 系统 → 存储 → 高级存储设置
2. 管理 BitLocker → 启用 BitLocker
3. 安全保存恢复密钥(不与 GPG 备份一起保存)

### Windows Defender 防病毒

**性能排除项:**

将 GPG 目录添加到排除项:

1. Windows 安全中心 → 病毒和威胁防护 → 管理设置
2. 排除项 → 添加或删除排除项
3. 添加文件夹:`C:\Users\<Username>\.gnupg`
4. 添加文件夹:`C:\Program Files (x86)\GnuPG`

**注意:** 仅在你信任 GPG 安装来源时才这样做。

### 临时文件清理

**导出敏感 GPG 数据后:**

```bash
# 首先验证文件已在其他地方保护!

# 从 /tmp/ 删除
rm -f /tmp/private-key.asc

# 验证删除
ls -lah /tmp/ | grep private-key
# 应该返回空

# 可选运行磁盘清理
cleanmgr.exe
```

## 与 Git GUI 工具集成

### GitKraken

GitKraken 可以自动管理 GPG 密钥:

1. 文件 → 首选项 → GPG 首选项
2. 选择"使用 GPG 签名" → 启用
3. 签名密钥 → 创建或导入
4. GitKraken 将处理密钥生成和 Git 配置

**资源:**

- [GitKraken GPG 签名指南](https://help.gitkraken.com/gitkraken-desktop/commit-signing-with-gpg/)

### GitHub Desktop

GitHub Desktop 不原生支持 GPG 签名。改用命令行或 GitKraken。

### VS Code

VS Code 自动使用 Git 的配置:

- 如果 Git 配置为 GPG 签名,VS Code 将提示输入密码短语
- 确保 GPG agent 正在运行并已配置

## 验证和测试

### 测试 GPG 安装

```bash
# 测试 GPG 工作
echo "test" | gpg --clearsign

# 测试密钥签名
git commit --allow-empty -m "Test GPG signing"

# 验证签名
git log --show-signature -1
```

### 验证路径

```bash
# 检查 Git 的 GPG 程序路径
git config --global gpg.program

# 检查 GPG 密钥环位置
gpgconf --list-dirs homedir

# 检查 agent 是否正在运行
gpgconf --list-components | grep gpg-agent
```

## 官方文档

- [Gpg4win 官方文档](https://www.gpg4win.org/documentation.html)
- [GnuPG Windows 安装指南](https://www.gnupg.org/download/index.html#binary)
- [GitHub: 管理 commit 签名验证](https://docs.github.com/en/authentication/managing-commit-signature-verification)

## 最后验证时间

2025-11-22
