# 安装后配置

Git GUI 工具安装后的配置步骤。

## 1. 配置 Git 身份（如果尚未配置）

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

大多数 GUI 工具会在首次启动时提示你配置。

---

## 2. 添加 SSH 密钥（用于 SSH clone）

GUI 工具通常会集成你系统的 SSH 密钥（`~/.ssh/id_ed25519`、`~/.ssh/id_rsa`）。

如果你还没有 SSH 密钥，请参见 **setup** skill 了解如何生成 SSH 密钥。

---

## 3. 与 Git 托管服务进行认证

大多数 GUI 工具在首次使用时会提示你通过 GitHub、GitLab、Bitbucket 等进行认证。

**GitHub**：使用 OAuth（浏览器认证）
**GitLab/Bitbucket**：使用 OAuth 或 Personal Access Token
**自建服务**：使用用户名/密码或 token

---

## 4. 配置 Diff/Merge 工具（可选）

某些 GUI 工具会把自己设置为 Git 配置中的默认 diff/merge 工具。你可以验证或修改：

```bash
# View current diff tool
git config --get diff.tool

# View current merge tool
git config --get merge.tool

# Set a different diff tool (if needed)
git config --global diff.tool <tool-name>

# Set a different merge tool (if needed)
git config --global merge.tool <tool-name>
```

---

**最后更新：** 2025-11-28
