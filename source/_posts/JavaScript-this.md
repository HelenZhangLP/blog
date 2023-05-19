---
title: JavaScript this
date: 2018-07-11 13:08:57
tags:
- JavaScript
---

## 函数与 `this`
```javascript
function public() {
  return `${this.name}'s age is ${this.age}` 
}

let person1 = {
  name: 'helen',
  age: 12,
  public: public
}

let person2 = {
  name: 'audery',
  age: 4,
  public: public
}

console.log(person1.public())
console.log(person2.public())
```
从代码的角度看，`this` 代表的是一个对象，具体代表的是执行调用 `public` 的当前对象，this 英文中的意思是`这个`，这个就是调用`public`方法的对象

**[可以使用'.'运算符、 `call,apply,bind` 来决定 `this` 的参考对象](https://helenzhanglp.github.io/2019/03/18/Javascript-%E5%87%BD%E6%95%B0-bind-call-apply/)**

## 全局对象 与 `this`
ES6 之前 JavaScript 并没有名称管理机制，如 Number/Math 等都是作为全局对象的特性存在。
```javascript
// window 下的 Math
window.Math
// node 下取 Math
global.Math
```
> 调用函数时，无法通过 '.' 运算符，call/apply 等方式，指定 this，严格模式下，this 为 undefined。非严格模式下，全局作用域下调用 this 代表 window 
```javascript
'use strict'; 
function testThis() {console.log(this)} 
testThis() // undefined
```
函数是 Function 构造函数的实例，如下 demo 可以创建 Function 实例取全局对象
```javascript
Function('return this')()
/**
 * node 环境的返回值
 Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  ...
  */
/**
 * 浏览器 下的返回值
Window{window: Window, self: Window, document: document, name: "", location: Location,…}*/
```

## 箭头函数与 `this`
箭头函数可以看成是函数字面量的语法糖。<font color="#f33">箭头函数在 this 方面的解析与函数字面量不同</font>
```javascript
this.x = 1
var fn = function () {
  console.log(this.x)
}
var fnArrow = () => {
  console.log(this.x)
}
var obj = {
  x: 2,
  fn: fn,
  fnArrow: fnArrow
}
console.log(obj.fn()) // 2
console.log(obj.fnArrow()) // 1
```
> 字面量函数的 this，由调用者决定。<font color="#f33">箭头函数的 this 由词法环境决定，即箭头函数在哪个环境中定义的，则 this 代表哪个环境。this 具有继承性。使用 `obj.fnArrow.call(obj)`也无法改变 this。箭头函数中 this 一旦绑定，就无法改变。</font>

```javascript
this.x = 1
var _this = this

// ES6 之前，使用变量继承 this
var fn = function () {
  console.log(_this.x)
}

// ES6 之后使用箭头函数继承 this
var fnArrow = () => {
  console.log(this.x)
}

var obj = {
  x: 2,
  fn: fn,
  fnArrow: fnArrow
}
console.log(obj.fn()) // 1
console.log(obj.fnArrow()) // 1
```

