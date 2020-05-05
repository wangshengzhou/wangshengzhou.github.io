---
title: node-learn03
tags:
  - nodejs系列
categories:
  - Nodejs
description: nodejs学习
date: 2020-05-03 18:11:39
---

**前言：**
现在抽点时间来学习nodejs，发现菜鸟教程讲解通俗易懂方便新手入门，所以决定每读一篇都会动手实践下，期间会把重要的步骤和操作过程记下来，毕竟只看不练，很快就会忘记且不便于理解
[菜鸟教程](https://www.runoob.com/nodejs/nodejs-repl.html)

**`Node.js` `REPL`(交互式解释器)**
`Node.js` `REPL`(`Read Eval Print Loop`:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

Node 自带了交互式解释器，可以执行以下任务：
- **读取** - 读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
- **执行** - 执行输入的数据结构
- **打印** - 输出结果
- **循环** - 循环操作以上步骤直到用户两次按下 **ctrl-c** 按钮退出

<!--more-->

Node 的交互式解释器可以很好的调试 Javascript 代码。

首先，启动终端：

```
$ node
Welcome to Node.js v12.16.3.
Type ".help" for more information.
>
```

这时我们就可以在 > 后输入简单的表达式，并按下回车键来计算结果。

**使用变量:**

变量声明需要使用 **var** 关键字，如果没有使用 var 关键字变量会直接打印出来。

使用 **var** 关键字的变量可以使用 `console.log()` 来输出变量。

```
$ node
Welcome to Node.js v12.16.3.
Type ".help" for more information.
> a=1
1
> var b=5
undefined
> a+b
6
> console.log(a+b)
6
undefined
>
```

**多行表达式：**

`Node REPL` 支持输入多行表达式，这就有点类似 JavaScript。接下来让我们来执行一个 do-while 循环：

```
$ node
Welcome to Node.js v12.16.3.
Type ".help" for more information.
> var i=0
undefined
> do{ i++;
... console.log("i="+i);
... } while(i<5);
i=1
i=2
i=3
i=4
i=5
undefined
>
```

**...** 三个点的符号是系统自动生成的，回车换行后即可。Node 会自动检测是否为连续的表达式。

**下划线(_)变量**

可以使用下划线(_)获取上一个表达式的运算结果：

```
$ node
Welcome to Node.js v12.16.3.
Type ".help" for more information.
> var a=0,b=10,c=20
undefined
> a+b+c
30
> var sum=_
undefined
> console.log(sum)
30
undefined
>
```

## `REPL` 命令

- **ctrl + c** - 退出当前终端。
- **ctrl + c 按下两次** - 退出 Node REPL。
- **ctrl + d** - 退出 Node REPL.
- **向上/向下 键** - 查看输入的历史命令
- **tab 键** - 列出当前命令
- **.help** - 列出使用命令
- **.break** - 退出多行表达式
- **.clear** - 退出多行表达式
- **.save filename** - 保存当前的 Node REPL 会话到指定文件
- **.load filename** - 载入当前 Node REPL 会话的文件内容。
- **.exit** 退出当前终端




