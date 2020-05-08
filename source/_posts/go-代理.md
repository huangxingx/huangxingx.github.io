---
title: go-proxy
date: 2020-05-08 11:30:57
tags: 
- go
categories:
- go
---

# golang http 代理

```golang

package main

import (
    "github.com/elazarl/goproxy"
    "log"
    "net/http"
)

func main() {
    proxy := goproxy.NewProxyHttpServer()
    proxy.Verbose = true
    log.Fatal(http.ListenAndServe(":9999", proxy))
}


```
