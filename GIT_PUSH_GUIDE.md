# Git Push 完全指南

## 基本用法

### 最常用
```bash
git push                    # 推送当前分支到远程
git push origin main        # 推送到 main 分支
git push -u origin feature  # 推送并设置上游分支
```

### 推送标签
```bash
git push origin v1.0.0      # 推送单个标签
git push origin --tags      # 推送所有标签
git push origin --tags 'v1.*'  # 推送匹配的标签
```

### 删除远程分支
```bash
git push origin --delete feature    # 删除远程分支
git push origin :feature            # 另一种写法
```

### 强制推送
```bash
git push --force            # 强制推送（危险）
git push --force-with-lease # 安全的强制推送
```

## 常用选项

| 选项 | 说明 |
|------|------|
| `-u` | 设置上游分支，首次推送时使用 |
| `--all` | 推送所有分支 |
| `--tags` | 推送所有标签 |
| `--force` | 强制覆盖远程 |
| `--force-with-lease` | 安全强制推送 |
| `--dry-run` | 模拟运行，不实际推送 |
| `--delete` | 删除远程分支 |

## 实际示例

```bash
# 首次推送新分支
git checkout -b feature/new-feature
# ... 做一些修改 ...
git commit -m "Add new feature"
git push -u origin feature/new-feature

# 推送当前分支
git push

# 推送所有分支
git push --all

# 删除远程分支
git push origin --delete old-branch

# 推送标签
git tag v1.0.0
git push origin v1.0.0
```
