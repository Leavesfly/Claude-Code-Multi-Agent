# Git 内置 GUI 工具

Git 自带两个内置的 GUI 工具，在所有平台上无需额外安装即可使用。

## gitk —— repository 浏览器

**用途**：可视化 repository 历史

```bash
# View current branch history
gitk

# View all branches
gitk --all

# View specific file history
gitk path/to/file

# View commits by specific author
gitk --author="John Doe"
```

**特性：**

- ✅ 随 Git 一起发布（无需安装）
- ✅ 快速、轻量
- ✅ 适合快速浏览历史
- ⚠️ UI 较陈旧
- ⚠️ 只读（仅用于可视化）

---

## Git GUI —— commit 工具

**用途**：暂存变更并创建 commit

```bash
# Open Git GUI in current repository
git gui

# Open Git GUI for specific repository
git gui --repo /path/to/repo
```

**特性：**

- ✅ 随 Git 一起发布（无需安装）
- ✅ 轻量
- ✅ 适合暂存 / commit
- ⚠️ UI 较陈旧
- ⚠️ 功能有限（没有 branch 管理、merge 等）

---

**最后更新：** 2025-11-28
