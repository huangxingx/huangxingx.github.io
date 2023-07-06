---
title: git config
tags:
  - git
categories:
  - git
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
---

## git config

官方文档地址：<https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE>
自定义 Git - 配置 Git: <https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-%E9%85%8D%E7%BD%AE-Git>

### 用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```shell
$ git config --global user.name "Joemuhuang"
$ git config --global user.email "Joemuhuang@tencent.com`
```

### 查看不同级别的配置文件

```shell
#查看系统config
git config --system --list
　　
#查看当前用户（global）配置
git config --global  --list
 
#查看当前仓库配置信息
git config --local  --list
```

### 检查配置信息

```shell
git config --list
```

### 生成 sshkey

```shell
ssh-keygen -t rsa -C "youxiang@tencent.com"
```

### 验证和修改

```shell
ssh -T git@github.com
```

### Window 换行符配置

```shell
# window换行符改为 LF
git config core.autocrlf input
```

