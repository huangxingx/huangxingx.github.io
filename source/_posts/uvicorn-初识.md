---
title: uvicorn 初识

date: 2021-09-09 17:10:23
tags:
- python
- web
categories:
- python

auto_excerpt:
  enable: true
  length: 150
---

## uvicorn简介

`uvicorn`是一个基于`asyncio`开发的一个轻量级高效的web服务器框架。

官方地址： [http://www.uvicorn.org/](http://www.uvicorn.org/)

`uvicorn` 设计的初衷是想要实现两个目标：

- 使用[`uvloop`](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FMagicStack%2Fuvloop)和[`httptools`](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FMagicStack%2Fhttptools)实现一个极速的`asyncio`服务器。
- 实现一个基于[`ASGI(异步服务器网关接口)`](https://link.juejin.cn?target=http%3A%2F%2Fchannels.readthedocs.io%2Fen%2Fstable%2Fasgi.html)的最小的应用程序接口。

它目前支持`http`，`websockets`，`Pub/Sub` 广播，并且可以扩展到其他协议和消息类型。

