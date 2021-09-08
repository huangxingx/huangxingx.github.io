---
title: go-micro 使用问题记录
date: 2019-11-05 18:30:57
tags:
- golang
- go-micro
- 微服务
- 注意事项

categories:
- golang
- go-micro

auto_excerpt:
  enable: true
  length: 150
---

### 1. micro@v1.14 之后使用 consul 作为注册中心问题
micro@v1.14 之后更换了默认的注册中心，把 consul 换成了 etcd ，如果需要使用 consul 作为注册中心，
需要重新编译 micro。

1. clone 源码：

```shell
git clone https://github.com/micro/micro.git 
```

2. 切到源码目录并新增 plugins.go :

```shell
cd micro源码目录 vi plugins.go
```

```golang
package main

import (
	_ "github.com/micro/go-plugins/registry/consul"
)
```

3. 编译

```shell
go build -o mainWithConsul main.go plugins.go 
```

4. 运行

```shell
 ./mainWithConsul --registry=consul api 
```

### 2. micro 用 gin 作为 api 问题
用 gin 做为 gin-api 服务时， 通过 *micro new --type web* 创建一个服务出来，注意修改服务的 name，改为 *com.example.api.ServiceName*,
且在 gin router 中创建：

```golang
router := gin.Default()
r := router.Group("/ServiceName")

```
```shell
micro api --namespace=com.example.api
```

启动 这里必须要加 api 不然转发不了

### 3. 使用 micro new 生成模板

使用 micro new 生成模板， RegisterSubscriber 的 topic 名称和 service name 一样，导致大概50%的概率出现请求失败问题.

**解决方法:**  注释 RegisterSubscriber 部分代码，或者换一个 topic 名称。
