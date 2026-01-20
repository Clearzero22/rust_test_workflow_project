# Git 合并策略对比

## 场景设置

假设有以下分支结构：

```
main 分支:     M1 ---- M2
                        \
feature 分支:             F1 ---- F2 ---- F3
```

---

## 1. Squash 合并

### 执行命令
```bash
gh pr merge 2 --squash --delete-branch
```

### 合并结果
```
main 分支:     M1 ---- M2 ---- S
                              ↑
                    F1+F2+F3 压缩成一个提交
```

### 特点
- ✓ 只有一个新提交进入 main
- ✓ 提交信息可以自定义
- ✗ 丢失了 F1、F2、F3 的独立历史
- ✓ feature 分支被删除

### 适合
- 日常功能开发
- 不需要保留详细历史
- 团队协作

---

## 2. Merge 合并

### 执行命令
```bash
gh pr merge 2 --merge --delete-branch
```

### 合并结果
```
                F1 ---- F2 ---- F3
               /                   \
main 分支:     M1 ---- M2 --------- M
                                      ↑
                              合并提交（2个父提交）
```

### 特点
- ✓ 保留所有原始提交
- ✓ 保留分支结构
- ✗ 历史图有分叉，稍复杂
- ✓ 可以看到何时合并的

### 适合
- 需要保留完整开发历史
- 功能分支提交很少
- 开源项目

---

## 3. Rebase 合并

### 执行命令
```bash
gh pr merge 2 --rebase --delete-branch
```

### 合并结果
```
main 分支:     M1 ---- M2 ---- F1' ---- F2' ---- F3'
                                    ↑
                    F1、F2、F3 被重新应用到 M2 后
```

### 特点
- ✓ 线性历史，无分叉
- ✓ 保留每个提交（但时间戳会变）
- ✗ 改写了历史（有风险）
- ✗ 如果有冲突需要逐个解决

### 适合
- 清理历史
- 保持线性提交
- 小型功能分支

---

## 快速选择指南

| 你的需求 | 推荐方式 |
|---------|---------|
| 默认/日常开发 | `--squash` |
| 需要保留完整历史 | `--merge` |
| 喜欢线性历史 | `--rebase` |
| 功能分支有很多"调试提交" | `--squash` |
| 功能分支提交很干净 | `--merge` 或 `--rebase` |
| 不确定 | `--squash` |

---

## `--delete-branch` 参数

这个参数会在合并后删除远程分支：

```bash
gh pr merge 2 --squash --delete-branch
#                        ↑
#                合并后删除远程 feature 分支
```

本地分支需要手动删除：
```bash
git branch -d feature-branch
```

---

## 实际对比

假设 feature 分支有 3 个提交：

### Squash 后 main 分支
```
abc123 (M2) - Initial commit
def456 (S)  - Add feature (squashed)
```

### Merge 后 main 分支
```
abc123 (M2) - Initial commit
ghi789 (M)  - Merge pull request #2
  ├─ jkl012 (F1) - First commit
  ├─ mno345 (F2) - Second commit
  └─ pqr678 (F3) - Third commit
```

### Rebase 后 main 分支
```
abc123 (M2) - Initial commit
jkl013 (F1') - First commit
mno346 (F2') - Second commit
pqr679 (F3') - Third commit
```
