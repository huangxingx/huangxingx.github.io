---
title: golang helloworld docker 启动
date: 2021-09-08 17:15:43
tags:
- golang
- docker
- docker-compose
- helloworld
categories:
- golang

auto_excerpt:
  enable: true
  length: 150
---

**golang的helloworld程序，利用docker-compose启动**

创建项目目录并初始化go mod 项目

```shell
mkdir hellogo
cd hellogo
# 初始化 go mod 项目
go mod init hellogo
```

项目结构

```shell
➜  hellogo tree 
.
├── docker-compose.yaml
├── dockerfile
├── go.mod
└── main.go
```

`vim main.go`

```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("hello world")
	i := 1
	for {
		time.Sleep(1 * time.Second)
		fmt.Printf("wait %d second\n", i)
		i++
	}
}

```

`vim dockerfile`

```dockerfile
# build stage
FROM golang:alpine AS builder
RUN apk add --no-cache git gcc libc-dev

RUN mkdir /app
COPY go.mod /app/go.mod
COPY main.go /app/main.go
WORKDIR /app
RUN go mod tidy
RUN go build -o hellogo main.go

# final stage
FROM alpine:latest
LABEL Name=hellogo Version=0.0.1
RUN apk --no-cache add ca-certificates
COPY --from=builder /app/hellogo /app
ENTRYPOINT ./app
# EXPOSE 80

```

`vim docker-compose.yaml`

```docker-compose
version: "3"
services:
  hellogo:
    build: .
    image: hellogo
    network_mode: bridge
    container_name: hellogo

```

**运行 docker-compose**

```shell
docker-compose -f docker-compose.yaml up --build
```

看到如下输出表示成功

```shell
...
Successfully built 50cb8339209e
Successfully tagged hellogo:latest
Starting hellogo ... done
Attaching to hellogo
hellogo    | hello world
hellogo    | wait 1 second
hellogo    | wait 2 second
hellogo    | wait 3 second
```

