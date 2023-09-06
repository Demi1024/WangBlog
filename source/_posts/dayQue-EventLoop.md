---
title: 【每日一题】Event Loop
date: 2020-07-24 21:52:48
tags: 
  - JavaScript
  - Node.JS
categories:
  - 面试题
---

## 为什么有事件循环

因为JavaScript是一个单线程执行的语言，为了主线程不被阻塞

## 微任务与宏任务

什么是宏任务？我们把setTimeOut\setInterval\Promise主线程当作宏任务来处理

什么是微任务？promise的.then()与.catch()是微任务

## 执行顺序

先执行同步任务，遇到宏任务丢到宏任务的队列，遇到微任务丢到微任务的队列。等所有的同步都执行完成后，开始执行宏任务，其次是微任务

## 小测验

请看下面的代码，说出输出的顺序

```js
setTimeout(() => {
  console.log(1)
  Promise.resolve(3).then(data => console.log(data))
}, 0)

setTimeout(() => { console.log(2) }, 0)
```
