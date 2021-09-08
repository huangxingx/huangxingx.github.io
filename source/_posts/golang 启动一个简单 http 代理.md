---
title: golang 启动一个简单 http 代理
date: 2020-05-08 11:30:57
tags: 
- golang
- http

categories:
- golang
---

## golang 转发 http 请求


```golang
package main

import (
	"log"
	"net/http"
	"net/http/httputil"
	"net/url"
)

//将request转发给 http://127.0.0.1:2003
func proxyHandler(w http.ResponseWriter, r *http.Request) {

	trueServer := "http://127.0.0.1:15672"

	url, err := url.Parse(trueServer)
	if err != nil {
		log.Println(err)
		return
	}

	proxy := httputil.NewSingleHostReverseProxy(url)
	proxy.ServeHTTP(w, r)

}

func main() {
	http.HandleFunc("/", proxyHandler)
	log.Fatal(http.ListenAndServe(":2002", nil))
}

```
