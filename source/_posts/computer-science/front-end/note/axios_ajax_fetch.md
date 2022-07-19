---
title: 'axios, ajax和fetch的区别'
date: 2022-04-08 20:08:25
categories:
 - [计算机科学,前端技术]
tags: 
 - 前端技术
 - 软件技术
 - javascript
---

# axios、ajax和fetch的比较

## Ajax

前端程序员常说的Ajax是`Asynchronous JavaScript and XML`的缩写，即异步网络请求，区别于传统web开发中的同步方式。

Ajax带来的最大影响是页面可以无刷新的请求数据。以往，页面表单提交数据，在用户点击完"submit"按钮后，页面会强制刷新一下，
传统web请求方式:

### 实现一个Ajax请求

在浏览器上实现一个Ajax请求非常容易
```javascript ajax请求
var request = new XMLHttpRequest();

//设置回调函数
request.onreadystatechange = function(){
    //状态发生变化时，函数被回调
    if(request.readyState == 4){
        //判断响应状态码
        if(request.status === 200){
            //成功, 通过responseText拿到响应文本。
            return success(request.responseText);
        }else{
            //失败, 根据响应码判断失败原因
            return fail(request.status);
        }
    }else {
        //HTTP继续请求
    }
}

//发送请求
request.open('GET', '/api/categories');
request.setRequestHeader("Content-Type", "application/json") //设置请求头
request.send(); //请求正式发出
```
## axios

axios是一个基于Promise用于浏览器和nodejs的HTTP客户端，本质上也是对原生XHR的封装，只不过它是Promise的实现版本，符合最新的ES规范，有以下特点:
- 从浏览器中创建XMLHttpRequests
- 从node.js创建http请求
- 支持Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRF

### 浏览器支持

axios面向现代浏览器设计，设计简洁、API简单，支持浏览器和node，能很好的与各种前端框架整合。

## fetch

fetch是前端发展的一种新技术产物。

Fetch API提供了一个JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。还提供了一个全局fetch()方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

这种功能以前使用XMLHttpRequest实现，Fetch提供了一个更好的替代方法，可以很容易地被其他技术使用，例如Service Workers。Fetch还提供了单个逻辑位置来定义其他HTTP相关概念，例如CORS和HTTP扩展。

在使用fetch的时候需要注意:
- 当接收到一个代表错误的HTTP状态码时，从fetch()返回的Promise不会被标记为reject，即使该HTTP响应的状态码是404或500.相反，他会将Promise状态标记为resolve(但是会将resolve的返回值的ok属性设置为false)，仅当网络故障时或请求被阻止时，才会被标记为reject。
- 默认情况下，fetch不会从服务端发送或接收任何cookies，如果站点依赖于用户session，则会导致未经认证的请求(要发送cookies，必须设置credentials)。
```javascript 使用fetch获取数据
fetch('https://example.com/movies.json')
    .then(function(response){
        return response.json();
    })
    .then(function(myJson){
        console.log(myJson);
    })
```

