---
title: go-micro 使用注意事项
date: 2019-11-05 18:30:57
tags:
- go
- golang
- go-micro
- 注意事项

categories:
- go

auto_excerpt:
  enable: true
  length: 150
---

## 1. micro@v1.14 之后使用 consul 作为注册中心问题
micro@v1.14 之后更换了默认的注册中心，把 consul 换成了 etcd ，如果需要使用 consul 作为注册中心，
需要重新编译 micro。

1. clone 源码：

```shell
git clone https://github.com/micro/micro.git 
```

2. 切到源码目录并新增 plugins.go :

``` cd micro源码目录 vi plugins.go``` 

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

具体文档请查看 [micro@v1.14之后用户 consul作为注册中心解决方案](https://github.com/micro-in-cn/tutorials/tree/master/examples/basic-practices/micro-registry/etcdv3)

## 2. micro 用 Gin 作为 api 问题
用 gin 做为 gin-api 服务时， 通过 micro new --type web 创建一个服务出来，注意修改服务的 name，改为 com.example.api.ServiceName,
且在 gin router 中创建：

```golang
router := gin.Default()
r := router.Group("/ServiceName")

```
启动 ```micro api --namespace=com.example.api``` 这里必须要加 api 不然转发不了。
