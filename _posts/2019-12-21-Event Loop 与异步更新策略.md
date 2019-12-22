--- 
layout:     post
title:      Event Loop 与异步更新策略
subtitle:   从Event Loop中着手性能优化
date:       2019-12-22
author:     Ruiyoung
header-img: img/post-bg-debug.png
catalog: true
tags:
    - 前端性能优化
    - 浏览器
---

> Micro-Task 与 Macro-Task
>> 事件循环中的异步队列有两种：macro（宏任务）队列和 micro（微任务）队列。  
>> 常见的 macro-task 比如： setTimeout、setInterval、 setImmediate、script（整体代码）、 I/O 操作、UI 渲染等。  
常见的 micro-task 比如: process.nextTick、Promise、MutationObserver 等。

### 每一次循环都是一个这样的过程：
将一个macro-task执行并出队 =》将一队micro-task执行并出队 =》 执行渲染操作，更新界面=》处理worker相关的任务

### 渲染的时机
我们更新 DOM 的时间点，应该尽可能靠近渲染的时机。当我们需要在异步任务中实现 DOM 修改时，把它包装成 micro 任务是相对明智的选择。

### 异步更新策略
当我们使用 Vue 或 React 提供的接口去更新数据时，这个更新并不会立即生效，而是会被推入到一个队列里。待到适当的时机，队列中的更新任务会被批量触发。这就是异步更新。