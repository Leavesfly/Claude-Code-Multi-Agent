# 命令参考

Git 换行符管理的完整命令参考。

## 配置

```bash
# View configuration
git config --list --show-origin | grep -E "autocrlf|eol|safecrlf"

# Set configuration
git config --global core.autocrlf true      # Or: input, false
git config --global core.eol native         # Or: lf, crlf
git config --global core.safecrlf warn      # Or: true, false
```

## 检查

```bash
# Check file attributes
git check-attr -a file.txt

# Check line endings
git ls-files --eol
git ls-files --eol | grep "\.md$"
git ls-files --eol README.md

# Find mismatches
git ls-files --eol | grep "w/crlf" | grep "eol=lf"
git ls-files --eol | grep "w/mixed"
```

## 规范化

```bash
# Normalize all files
git add --renormalize .

# Normalize specific file
git add --renormalize file.txt

# Review changes
git status
git diff --cached --stat
```

## 测试

```bash
# Create test file with CRLF
printf "Line 1\r\nLine 2\r\n" > test.txt

# Check line endings
file test.txt

# Test what Git will store
git add test.txt
git ls-files --eol test.txt
```

## Git LFS

```bash
# Install and track
git lfs install
git lfs track "*.psd"

# View and list
git lfs track
git lfs ls-files

# Migrate existing files
git lfs migrate import --include="*.png,*.jpg"
```
