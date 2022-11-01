---
title: git 常用命令
tags:
  - git
categories:
  - git
auto_excerpt:
  enable: true
  length: 150
date: 2021-09-08 15:43:04
---

**git pull**

**git push** 

**git checkout <branch>**

```shell
# 切换分支到本地 develop 分支
git checkout develop

# 从当前分支新建一个分支 develop，并切换到 develop 分支
git checkout -b develop 
```

**git status**

**git log**

**git stash**

**git branch**

**git rebase**

```shell
# 变基到两个提交之前
git rebase -i HEAD~2
```

**git submodule**

```shell
# 所有子模块执行 git reset --hard
git submodule foreach --recursive git reset --hard

# 更新子模块
git submodule  update --init 	
git submodule  update --remote

```

