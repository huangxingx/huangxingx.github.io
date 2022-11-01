---
title: mac zsh 终端
tags:
  - mac
categories:
  - mac
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
typora-root-url: ../..
---

## mac zsh 终端

### 利用iTerm2+oh-my-zsh+Dracula主题打造我的Mac终端利器

参考文章：https://blog.csdn.net/daiyuhe/article/details/88667875
效果图

![image](/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RhaXl1aGU=,size_16,color_FFFFFF,t_70.png)

### item2
官网：https://www.iterm2.com/index.html


### oh-my-zsh配置

#### 安装oh-my-zsh
```shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### 在oh-my-zsh中启用插件
```
plugins=(
git 
zsh-autosuggestions 
zsh-syntax-highlighting
mvn
)
```