---
title: node-learn02
tags:
  - nodejs系列
categories:
  - Nodejs
description: nodejs学习
date: 2020-05-03 15:00:25
---

**前言：**
现在抽点时间来学习nodejs，发现菜鸟教程讲解通俗易懂方便新手入门，所以决定每读一篇都会动手实践下，期间会把重要的步骤和操作过程记下来，毕竟只看不练，很快就会忘记且不便于理解
[菜鸟教程](https://www.runoob.com/nodejs/nodejs-http-server.html)

**`Node.js` 创建第一个应用**

该应用有以下几个部分组成：

1. **引入 required 模块：**我们可以使用 **require** 指令来载入 `Node.js` 模块。
2. **创建服务器：**服务器可以监听客户端的请求，类似于 `Apache` 、`Nginx` 等 HTTP 服务器。
3. **接收请求与响应请求** 服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

<!--more-->

**创建 `Node.js` 应用**

实例如下，在项目的根目录下创建一个叫 `server.js`的文件，并写入以下代码：

```nodejs
//step1 引入 required 模块
var http = require('http');
//step2 创建服务器
http.createServer(function(request,response){
  //发送 HTTP 头部
  //HTTP 状态值：200  OK
  //内容类型： text/plain
  response.writeHead(200,{'Content-Type':'text/plain'});

  //发送响应数据 “Hello World”
  response.end('Hello World\n');
}).listen(8080);

console.log('Server running at http://127.0.0.1:8080');

```

以上代码我们完成了一个可以工作的 HTTP 服务器。

使用 **node** 命令执行以上的代码：

```
$ node server.js
Server running at http://127.0.0.1:8080
```

接下来，打开浏览器访问 http://127.0.0.1:8080/，你会看到一个写着 "Hello World"的网页。

![](http://203.195.133.214/imgaes/nodejs/node-learn02-01.png)

**分析`Node.js` 的 HTTP 服务器：**

- 第一行请求（require）`Node.js` 自带的 `http` 模块，并且把它赋值给 `http` 变量。
- 接下来我们调用 `http` 模块提供的函数： `createServer` 。这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。
