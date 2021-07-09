---
title: 函数 bind call apply
date: 2019-03-18 14:17:37
tags:
- JavaScript
- function
- 函数作用域
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
从代码的角度看，`this` 代表的是一个对象，具体代表的是执行调用 `public` 的当前对象

```javascript
toString.call([]) // "[object Array]"
```
`toString` window 环境中调用的，返回 `window.toString()` 的返回值。使用 `call`，指定参数[] 为 this，用来判断数据类型

## 可以使用 `call,apply,bind` 来决定 `this` 的参考对象
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
在 window 环境下，直接调用 `public()`，相当于 `window.public()`，这时的 `this` 代表 `window` 对象。使用`call` 传入参数对象 person，就相当于让 `this=person`

## Function.prototype.call()
> `call()` 使用一个指定的 `this` 值和单独给出一个或多个参数来调用一个函数
<font color="#f33">对象实例 A 需要调 对象 B 的方法，采用 call 传入对象实例 A，执行调用</font>

```javascript
/**
 * thisArg 在 function 函数运行时使用的 this 值
 * 如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。
 * arg1, arg2, ...指定的参数列表。
 */
function.call(thisArg, arg1, arg2, ...)
```

### <font color="#f99">this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。</font>

```javascript
// 'use strict';
var sData = 'Wisen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call();  // sData value is Wisen
```
> <font color="#f33">注意在做严格模式测试时 `use strict` 必须写在代码第一行，否则测试无效</font>

```JavaScript
  // 调用一个具有给定 this 的函数
  function TempObj(){
    this.tempArray = [1,4,6,2]
  }
  
  let tempObj = new TempObj()

  // 指定作用域为 tempObj
  console.log(Math.max.call(tempObj, ...tempObj.tempArray))
```
----
> 每个数据类型都有自己的 toString 方法，但实现方式不同。现在需要 emptyArray，调用对象 toString ，拿到对象 toString 方法的返回值
```javascript
let emptyArray = []
window.toString() // "[object Window]"
emptyArray.toString() // ""
window.toString.call(emptyArray) // "[object Array]"
```

## Function.prototype.apply 的使用场景
<font color="#f99">有一组自变量必须在多次调用间共享</font>
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

## Function.prototype.bind 的使用
> Function.prototype.bind 执行结果返回新函数，这个新函数的 this 绑定对象，会固定为调用 bind() 时指定的对象
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
 * 返回以下函数
 * forEach(callback) {
     console.warn(callback(this.i), 1)
   } 2
 */
```


### bind、call、apply
[bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
[call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
[apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
#### 从定义上对比看
* bind - 创建一个新的函数
* call - call 与 apply 相似，仅传参不同
* apply - 调用一个具有给定 this 值的函数
> func.apply(thisArg, [argsArray])

** DEMO **
```JavaScript
// 数组合并
let a = [1,2], b = [3,4]
a.push(b) // 3，返回数组 length
a.concat(b) // [1, 2, 3, 4] 创建了一个新数组
a.push.apply(a, b) // 4
console.log(a) // [1, 2, 3, 4]
a.push.call(a, ...b) // 6
console.log(a) // [1, 2, 3, 4, 3, 4]
```
