---
title: AMQP介绍
date: 2018-07-16 17:21:05
tags:
- http
- web
categories:
- http web

auto_excerpt:
  enable: true
  length: 150
---

### AMQP介绍
AMQP，即Advanced Message Queuing Protocol，高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。


AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。


AMQP在消息提供者和客户端的行为进行了强制规定，使得不同卖商之间真正实现了互操作能力。


JMS是早期消息中间件进行标准化的一个尝试，它仅仅是在API级进行了规范，离创建互操作能力还差很远。


与JMS不同，AMQP是一个Wire级的协议，它描述了在网络上传输的数据的格式，以字节为流。因此任何遵守此数据格式的工具，其创建和解释消息，都能与其他兼容工具进行互操作。
<!--more-->

AMQP规范的版本：
0-8        是2006年6月发布
0-9        于2006年12月发布
0-9-1     于2008年11月发布
0-10      于2009年下半年发布
1.0 draft  （文档还是草案）


AMQP的实现有：


1）OpenAMQ
AMQP的开源实现，用C语言编写，运行于Linux、AIX、Solaris、Windows、OpenVMS。


2）Apache Qpid
Apache的开源项目，支持C++、Ruby、Java、JMS、Python和.NET。


3）Redhat Enterprise MRG
实现了AMQP的最新版本0-10，提供了丰富的特征集，比如完全管理、联合、Active-Active集群，有Web控制台，还有许多企业级特征，客户端支持C++、Ruby、Java、JMS、Python和.NET。


4）RabbitMQ
一个独立的开源实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、 ActionScript、XMPP、STOMP等，支持AJAX。RabbitMQ发布在Ubuntu、FreeBSD平台。


5）AMQP Infrastructure
Linux下，包括Broker、管理工具、Agent和客户端。


6）?MQ
一个高性能的消息平台，在分布式消息网络可作为兼容AMQP的Broker节点，绑定了多种语言，包括Python、C、C++、Lisp、Ruby等。


7）Zyre
是一个Broker，实现了RestMS协议和AMQP协议，提供了RESTful HTTP访问网络AMQP的能力。

 

RabbitMQ 是一个由 Erlang 写成的 Advanced Message Queuing Protocol (AMQP) 实现，AMQP 的出现其实也是应了广大人民群众的需求，虽然在同步消息通讯的世界里有很多公开标准（如 COBAR 的 IIOP ，或者是 SOAP等），但是在异步消息处理中却不是这样，只有大企业有一些商业实现（如微软的 MSMQ ，IBM 的 Websphere MQ 等），因此，在 2006 年的 6 月，Cisco 、Redhat、iMatix 等联合制定了 AMQP 的公开标准。
