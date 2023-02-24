---
title: JavaScript-函数
date: 2019-03-18 14:17:37
top: 1
tags:
- JavaScript
- Function
- 函数作用域
---

Javascript 是函数式编程，函数是第一类对象，是一等公民。Javascript 中的函数是可调用的对象。函数声明和函数表达式的不同在于调用时机不同

## 函数声明（function decalations） 
以关键字 function 开头，函数名，小括号参数名以逗号隔开，参数可选；大括号包含函数体，大括号必须。
> 函数声明是通过字面量(function literal)创建，必须有函数名的独立语句，用函数名作为引用方式，方便调用。
```javascript
function fn(arg1,arg2,...){}
```

## 函数构造器
通过字符串，构造出一个新的函数
```javascript
var fn = new Function('a', 'b', 'return a + b')
fn(1,2) // 3
```

## 箭头函数（lambda）
> [详见-Javascript-箭头函数](/2019/03/18/JavaScript-箭头函数)

## 构造函数
构造函数的目的是创建一个新对象，并进行初始化设置，然后作为构造函数的返回值。
可以通过构造函数，构造并初始化一个新对象
```javascript
function ConFn() {
	this.fn = function() {console.log(this)}
}

let conFnObj = new ConFn()
```
### new 一个构造函数会进行如下操作：
* 创建一个新空对象
* 该对象作为 `this` 参数传递给构造函数，并初化，且该对象成为函数上下文
* 新创建的对象作为 new 运算符的返回值
### 构造函数的返回值
* 构造函数显式返回非对象类型，构造函数忽略返回值，返回新创建的对象
```javascript
function Fn() {
	this.variable = 'variable'
	return 1
}

console.log(new Fn()) // Fn {variable: 'variable'} 返回调用构造函数生成的对象
```
* 构造函数显式返回对象类型，则对象作为构造函数实例的返回值，传入的构造函数 `this` 被丢弃
```javascript
function Fn() {
	this.getThis = function() {console.log(this)}
	return {}
}

let fnObj = new Fn() // fnObj {}
fnObj.getThis() // Uncaught TypeError: fnObj.getThis is not a function

```

## 生成器函数
> [详见-Javascript-生成器函数](/2023/02/14/JavaScript-生成器函数/)

## 函数使用场景
> 通过字面量创建
```javascript
function fn(arg1,arg2,...){}
```
> 赋值给变量，数组，其它对象属性
```javascript
var fn = function(){}
var arr = []
arr.push(function(){})
var obj = {}
obj.fn = function(){}
```

> 作为参数，回调
```javascript
function fn(callback) {
	callback()
}
fn(function(){})
```

> 作为函数返回值
```javascript
function fn() {
	return function(){}
}
```

> 动态创建和分配属性
```javascript
function fn() {}
fn.name = 'function'
```

## 回调函数
> 使用场景 —— 事件、服务器渲染、UI 动画

## 自执行函数
### <font color="#a33">Uncaught SyntaxError: Function statements require a function name (at (index):41:4)</font>
```javascript
function(){}(3)
```
需求：定义一个 自执行（立即执行函数 IIFE）, Javascript 引擎解析 uncaught SyntaxError。函数声明需要一个函数名。

Function Statements 函数声明 需要一个函数名，Javascript 引擎将 `function(){}(3)` 解析为一个声明。而立即执行函数为一个函数表达式，具体写法如下：
```javascript
(function(){})(3)
```
将函数表达式放入括号内，目的是告诉 Javascript 引擎，目前处理的是一个函数表达式。也可以用一元表达式，告诉浏览器引擎，当前语句为 Javascript 表达式
```javascript
!function(){}()
+function(){}()
-function(){}()
~function(){}()
```
注意使用一元表达式创建的立即执行函数并没有存在其它地方。

## 函数参数
> [详见 —— Javascript-函数参数](/2023/02/03/Javascript-函数参数)

## 函数上下文
> [详见 —— Javascript-函数上下文/](/2023/02/02/Javascript-函数上下文)

## 严格模式
> 严格模式是在 ES5 中引入的特性。它可以改变 Javascript 引擎的默认行为并执行更加严格的语法检查。普通模式下静默错误会在严格模式下抛出异常。在严格模式下，部分语言特性会被改变，甚至会完全禁用一些不安全的语言特性

### 严格模式下被禁用的语言特性
* arguments 在严格模式下不能作为函数的别名，即不能通过修改 argument[n] 修改参数的值
```javascript
'use strict'
function fn(arg1) {
	// 非严格模式下，无论修改 arguments[0] 还是 arg1，两个变量值会保持一致
	console.log(arguments[0], arg1) // 1 1
	arg1 = 2
	console.log(arguments[0], arg1) // 1 2 arg1 参数的值修改为2，但 arguments[0] 保持不变
	arguments[0] = 3
	console.log(arguments[0], arg1) // 3 2
}
fn(1)
```
* 严格模式下，不允许使用解构参数、剩余参数、默认参数
* this 严格模式下，全局环境直接调用函数，`this:undefined`，非全局模式下 `this: window`

## 函数与严格模式
{%plantuml%}
@startmindmap
* 函数（function）
** 箭头函数（arrow function）
*** arguments 弃用，可使用剩余参数获取
**** Uncaught ReferenceError: arguments is not defined
** 函数声明（function declaration和函数表达式（function expression）
*** 严格模式
**** <b>arguments[0]</b>与<b>函数第一参数</b>分别代表两个变量，即 arguments 不能作参数别名
**** SyntaxError: "use strict" not allowed in function with non-simple parameters
*****_ [firefox]句法错误："use strict" 不允许在带 rest 参数的函数中
*****_ [chrome]非法的'use strict'指令，在带有非简单参数列表的函数中
****_ [firefox]句法错误："use strict" 不允许在带默认参数的函数中
****_ [firefox]句法错误："use strict" 不允许在带解构参数的函数中
*** 普通模式
**** arguments 可作参数别名使用
**** rest parameter 可以使用
** 构造函数
** 生成器函数
@endmindmap
{%endplantuml%}