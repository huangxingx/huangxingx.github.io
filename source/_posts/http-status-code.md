---
title: HTTP Status Code
date: 2018-04-18 17:21:05
updated: 2018-04-18 17:21:05
tags:
- http
- web
categories:
- http


auto_excerpt:
  enable: true
  length: 150
---


当用户试图通过HTTP或FTP协议访问一台运行主机上的内容时，Web服务器返回一个表示该请求的状态的数字代码。该状态代码记录在服务器日志中，同时也可能在 Web 浏览器或 FTP客户端显示。也就是我们打开页面发生错误时浏览器显示的错误信息代码。状态代码可以指明具体请求是否已成功，还可以揭示请求失败的确切原因。
HTTP协议状态码表示的意思主要分为五类 ,大体是 :

```
1×× 　　保留
2×× 　　表示请求成功地接收
3×× 　　为完成请求客户需进一步细化请求
4×× 　　客户错误
5×× 　　服务器错误
```
<!--more-->

### 100 Continue
指示客户端应该继续请求。回送用于通知客户端此次请求已经收到，并且没有被服务器拒绝。
客户端应该继续发送剩下的请求数据或者请求已经完成，或者忽略回送数据。服务器必须发送
最后的回送在请求之后。

### 101 Switching Protocols 
服务器依照客服端请求，通过Upgrade头信息，改变当前连接的应用协议。服务器将根据Upgrade头立刻改变协议
在101回送以空行结束的时候。

# Successful 

### 200 OK
指示客服端的请求已经成功收到，解析，接受。
### 201 Created 
请求已经完成并一个新的返回资源被创建。被创建的资源可能是一个URI资源，通常URI资源在Location头指定。回送应该包含一个实体数据
并且包含资源特性以及location通过用户或者用户代理来选择合适的方法。实体数据格式通过煤体类型来指定即content-type头。最开始服务 器
必须创建指定的资源在返回201状态码之前。如果行为没有被立刻执行，服务器应该返回202。
### 202 Accepted 
请求已经被接受用来处理。但是处理并没有完成。请求可能或者根本没有遵照执行，因为处理实际执行过程中可能被拒绝。
### 203 Non-Authoritative Information
不是权威性信息。
### 204 No Content 
服务器已经接受请求并且没必要返回实体数据，可能需要返回更新信息。回送可能包含新的或更新信息由entity-headers呈现。
### 205 Reset Content 
服务器已经接受请求并且用户代理应该重新设置文档视图。
### 206 Partial Content 
服务器已经接受请求GET请求资源的部分。请求必须包含一个Range头信息以指示获取范围可能必须包含If-Range头信息以成立请求条件。
# Redirection 

### 300 Multiple Choices
请求资源符合任何一个呈现方式。
### 301 Moved Permanently 
请求的资源已经被赋予一个新的URI。
### 302 Found 
通过不同的URI请求资源的临时文件。
### 303 See Other
### 304 Not Modified 
如果客服端已经完成一个有条件的请求并且请求是允许的，但是这个文档并没有改变，服务器应该返回304状态码。304
状态码一定不能包含信息主体，从而通常通过一个头字段后的第一个空行结束。
### 305 Use Proxy
请求的资源必须通过代理（由Location字段指定）来访问。Location资源给出了代理的URI。
### 306 Unused
### 307 Temporary Redirect
临时重定向。
# Client Error

### 400 Bad Request
因为错误的语法导致服务器无法理解请求信息。
### 401 Unauthorized 
如果请求需要用户验证。回送应该包含一个WWW-Authenticate头字段用来指明请求资源的权限。
### 402 Payment Required 
保留状态码。
### 403 Forbidden 
服务器接受请求，但是被拒绝处理。
### 404 Not Found 
服务器已经找到任何匹配Request-URI的资源。
### 405 Menthod Not Allowed 
Request-Line 请求的方法不被允许通过指定的URI。
### 406 Not Acceptable
客户端浏览器不接受所请求页面的 MIME 类型。
### 407 Proxy Authentication Required
要求进行代理身份验证。
### 408 Reqeust Timeout 
客服端没有提交任何请求在服务器等待处理时间内。
### 409 Conflict
### 410 Gone
### 411 Length Required 
服务器拒绝接受请求在没有定义Content-Length字段的情况下。
### 412 Precondition Failed
前提条件失败。
### 413 Request Entity Too Large 
服务器拒绝处理请求因为请求数据超过服务器能够处理的范围。服务器可能关闭当前连接来阻止客服端继续请求。
### 414 Request-URI Too Long 
服务器拒绝服务当前请求因为URI的长度超过了服务器的解析范围。
### 415 Unsupported Media Type 
服务器拒绝服务当前请求因为请求数据格式并不被请求的资源支持。
### 416 Request Range Not Satisfialbe
所请求的范围无法满足。
### 417 Expectation Failed
执行失败。
# Server Error 

### 500 Internal Server Error
服务器遭遇异常阻止了当前请求的执行
### 501 Not Implemented 
服务器没有相应的执行动作来完成当前请求。
### 502 Bad Gateway
Web 服务器用作网关或代理服务器时收到了无效响应。
### 503 Service Unavailable 
因为临时文件超载导致服务器不能处理当前请求。
### 504 Gateway Timeout
网关访问超时。
### 505 Http Version Not Supported
HTTP 版本不受支持。

```
"100" : Continue
"101" : witching Protocols
"200" : OK
"201" : Created
"202" : Accepted
"203" : Non-Authoritative Information
"204" : No Content
"205" : Reset Content
"206" : Partial Content
"300" : Multiple Choices
"301" : Moved Permanently
"302" : Found
"303" : See Other
"304" : Not Modified
"305" : Use Proxy
"307" : Temporary Redirect
"400" : Bad Request
"401" : Unauthorized
"402" : Payment Required
"403" : Forbidden
"404" : Not Found
"405" : Method Not Allowed
"406" : Not Acceptable
"407" : Proxy Authentication Required
"408" : Request Time-out
"409" : Conflict
"410" : Gone
"411" : Length Required
"412" : Precondition Failed
"413" : Request Entity Too Large
"414" : Request-URI Too Large
"415" : Unsupported Media Type
"416" : Requested range not satisfiable
"417" : Expectation Failed
"500" : Internal Server Error
"501" : Not Implemented
"502" : Bad Gateway
"503" : Service Unavailable
"504" : Gateway Time-out
"505" : HTTP Version not supported
```
