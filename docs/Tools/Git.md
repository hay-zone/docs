---
title: Git 概述
date: 2026-01-14
tags: 
 - Git
categories: 
 - Git
---
# Git 版本控制

## 1. Git 基础

### 核心概念
- 工作区(Working Directory)
- 暂存区(Staging Area)
- 本地仓库(Local Repository)
- 远程仓库(Remote Repository)

### 基本命令
```bash
git init              # 初始化仓库
git add .             # 添加到暂存区
git commit -m "msg"   # 提交到本地仓库
git push              # 推送到远程仓库
git pull              # 拉取远程更新
```

## 2. 分支管理

### 分支操作
```bash
git branch            # 查看分支
git branch <name>     # 创建分支
git checkout <name>   # 切换分支
git merge <name>      # 合并分支
git branch -d <name>  # 删除分支
```

### 分支策略
- Git Flow
- GitHub Flow
- GitLab Flow

## 3. 高级操作

### rebase
变基,重新应用提交。

```bash
git rebase master
```

### cherry-pick
挑选特定提交应用到当前分支。

```bash
git cherry-pick <commit-hash>
```

### stash
暂存工作区改动。

```bash
git stash        # 暂存
git stash pop    # 恢复
```

## 4. 最佳实践

- 提交信息要清晰
- 经常提交,小步快跑
- 使用 .gitignore
- 不要提交敏感信息
- 代码审查(Pull Request)
