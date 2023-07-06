---
title: consul 常用命令
tags:
  - consul
categories:
  - consul
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
---

## consul 常用命令

#### 查询

```shell
# 通过健康检查的服务
curl http://localhost:8500/v1/health/service/:service_id?passing
# token
curl --header "X-Consul-Token: {token}"  http://127.0.0.1:8500/v1/catalog/services

curl http://localhost:8500/v1/catalog/service/:service_id
```

#### 注册

```shell
# 注销
curl -XPUT http://localhost:8500/v1/agent/service/deregister/:service_id	

curl -XPUT http://localhost:18500/v1/agent/service/deregister/enterprise	
```

