---
title: ssh
tags:
  - ssh
  - linux
categories:
  - linux
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
---

## ssh

### 生成公钥

```shell
ssh-keygen -t rsa
```

\-t 指定算法
\-f 指定生成秘钥路径
\-N 指定密码

### 拷贝公钥

```shell
cd ~/.ssh
scp id_rsa.pub root@B:/root/.ssh/authorized_keys #此命令在A机器执行，目的将公钥发送至B机器
scp id_rsa.pub root@A:/root/.ssh/authorized_keys #此命令在B机器执行，目的将公钥发送至B机器
```
将你的SSH公钥复制到远程主机，开启无密码登录 – 简单的方法
```shell
ssh-copy-id username@hostname
```

如果发现设置免密登陆，还需要输入密码，那么检查一下/root .ssh authorized\_keys目录和文件的权限。

```shell
chmod 600 authorized_keys 
chmod 700 .ssh
```

如果authorized\_keys文件、𝐻𝑂𝑀𝐸/.ssh 目录或HOME目录让本用户之外的用户有写权限，那么 sshd 都会拒绝使用 \~/.ssh/authorized\_keys 文件中的key来进行认证的。