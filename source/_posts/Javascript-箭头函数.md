---
title: JavaScript-箭头函数
date: 2019-03-18 13:40:26
categories:
- 技术
tags:
- JavaScript
- ES6
- Function
---

## 箭头函数兼容性
> [箭头函数兼容性](http://kangax.github.io/compat-table/es6/#test-arrow_functions)

## 箭头函数定义
lambda 函数以尽量简洁的语法定义函数，是函数表达式的简化版。

```javascript
param => expression // 只有一个参数，省略括号，操作符，胖箭头 => 箭头函数的核心。仅一行语句，即为返回值。省略 { return expression }
() => expression // 没有参数，必须使用 () 是必须的，其它同上
(param1, param2) => expression // 一个以上参数，参数由括号包裹，参数以逗号隔开，其它同上
() => {
	expression1;
	expression2;
} // {} 包裹的函数体代码块，没有 return 语句，默认返回 undefined，否则返回 return 语句的值
```

## 箭头函数与函数执行上下文
> 箭头函数没有单独的 `this`， <font color="#a33">***箭头函数的 `this` 与 函数声明所在的上下文相同***，</font>由以 demo 可知，因 箭头函数在 window 中定义，所以 `this` 指向 `window`，函数表达式 `this` 指向调用的环境，所以 `this` 指向 `lambda` 对象
```javascript
function thisLambda() {
let lambda = {
	// 在 window 中声明
	getThislambda: () => {
		// assets('assets', true, this)
		console.log('lambda', this)
	},
	
	getThis: function() {
		console.log('function expression',this)
	}
}

lambda.getThislambda() //Window {window: Window, self: Window, document: document, name: '', location: Location, …}
lambda.getThis() // {getThislambda: ƒ, getThis: ƒ}
}
```

<!-- 
### 箭头函数
[Arrow Functions](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
> 没有自己的 this、arguments、super、new.target，它不能用作构造函数，更适用于那些本来需要匿名函数的地方，函数简短，并且不绑定 this

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
``` -->
