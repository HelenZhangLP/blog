---
title: JavaScript 函数上下文
date: 2023-02-02 18:39:45
tags:
- JavaScript
- Function
---

## 函数上下文
函数的隐式参数 `this` 表示被调用的函数上下文的对象。代表函数调用相关对象。**`this` 指向受函数定义、位置和调用方式影响**
### 函数调用与this
{%plantuml%}
@startmindmap
* 函数
** 函数调用 
*** 其它函数
**** 严格模式下，函数直接调 `this: undefined`
**** 非严格模式下，函数直接调 `this: window`
*** 作为对象方法调用
**** this 指向调用该方法的对象，严格模式下实现一致
*** 作为构造函数调用
**** this 指向新构造实例
*** 通过 call/apply/bind 调用
**** 可用于指定 this 具体指向某个对象
** 箭头函数
*** 定义时继承函数上下文，与函数的调用方式无关 
@endmindmap
{%endplantuml%}

## 修改函数上下文
> 使用 bind、call、apply 仅只修改函数声明，函数表达式 `this`，对于箭头函数无能为力。箭头函数 `this` 指向声明时所处函数上下文

bind、call、apply 都是用第一个参数来指定 `this` 的，也就是说函数体内若使用 `this`，那么它指的就是执行函数体内使用 `this`，`this` 指的就是 `bind、call、apply` 的第一个参数。具体使用及函数返回值，参考下文
### [Function.prototype.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
> func.apply(thisArg, [argsArray])，返回调用有指定 this 值和参数的函数的结果。则返回 undefined。
#### demo
```javascript
let _this = {}
function fn(a, b, c) {
	return this + ' ' + (a + b + c)
}

console.log(fn(1,2,3)) // [object Window] 6
console.log(fn.apply(_this, [4,5,6])) // [object Object] 15
```
#### demo
<font color="#a33">有一组自变量必须在多次调用间共享</font>
```javascript
function plus(num1, num2) {
  return this.num + num1 + num2
}

let arr = [5,10]
let numberObj1 = {num: 1}
let numberObj2 = {num: 2}

console.log(plus.apply(numberObj1, arr)) // 16
console.log(plus.apply(numberObj2, arr)) // 17
```
#### demo
```javascript
// 数组合并
let a = [1,2], b = [3,4]
a.push(...b) // 3，返回数组 length, 修改数组 a = [1,2,3,4]
a.push.apply(a, b) // 4
console.log(a) // [1, 2, 3, 4]
```
### [Function.prototype.call()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
> `call()` 使用一个指定的 `this` 值和单独给出一个或多个参数来调用一个函数
> <font color="#a33">对象实例 A 需要调 对象 B 的方法，采用 call 传入对象实例 A，执行调用</font>
> 使用调用者提供的 this 值和参数调用该函数的返回值。若该方法没有返回值，则返回 undefined。同 apply
```javascript
/**
 * thisArg 在 function 函数运行时使用的 this 值
 * 如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。
 * arg1, arg2, ...指定的参数列表。
 */
function.call(thisArg, arg1, arg2, ...)
```

#### Demo
```javascript
function public() {
  return `${this.name}'s age is ${this.age}`
}

let person1 = {
  name: 'helen',
  age: 12
}

let person2 = {
  name: 'audery',
  age: 4
}

public.call(person1)
public.call(person2)
```
在 window 环境下，直接调用 `public()`，相当于 `window.public()`，这时的 `this` 代表 `window` 对象。使用`call` 传入参数对象 person，就相当于指定当前函数调用的执行上下文为 `person` 对象

#### demo
<font color="#a33">`this`可能不是该方法看到的实际值：**如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象**，原始值会被包装。</font>
<font color="#f33">注意在做严格模式测试时 `use strict` 必须写在代码第一行，否则测试无效</font>

```javascript
// 'use strict';
var sData = 'Helen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call();  // sData value is Helen，严格模式下，Uncaught TypeError: Cannot read properties of undefined (reading 'sData')
display.call(null);  // sData value is Helen，严格模式下，同上
display.call(undefined);  // sData value is Helen，严格模式下，同上
display.call({});  // sData value is undefined
```
#### demo
```JavaScript
  // 调用一个具有给定 this 的函数
  function TempObj(){
    this.tempArray = [1,4,6,2]
  }
  
  let tempObj = new TempObj()

  // 指定作用域为 tempObj
  console.log(Math.max.call(tempObj, ...tempObj.tempArray))
```

#### demo
```JavaScript
// 数组合并
let a = [1,2], b = [3,4]
a.push(...b) // 4，返回数组 length，修改数组 a, 数组 a = [1,2,3,4]
a = [1,2]
a.concat(b) // [1, 2, 3, 4] 创建了一个新数组，不会修改数组 a
a = [1,2]
a.push.call(a, ...b) // 同直接调用 push 方法，返回数组 length 为 4，修改数组 a, 数组 a = [1,2,3,4]
```
#### demo
> 每个数据类型都有自己的 toString 方法，但实现方式不同。现在需要 emptyArray，调用对象 toString ，拿到对象 toString 方法的返回值
```javascript
let emptyArray = []
window.toString() // "[object Window]"
emptyArray.toString() // ""
window.toString.call(emptyArray) // "[object Array]"
```
`toString` window 环境中调用的，返回 `window.toString()` 的返回值。使用 `call`，指定参数[] 为 this，用来判断数据类型

### [Function.prototype.bind()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 
> Function.prototype.bind 执行结果返回新函数，这个新函数的 this 绑定对象，会固定为调用 bind() 时指定的对象`
#### demo
```javascript
function forEach(callback) {
  callback(this.i)
}

let obj = {i: 0}

let fn = forEach.bind(obj)
fn(function(i) {
  console.log(i, 3)
})
/**
 * bind 方法返回的是一个函数
 * 在 forEach.bind(obj) 将 this 绑定到 obj 对象
 * 返回函数 forEach(callback) {
     console.warn(callback(this.i), 1)
   }
 */
```
#### demo
```javascript
	var bindDemo = {
		clicked: false,
		click: function() {
			console.log(this.name);
			this.clicked = true
			assets('assets', this.clicked, this + '')
		}
	}

	var button = document.getElementById('button')
	button.addEventListener('click', bindDemo.click.bind(bindDemo)) 
```
> click 函数调用绑定方法，<font color='#a33'>创建一个新函数</font>，该函数体一致，函数行为一致。this 指向绑定参数


