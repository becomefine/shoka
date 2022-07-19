---
title: ASGI&WSGI介绍
date: 2022-03-09 16:57:09
categories:
 - [计算机科学,网络技术]
tags: 
 - 异步网关协议接口
 - 网络技术
 - fastApi
---
# ASGI&WSGI
[`转载`](https://www.jianshu.com/p/65807220b44a)

## 什么是WSGI
先说一下`CGI`, (通用网关接口, Common Gateway Interface/CGI), 定义客户端与Web服务器的交流方式的一个程序。例如正常情况下客户端发来一个请求，根据`HTTP`协议Web服务器将请求内容解析出来，经过计算后，再将us安出来的内容封装好，例如服务器返回一个`HTML`页面，并且根据HTTP协议构建返回内容的响应格式。涉及到`TCP`连接、`HTTP`原始请求和相应格式的这些，都由一个软件来完成，这时，以上的工作需要一个程序来完成，而这个程序就是`CGI`。

**WSGI**, 维基上的解释为, **Web服务器网关接口**(Python Web Server GateWay Interface, WSGI), 是为`Python`语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。从语义上理解，貌似`WSGI`就是`Python`为了解决Web服务器与客户端之间的通信问题而产生的，并且`WSGI`是基于现存的`CGI`标准而设计的，同样是一种程序(或者`Web`组件的接口规范)。
**WSGI**分为两部分: 一种为“服务器”或“网关”，另一种为“应用程序”或应用框架”。
所谓的`WSGI`中间件同时实现了`API`的两方, 即在`WSGI`服务器和`WSGI`应用之间起协调作用：从`WSGI`服务器的角度来说，中间件扮演应用程序，而从应用程序的角度来说，中间件扮演服务器。中间件具有的功能：

- 重写环境变量后，根据目标URL，将请求消息路由到不同的应用对象。
- 允许在一个进程中同时运行多个应用程序或应用框架。
- 负载均衡和远程处理，通过在网络上转发请求和相应消息。
- 进行内容后处理，例如应用`XSLT`样式表。

总的来说，`WSGI`就是基于`CGI`为标准做了一些扩展

## 什么是ASGI

异步网关协议接口，一个介于网络协议服务和`Python`应用之间的标准接口，能够处理多种通用的协议类型，包括`HTTP`, `HTTP2`和`WebSocket`。
然而目前的常用的`WSGI`主要针对`HTTP`风格的请求响应模型做的设计，并且越来越多的不遵循这种模式的协议逐渐成为`Web`变成的标准之一，例如`WebSocket`。
`ASGI`尝试保持在一个简单的应用接口的前提下，提供允许数据能够在任意的时候，被任意的应用进程发送和接受的抽象。并且同样描述了一个新的，兼容`HTTP`请求响应以及`WebSocket`数据帧的序列格式。允许这些协议能通过网络或本地`socket`进行传输，以及让不同的协议被分配到不同的进程中。

我们知道，WSGI已经存在很长时间了。只要用Python进行Web开发，其底层都是WSGI规范，不管是鼎鼎大名的Django、Flask还是一些小众的web框架。再搭配上uWSGI、gunicorn、mod_wsgi等WEB服务，用nginx做反向代理。这就是最典型的Python Web开发技术栈。它们 运行的很好，我们都习以为常了。那为什么又出现ASGI了呢？

在我们了解为什么之前，先来看看什么是ASGI吧。一句话，ASGI是WSGI的继任者。在 Web 2.0时代，WSGI完美地承载了我们的业务，随着移动网络的发展，Web技术也在升级，比如WebSocket、HTTP/2，HTTP/3。我们知道WSGI应用是一个单调用、同步接口，即输入一个请求，返回一个响应。这个模式无法支持长连接或者WebSocket这样的连接。即使我们想办法将应用改成异步，还有另一个限制：一个URL对应一个请求，而HTTP/2、Websocket等在一个URL里会出现多个请求。

在这些场景里，WSGI无法提供支持。而ASGI是异步应用。ASGI将请求相关的信息封装在 scope中，一个异步可等待的receive用来接收请求信息，一个异步可等待的send哦用来将相应发送出去。

因为是异步的关系，不再适合用请求和相应来描述双向通信的数据。根据ASGI的描述，这些叫事件event。这种模式使得一个应用可以接收和发送多个事件。由于已经是异步，应用还可以支持后台协程。

### ASGI工作原理

ASGI由单独的异步应用沟通构成，其中包括`scope`, 包含传入请求的所有信息; `send`, 用于向客户端发送事件的异步方法，`receive`，用于接受客户端发来事件的异步方法。
ASGI不仅让应用可以多次接受或发送事件，并且可以结合协程时应用同时处理其它(比如监听外部触发的事件，就像Redis队列一样)。
一个最简单的应用就像函数一样:
```python
async def appliction(scope, receive, send):
    event = await receive()
    ...
    await send({"type": "websocket.send", ...})
```

每一个事件都是按预定的格式编排的python字典，这种格式构成了通信规范的基础，并时的应用可以在服务器之间交换信息。
每个事件都有一个`type`属性，用于表明事件的结构。比如从`receive`方法可能收到如下结构的事件，用于发送HTTP响应的开头：
```python
{
    "type": "http.response.start",
    "status": 200,
    "headers": [(b"X-Header", b"Amazing Value")],
}
```
也可能将如下结构的事件传递给`send`方法用于发送WebSocket信息：
```python
{
    "type": "websocket.send",
    "text": "Hello world!",
}
```
## WSGI和ASGI的区别在哪

以上，`WSGI`是基于`HTTP`协议模式的，不支持`WebSocket`, 而`ASGI`的诞生则是为了解决Python常用的`WSGI`不支持当前Web开发中的一些新的协议标准。同时, `ASGI`和`WSGI`原有的模式的支持和`WebSocket`的扩展，即`ASGI`是`WSGI`的扩展。



