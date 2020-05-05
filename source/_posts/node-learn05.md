---
title: node-learn05
tags:
  - nodejs系列
categories:
  - Nodejs
description: nodejs学习
date: 2020-05-04 12:29:06
---


**前言：**
现在抽点时间来学习nodejs，发现菜鸟教程讲解通俗易懂方便新手入门，所以决定每读一篇都会动手实践下，期间会把重要的步骤和操作过程记下来，毕竟只看不练，很快就会忘记且不便于理解
[菜鸟教程](https://www.runoob.com/nodejs/nodejs-event-loop.html)

**Node.js 事件循环**

Node.js 是单进程单线程应用程序，但是因为 [`V8`](https://www.jianshu.com/p/81f6ded64ab2) 引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发，所以性能非常高。

Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。

Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.

**事件驱动程序**
<!--more-->
Node.js 使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。

当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。

这个模型非常高效可扩展性非常强，因为 webserver 一直接受请求而不等待任何读写操作。（这也称之为非阻塞式IO或者事件驱动IO）

在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。

![event_loop](https://www.runoob.com/wp-content/uploads/2015/09/event_loop.jpg)

整个事件驱动的流程就是这么实现的，非常简洁。有点类似于观察者模式，事件相当于一个主题(Subject)，而所有注册到这个事件上的处理函数相当于观察者(Observer)。

Node.js 有多个内置的事件，可以通过引入 events 模块，并通过实例化 EventEmitter 类来绑定和监听事件，如下实例：

```javascript
// 1.引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();

// 2.绑定事件及事件的处理程序
eventEmitter.on('eventName', eventHandler);

// 3.触发事件
eventEmitter.emit('eventName');
```

**实例**

创建 `event-demo01.js` 文件，代码如下所示：

```javascript
// 1.引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();

// 2.创建事件处理程序
var connectHandler = function connected(){
  console.log('连接成功');
  // 触发 data_received 事件
  eventEmitter.emit('data_received');
}

// 3.绑定 connection 事件处理程序
eventEmitter.on('connection',connectHandler);

// 4.使用匿名函数绑定 data_received 事件
eventEmitter.on('data_received',function(){
  console.log('数据接收成功');
});

// 5.触发 connection 事件
eventEmitter.emit('connection');

console.log('程序执行完毕.');
```

接下来让我们执行以上代码：

```
$ node event-demo01.js
连接成功
数据接收成功
程序执行完毕.
```
