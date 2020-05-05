---
title: node-learn06
tags:
  - nodejs系列
categories:
  - Nodejs
description: nodejs学习
date: 2020-05-04 17:34:41
---


**前言：**
现在抽点时间来学习nodejs，发现菜鸟教程讲解通俗易懂方便新手入门，所以决定每读一篇都会动手实践下，期间会把重要的步骤和操作过程记下来，毕竟只看不练，很快就会忘记且不便于理解
[菜鸟教程](https://www.runoob.com/nodejs/nodejs-event.html)

**Node.js EventEmitter**

Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。

Node.js 里面的许多对象都会分发事件：一个 net.Server 对象会在每次有新连接时触发一个事件， 一个 fs.readStream 对象会在文件被打开的时候触发一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。

**EventEmitter 类**
<!--more-->
events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。

EventEmitter 对象如果在实例化时发生错误，会触发 error 事件。当添加新的监听器时，newListener 事件会触发，当监听器被移除时，removeListener 事件被触发。

下面我们用一个简单的例子(`event-demo02.js`)说明 EventEmitter 的用法：

```javascript
var EventEmitter = require('events').EventEmitter;
var event = new EventEmitter();
event.on('some_event',function(){
     console.log('some_event 事件触发');
 });

 setTimeout(function(){
   event.emit('some_event');
   },1000);
```

执行结果如下：

```javascript
$ node event-demo02.js
some_event 事件触发
```

运行这段代码，1 秒后控制台输出了 **`some_event 事件触发`**。其原理是 event 对象注册了事件 some_event 的一个监听器，然后我们通过 setTimeout 在 1000 毫秒以后向 event 对象发送事件 some_event，此时会调用some_event 的监听器。

EventEmitter 的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。对于每个事件，EventEmitter 支持 若干个事件监听器。

当事件触发时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。

下面的例子(`event-demo03.js`)解释这个过程：

```javascript
var events = require('events');
var emitter = new events.EventEmitter();

emitter.on('someEvent',function(arg1,arg2){
  console.log('listener1',arg1,arg2);
 });

emitter.on('someEvent',function(arg1,arg2){
  console.log('listener2',arg1,arg2);
  });

 emitter.emit('someEvent','arg1 参数','arg2 参数');
```

执行以上代码，运行的结果如下：

```javascript
$ node event-demo03.js
listener1 arg1 参数 arg2 参数
listener2 arg1 参数 arg2 参数
```

上面例子中，emitter 为事件 someEvent 注册了两个事件监听器，然后触发了 someEvent 事件。

运行结果中可以看到两个事件监听器回调函数被先后调用。 

EventEmitter 提供了多个属性，如 **on** 和 **emit**。**on** 函数用于绑定事件函数，**emit** 属性用于触发一个事件。接下来我们来具体看下 EventEmitter 的属性介绍。

| 方法                                   | 描述                                                         | 示例                                                         |
| -------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **addListener(event, listener)**       | 为指定事件添加一个监听器到监听器数组的尾部。                 |                                                              |
| **on(event, listener)**                | 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。 | `server.on('connection', function (stream) {   console.log('someone connected!'); });` |
| **once(event, listener)**              | 为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。 | `server.once('connection', function (stream) {   console.log('Ah, we have our first user!'); });` |
| **removeListener(event, listener)**    | 移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。第一个是事件名称，第二个是回调函数名称。 | `var callback = function(stream) {   console.log('someone connected!'); }; server.on('connection', callback); // ... server.removeListener('connection', callback);` |
| **removeAllListeners([event])**        | 移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。 |                                                              |
| **listeners(event)**                   | 返回指定事件的监听器数组。                                   |                                                              |
| **emit(event, [arg1], [arg2], [...])** | 按监听器的顺序执行执行每个监听器，如果事件有注册监听返回 true，否则返回 false。 |                                                              |

| 类方法                            | 描述                       | 示例                                     |
| --------------------------------- | -------------------------- | ---------------------------------------- |
| **listenerCount(emitter, event)** | 返回指定事件的监听器数量。 | `eventEmitter.listenerCount(eventName) ` |

| 事件               | 描述                                                         | 示例 |
| ------------------ | ------------------------------------------------------------ | ---- |
| **newListener**    | **event** - 字符串，事件名称<br>**listener** - 处理事件函数<br>该事件在添加新监听器时被触发。 |      |
| **removeListener** | **event** - 字符串，事件名称 <br>**listener** - 处理事件函数<br>从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引。 |      |

**实例**

以下实例通过 connection（连接）事件演示了 EventEmitter 类的应用。

创建 event-demo04.js 文件，代码如下：

```javascript
var events = require('events');
var eventEmitter = new events.EventEmitter();

// 监听器 #1
var listener1 = function listener1(){
        console.log('监听器 listener1 执行。');
}

// 监听器 #2
var listener2 = function listener2(){
        console.log('监听器 listener2 执行。');
}

// 绑定 connection 事件，处理函数为 listener1
eventEmitter.addListener('connection',listener1);

// 绑定 connection 事件，处理函数为 listener2
eventEmitter.on('connection',listener2);

var eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + '个监听器监听连接事件。');


// 处理 connection 事件
eventEmitter.emit('connection');

// 移除绑定的 listener1 函数
eventEmitter.removeListener('connection',listener1);
console.log('listener1 不再受监听。');

eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");
// 处理 connection 事件
eventEmitter.emit('connection');

console.log("程序执行完毕。");
```

以上代码，执行结果如下所示：

```javascript
$ node event-demo04.js
2个监听器监听连接事件。
监听器 listener1 执行。
监听器 listener2 执行。
listener1 不再受监听。
1 个监听器监听连接事件。
监听器 listener2 执行。
程序执行完毕。
```

**error 事件**

EventEmitter 定义了一个特殊的事件 error，它包含了错误的语义，我们在遇到 异常的时候通常会触发 error 事件。

当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。

一般要为触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。例如：

```javascript
var events = require('events'); 
var emitter = new events.EventEmitter(); 
emitter.emit('error'); 
```

运行时会显示以下错误：

```javascript
$ node event-error-noHandle.js
events.js:301
    throw err; // Unhandled 'error' event
    ^

Error [ERR_UNHANDLED_ERROR]: Unhandled error. (undefined)
    at EventEmitter.emit (events.js:299:17)
    at Object.<anonymous> (G:\Projects\nodejs\event-error-noHandle.js:3:9)
    at Module._compile (internal/modules/cjs/loader.js:1133:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1153:10)[
39m
    at Module.load (internal/modules/cjs/loader.js:977:32)
    at Function.Module._load (internal/modules/cjs/loader.js:877:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.j
s:74:12)
    at internal/main/run_main_module.js:18:47 {
  code: 'ERR_UNHANDLED_ERROR',
  context: undefined
}
```

**继承 EventEmitter**

大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。

为什么要这样做呢？原因有两点：

1. 具有某个实体功能的对象实现事件符合语义， 事件的监听和发生应该是一个对象的方法。

2.  JavaScript 的对象机制是基于原型的，支持 部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系。

**继承方式：**

1. 原型继承

   ```javascript
   var eventEmitter  = require('events').EventEmitter
   
   function Client() {
     eventEmitter.call(this);
   }
   
   Client.prototype.see = function() {
       console.log('It is a see!')
    }
   
   Client.prototype.__proto__ = eventEmitter.prototype;
   
       var client = new Client();
   
       //接收事件
       client.on('see', client.see)
       //发送事件
       client.emit('see');
   
       console.log('程序执行结束');
   ```

   运行结果：

   ```javascript
   $ node event-prototype.js
   It is a see!
   程序执行结束
   ```

   

2. 类继承法

   ```javascript
   var EventEmitter = require('events').EventEmitter
   
   class Client extends EventEmitter {
     constructor() {
         super()
           }
   
     see() {
            console.log('extends：It is a see!');
             }
    }
   
   
      var client = new Client();
      //接收事件
      client.on('see', client.see);
      //发送事件
      client.emit('see');
   
      console.log('程序执行结束');
   ```

   运行结果：

   ```javascript
   $ node event-extend.js
   extends：It is a see!
   程序执行结束
   ```

   

3. 对象继承

   ```javascript
   var EventEmitter  = require('events').EventEmitter
   
   var client = {
     see() {
            console.log('object：It is a see!');
           }
   };
   
   client.__proto__ = EventEmitter.prototype;
   
        // Subscribe to event.
       client.on("see", client.see);
       // Emit event.
       client.emit("see");
   
       console.log('程序执行结束');
   ```

   运行结果：

   ```javascript
   $ node event-object.js
   object：It is a see!
   程序执行结束
   ```

