# .gitattributes 指南

用于换行符管理的 .gitattributes 配置完整指南。

## 什么是 .gitattributes？

`.gitattributes` 是 repository 级别的配置文件，明确声明 Git 如何处理特定文件。它会被纳入版本控制并对所有开发者生效，保证团队内行为一致。

## 常见模式

**最小配置：**

```gitattributes
* text=auto
```

**完整配置：**

```gitattributes
# Default behavior
* text=auto

# Documentation - LF
*.md text eol=lf
*.txt text eol=lf

# Shell scripts - MUST be LF (Unix requirement)
*.sh text eol=lf
*.bash text eol=lf

# PowerShell scripts - CRLF (Windows standard)
*.ps1 text eol=crlf
*.cmd text eol=crlf
*.bat text eol=crlf

# Configuration files - LF
*.json text eol=lf
*.yml text eol=lf
.gitignore text eol=lf

# Binary files
*.png binary
*.jpg binary
*.pdf binary
*.exe binary
```

## 属性参考

- `text=auto` —— 自动识别文本文件并规范化
- `text eol=lf` —— 在工作目录中使用 LF
- `text eol=crlf` —— 在工作目录中使用 CRLF
- `binary` —— 永不转换换行符

## 何时添加 .gitattributes

始终添加给：

- 你新建的 repository
- 你掌控的团队 repository
- 有跨平台贡献者的项目
- 包含 shell 脚本或特定平台文件的项目

## 为已有 repository 添加

```bash
# Create .gitattributes
vim .gitattributes

# Normalize existing files
git add --renormalize .

# Review and commit
git status
git commit -m "Add .gitattributes and normalize line endings"
```
