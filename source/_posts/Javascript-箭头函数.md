---
title: 箭头函数
date: 2019-03-18 13:40:26
categories:
- 技术
tags:
- JavaScript
- ES6
---

### 箭头函数
[Arrow Functions](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
> 没有自己的 this、arguments、super、new.target，它不能用作构造函数，更适用于那些本来需要匿名函数的地方，函数简短，并且不绑定 this

#### 从函数中认识 this
##### 构造函数
```JavaScript
function Construct() {
  this.prop = 1
  console.log(this.prop, this)
  setTimeout(function() {
    ++this.prop
    console.log(this.prop, this)
  },1)
}
var construct = new Construct()

// 1 Construct {prop: 1}prop: 1__proto__: Object
// NaN Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
```
