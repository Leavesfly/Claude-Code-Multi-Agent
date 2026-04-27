---
name: github-issues
description: 用 gh CLI 查询和搜索 GitHub issues，并带 web 兜底。支持按 label、state、assignee 过滤以及全文搜索。在排查错误、确认 issue 是否已被报告、或寻找 workaround 时使用。
allowed-tools: Bash, Read, Glob, Grep, WebFetch, WebSearch
---

# GitHub Issues Lookup（GitHub Issue 查询）

查询和搜索 GitHub issues，用于排查、bug 追踪以及寻找 workaround。支持 `gh` CLI，并在不可用时自动降级到 web 方式。

## 概述

本 skill 为从任意 repo 查询 GitHub issues 提供完整指导。优先使用 GitHub CLI（`gh`）以获得快速、可靠的访问；当 gh 不可用时自动降级到 web 方式。

**核心价值：** 在排查错误、确认 bug 是否已被报告、或为已知问题寻找 workaround 时，快速找到相关 issue。

## 何时使用本 skill

在以下场景使用本 skill：

- **排查错误** —— 搜索与错误信息或症状匹配的 issue
- **确认 issue 是否存在** —— 报告 bug 前先搜重复项
- **寻找 workaround** —— 从 issue 讨论中发现解决方案
- **追踪特性** —— 搜索 feature request 及其状态
- **理解历史** —— 找出解释过去决策的已关闭 issue

**触发关键词：** github issues、search issues、find issue、bug report、issue lookup、gh issue、troubleshoot、workaround、known issue

## 前置条件

**推荐（非必需）：**

- **GitHub CLI（gh）** —— 从 <https://cli.github.com/> 安装
- **认证** —— 对 private repo 运行 `gh auth login`

即使没有 `gh`，本 skill 也能通过降级到 web 方式工作。

## 快速开始

### 搜索 issue（gh CLI）

```bash
# Check if gh is available
gh --version

# Search for issues by keyword
gh issue list --repo owner/repo --search "keyword" --state all

# Filter by label
gh issue list --repo owner/repo --label "bug" --state open

# View specific issue
gh issue view 11984 --repo owner/repo

# Search with multiple terms
gh issue list --repo owner/repo --search "error message here" --limit 20
```

### 搜索 issue（web 兜底）

当 gh 不可用时，使用 WebSearch 或 WebFetch：

```text
# Search via web (using WebSearch tool)
Search: "site:github.com/owner/repo/issues keyword"

# Direct URL pattern
https://github.com/owner/repo/issues?q=keyword
```

## 核心能力

### 1. 基础 issue 搜索

按关键词搜索 issue，匹配 title 和正文。

**gh CLI：**

```bash
# Basic keyword search (all states)
gh issue list --repo anthropics/claude-code --search "path doubling" --state all

# Open issues only
gh issue list --repo anthropics/claude-code --search "path doubling" --state open

# Closed issues only
gh issue list --repo anthropics/claude-code --search "path doubling" --state closed
```

**详细查询语法：** 参见 [references/query-patterns.md](references/query-patterns.md)

---

### 2. 按 label 过滤

用 repo 的 label 缩小范围。

```bash
# Single label
gh issue list --repo owner/repo --label "bug"

# Multiple labels (AND)
gh issue list --repo owner/repo --label "bug" --label "high-priority"

# Common label patterns
gh issue list --repo owner/repo --label "enhancement" --state open
gh issue list --repo owner/repo --label "documentation"
```

---

### 3. 按 assignee / author 过滤

根据 issue 的创建者或被指派者来查找。

```bash
# By assignee
gh issue list --repo owner/repo --assignee username

# By author
gh issue list --repo owner/repo --author username

# Combined filters
gh issue list --repo owner/repo --author username --state closed
```

---

### 4. 查看 issue 详情

获取完整 issue 内容，包括描述和评论。

```bash
# View issue (opens in terminal)
gh issue view 11984 --repo anthropics/claude-code

# View with comments
gh issue view 11984 --repo anthropics/claude-code --comments

# JSON output for parsing
gh issue view 11984 --repo anthropics/claude-code --json title,body,comments
```

---

### 5. Web 兜底策略

当 gh CLI 不可用时，降级到 web 方式。

**探测：**

```bash
# Check if gh is available
if command -v gh &> /dev/null; then
    echo "gh available"
else
    echo "falling back to web"
fi
```

**Web 方式（按优先级）：**

