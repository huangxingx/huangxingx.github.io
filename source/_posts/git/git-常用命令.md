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

> git pull

> git push

> git checkout <branch>

```shell
# 切换分支到本地 develop 分支
git checkout develop

# 从当前分支新建一个分支 develop，并切换到 develop 分支
git checkout -b develop 
```

> git status

> git log

> git stash

```shell
# 清除所有
git stash clear
```



> git branch

```shell
# 删除远端分支
git push origin --delete release/V1.10.1
# 删除本地分支
git branch -d fenzhi
```

> rebase

```shell
# 变基到两个提交之前
git rebase -i HEAD~2
```

> submodule

```shell
# 所有子模块执行 git reset --hard
git submodule foreach --recursive git reset --hard

# 更新子模块
git submodule  update --init 	
git submodule  update --remote

```

> filter-branch 批量修改提交人

```shell
git filter-branch -f --env-filter '
if [ "$GIT_AUTHOR_NAME" = "oldName" ]
then
export GIT_AUTHOR_NAME="newName"
export GIT_AUTHOR_EMAIL="newEmail"
fi
' HEAD

git filter-branch -f --env-filter '
if [ "$GIT_COMMITTER_NAME" = "oldName" ]
then
export GIT_COMMITTER_NAME="newName"
export GIT_COMMITTER_EMAIL="newEmail"
fi
' HEAD
```

