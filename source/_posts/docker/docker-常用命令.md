---
title: docker 常用命令
tags:
  - docker
categories:
  - docker
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
---

## docker 常用命令

> 镜像导入导出

> 导出

```shell
docker save {imageid} > filename.tar imagesname
```
举例
```shell
docker save 4e8c7ea9f913 > pulsar-manager_0.2.0.tar apachepulsar/pulsar-manager
```

> 导入

``` shell
docker load [OPTIONS]
```
OPTIONS 说明：

--input , -i : 指定导入的文件，代替 STDIN。
--quiet , -q : 精简输出信息。

> 批量删除未使用的容器

```shell
# 方法一
docker rm `docker ps -a|grep Exited|awk '{print $1}'`
# 方法二
docker container prune
```

> 删除所有镜像

```shell
sudo docker rmi $(docker images -q)
```

> 清理镜像

```shell
docker container prune
docker system prune
docker volume prune
```

> 设置 Docker 容器日志文件

```json
{
	"log-driver": "json-file",
	"log-opts": {
		"max-size":"100m",
		"max-file": "5"
	}
}
```

