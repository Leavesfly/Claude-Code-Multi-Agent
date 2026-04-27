# 工作流程和场景

Git 行结束配置的常见工作流程和实际场景。

## 场景 A：新 Repository（您控制）

```bash
# 1. Initialize repository
mkdir my-project && cd my-project && git init

# 2. Add .gitattributes FIRST
cat > .gitattributes << 'EOF'
* text=auto
*.sh text eol=lf
*.bat text eol=crlf
*.png binary
EOF

git add .gitattributes && git commit -m "Add .gitattributes"

# 3. Configure Git if not already
# Windows: git config --global core.autocrlf true
# Mac/Linux: git config --global core.autocrlf input

# 4. Add project files
git add . && git commit -m "Initial commit"
```

## 场景 B：现有 Repository 带有 .gitattributes

**新鲜克隆：**

```bash
git clone https://github.com/user/repo.git
# Files checked out with correct line endings automatically
```

**.gitattributes 更新后：**

```bash
git pull
git add --renormalize .
git status && git commit -m "Normalize line endings"
```

## 场景 C：外部 Repository 不带 .gitattributes

使用选项 1 配置（安全）：文件自动标准化 ✅
使用选项 2 配置（已损坏）：混合行结尾 ❌

## 场景 D：修复混合行结尾

```bash
# 1. Detect
git ls-files --eol | awk '{print $2}' | sort | uniq -c

# 2. Fix
cat > .gitattributes << 'EOF'
* text=auto
*.sh text eol=lf
*.bat text eol=crlf
*.png binary
EOF

# 3. Normalize
git add --renormalize .
git commit -m "Add .gitattributes and normalize line endings"
```
