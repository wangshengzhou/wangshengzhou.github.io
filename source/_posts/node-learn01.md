---
title: node-learn01
date: 2020-05-02 22:38:25
categories:
            - Nodejs
tags:
            - node.js-learn
---

Node.js 就是运行在服务端的 JavaScript。 

Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。

Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

<!--more-->


**脚本模式**：
第一个Node.js程序：Hello World！

例子：

```
console.log("Hello World");
```

保存改文件，文件名为helloworld.js,并通过 node命令来执行：

```
$ node helloworld.js
Hello World!
```

程序执行后，正常的话，就会在终端输出 Hello World。

**交互模式：**

```
$ node
Welcome to Node.js v12.16.3.
Type ".help" for more information.
> console.log("Hello World");
Hello World
```