1. **WebSearch 工具**：`site:github.com/owner/repo/issues keyword`
2. **firecrawl MCP**：抓取 GitHub 搜索结果
3. **直接 URL**：`https://github.com/owner/repo/issues?q=keyword`

**详细兜底指南：** 参见 [references/web-fallback.md](references/web-fallback.md)

---

## 输出格式

本 skill 支持三种输出格式：

### Compact（默认）

每个 issue 一行，适合快速扫读：

```text
#11984 [open] Path doubling in PowerShell hooks (bug, hooks)
#11523 [closed] Fix memory leak in long sessions (bug, fixed)
#10892 [open] Add custom status line support (enhancement)
```

### Table（表格）

Markdown 表格形式，结构化展示：

```markdown
| # | State | Title | Labels |
| --- | --- | --- | --- |
| 11984 | open | Path doubling in PowerShell hooks | bug, hooks |
| 11523 | closed | Fix memory leak in long sessions | bug, fixed |
```

### Detailed（详细）

完整信息，用于深入调查：

```markdown
### #11984 - Path doubling in PowerShell hooks
**State:** open | **Labels:** bug, hooks | **Created:** 2024-12-01
**URL:** https://github.com/anthropics/claude-code/issues/11984

When using cd && in PowerShell, paths get doubled...
```

---

## 常见 workflow

### 排查错误

1. 从错误信息中提取关键词
2. 用这些词搜 issue
3. 同时看 open 和 closed issue
4. 在评论里找 workaround

```bash
# Example: Troubleshoot a specific error
gh issue list --repo anthropics/claude-code --search "ENOENT" --state all --limit 10
```

### 报告 bug 前

1. 搜索症状相似的已有 issue
2. 查 closed issue 里过去的修复
3. 若存在重复，把你的上下文作为评论补上

```bash
# Search before reporting
gh issue list --repo owner/repo --search "feature not working" --state all
```

### 寻找 workaround

1. 用问题关键词搜 closed issue
2. 找正文里包含 "workaround" 或 "solution" 的 issue
3. 看 issue 评论中的社区方案

```bash
# Find workarounds
gh issue list --repo owner/repo --search "workaround" --state closed --label "bug"
```

---

## 错误处理

### gh 未安装

```text
Error: gh: command not found

Solution: Install GitHub CLI from https://cli.github.com/
  - macOS: brew install gh
  - Windows: winget install --id GitHub.cli
  - Linux: See https://github.com/cli/cli/blob/trunk/docs/install_linux.md

Alternatively, falling back to web-based search...
```

### 未认证

```text
Error: gh requires authentication

Solution: Run `gh auth login` and follow the prompts.
For public repos, web fallback can be used without authentication.
```

### 触发限流

```text
Error: API rate limit exceeded

Solution: Wait 60 seconds before retrying.
Authenticated requests have higher limits (5000/hour vs 60/hour).
```

### 没有结果

```text
No issues found matching "very specific query"

Suggestions:
- Try broader search terms
- Remove filters (state, label)
- Check spelling
- Search closed issues too (--state all)
```

---

## 参考

**详细指南：**

- [references/gh-cli-guide.md](references/gh-cli-guide.md) —— 安装、认证、进阶用法
- [references/query-patterns.md](references/query-patterns.md) —— 搜索语法与过滤示例
- [references/web-fallback.md](references/web-fallback.md) —— 基于 web 的兜底策略

**相关 agent：**

- **history-reviewer** agent —— Git 历史探索与总结

---

## 测试场景

### 场景 1：基础 issue 搜索

**Query：** "Search for issues about hooks in the Claude Code repo"

**期望行为：**

- skill 在 "issues"、"hooks"、"Claude Code" 这些词上激活
- 检查 gh CLI 是否可用
- 运行 `gh issue list --repo anthropics/claude-code --search "hooks" --state all`
- 返回格式化后的结果

### 场景 2：排查错误

**Query：** "I'm getting a path doubling error in PowerShell. Is this a known issue?"

**期望行为：**

- skill 在 "error"、"known issue"、"PowerShell" 上激活
- 搜索相关 issue
- 找到 #11984 和类似 issue
- 若有则给出 workaround

### 场景 3：降级到 web

**Query：** "Search for issues but gh isn't installed"

**期望行为：**

- 检测到 gh 不可用
- 用 `site:github.com/owner/repo/issues` 降级到 WebSearch
- 以同样格式返回结果

## 版本历史

- **v1.0.0**（2025-12-26）：首次发布

---

## 最后更新

**日期：** 2025-12-05
**模型：** claude-opus-4-5-20251101

**审计状态：** NEW —— 等待首次审计
