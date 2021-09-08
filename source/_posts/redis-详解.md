---
title: redis 详解
auto_excerpt:
  enable: true
  length: 150
date: 2021-09-08 15:22:26
tags:
categories:
---



[toc]

### redis 介绍

官网地址：[https://redis.io/](https://redis.io/)

Redis的全称是 **RE**mote **DI**ctionary **S**erver，是一个高效的内存键值数据库，常被用来做分布式的高速缓存，相比较我们常规使用的Mysql、MongoDB等数据库，Redis的最大特点在于数据读写全部在内存中进行，进而带来极大的效率优势。

### redis 使用场景

1. 作为缓存服务器（String, Hash）
2. 作为消息队列（List）
3. 数据排行榜（Sorted Set）
4. 热点数据
5. 计数器； 比如统计点击率、点赞率，Redis具有原子性，可以避免并发问题。

以前只列出简单且常见的用途

### redis 数据类型

**String、List、Hash、Set、Sorted Set** 五种数据类型

**String**
String和我们常规理解的字符串基本一致，主要存储序列化后的字符串，支持写入原生字符串也支持写入数字类型。

**List**
List即为列表，List在Redis底层采用的是**双向链表**实现的，所以我们会发现Redis的List操作命令有左右之分，比如LPUSH、RPUSH，实际上就是双端列表左右两端的存取。

**Hash**
Hash可以理解为我们常规使用的字典数据结构，Redis采用**散列表**来实现Hash， 一个Hash结构里面可以存在很多的key和value，Hash是Redis比较推荐使用的一种数据结构

**Set**
Set是集合，满足集合确定性、无序性、唯一性三个性质，可以用来进行元素的去重操作

**Sorted Set**
Sorted Set是有序集合，满足集合唯一性的要求，同时也满足有序的性质。向Sorted Set中插入元素的时候需要同时指定一个Score，用于作为排序的标准， Sorted Set的底层实现采用的是**Skip List**

### redis 命令

---

参考：

1. [Redis详解（1）——为什么我们都需要了解Redis](https://zhuanlan.zhihu.com/p/94680529)

---
