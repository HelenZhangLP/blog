---
title: 由一个面试题引发的一系列事故
date: 2019-04-04 14:28:09
categories:
- 技术
tags:
- JavaScript
- 面试题
---

```JavaScript
console.log(0)
setTimeout(function(){console.log(1);})
let promise = new Promise(function(){console.log(2)})
promise.then(console.log(3))
setTimeout(function(){console.log(4)},0)
console.log(5)
```
### JavaScript 异步编程
JavaScript 是单线程，同步方式运行代码。会出现阻塞进程的问题。
#### JavaScript 异步编程的实现
主要分三类：回调函数、发布订阅、Promise 对象

** 回调函数 **
```JavaScript
for (var i=0; i<5; i++) {
  setTimeout(function(){
    console.log(i)
  })
}
// 5, 5, 5, 5, 5
```
```JavaScript
function a(fn) {
  setTimeout(function() {
      fn();
  }, 0);
  console.log('a');
}
function b(fn) {
  console.log('b');
}
a(b);
// a b function a 相当于异步执行
```

** PubSub Publish/Subscribe ** 发布、订阅模式，用以分发事件
```JavaScript
var PubSub = function() {
  this.handlers = {}
}
PubSub.prototype.subscribe = function(eventType, handler) {
  if (!(eventType in this.handlers)) {
    this.handlers[eventType] = []
  }
  this.handlers[eventType].push(handler); // 添加事件监听器
  return this; // 返回上下文环境，实现链式调用
}
PubSub.prototype.publish = function(eventType) {
  var _args = Array.prototype.slice.call(arguments, 1)
}
```
