# 常见问题排查

常见的 Git 换行符问题与解决方案。

## 问题：`"warning: CRLF will be replaced by LF"`

**含义：** Git 正在正确执行规范化（仅为提示信息，不是错误）。

**解决方案：** 这是预期行为。如需抑制该警告（不推荐）：

```bash
git config --global core.safecrlf false
```

## 问题：文件显示错误的换行符

**症状：**

```bash
git ls-files --eol README.md
# Shows: i/lf w/crlf attr/text eol=lf
```

**解决方案：**

```bash
git add --renormalize .
git commit -m "Normalize line endings"
```

## 问题：Shell 脚本无法执行

**错误信息：** `bash: /bin/bash^M: bad interpreter`

**解决方案：**

```bash
# Immediate
dos2unix script.sh

# Permanent
echo "*.sh text eol=lf" >> .gitattributes
git add --renormalize script.sh
git commit -m "Fix shell script line endings"
```

## 问题：Git 显示每一行都被修改

**原因：** 换行符发生了变化（CRLF ↔ LF）。

**解决方案：**

```bash
git add --renormalize file.txt
git commit -m "Normalize line endings"
```

## 问题：二进制文件损坏

**解决方案：**

```bash
# Restore
git checkout HEAD -- file.png

# Prevent
echo "*.png binary" >> .gitattributes
git commit -m "Mark binary files"
```
