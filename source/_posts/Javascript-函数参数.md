---
title: Javascript-函数参数
date: 2023-02-03 12:16:15
tags:
- Function
- Javascript
---

## 形参（parameter）
> 定义函数时，描述函数参数所列举的变量
## 实参（argument）
> 函数调用时所传递给函数的值

## 实参与形参
* 实参的数量大于形参，额外的实参不会赋值给任何形参。
```javascript
function fn(arg1, arg2) {
	console.log(arg1, arg2)
}
fn(1,2,3) // 1,2 3不会赋值给任何形参变量
```
* 实参的数量小于形参，形参中没有被赋值变量值为 `undefined`
```javascript
function fn(arg1, arg2) {
	console.log(arg1, arg2)
}
fn(1) // 1,undefined 形参 arg2 被赋值为 undefined
```

## 剩余参数（rest parameter）
> 将未命名的形参的参数创建为一个不定数量的数组。如下，函数最后一个命名参数前加 ... 前缀，这个参数就叫剩余参数数组。如 ...restArg，其中包含传入的其它参数
```javascript
function fn(firstArg, ...restArg) {}
fn(1,2,3,4) // firstArg = 1, restArg = [2, 3, ,4]
```
<font color='#a33'>SyntaxError: parameter after rest parameter**注：只能在最后一个形参才会被视为剩余参数，即参数不能在剩余参数之后**</font>

## 默认参数（default parameter）
适应使用默认参数，避免空值。
> 允许在调用时没有值，或 `undefiend` 传入时使用指定的默认参数值
```javascript
function user(name, career='teacher') {
	return name + ' is a ' + carrer
}
```
> 对后面参数赋值可以引用前面默认参数
```javascript
function user(name, career='teacher', message=name + ' is a ' + carrer){
	return message
}
```

## 隐形参数
### arguments
* 传递给函数的所有参数的集合，可以访问函数调用过程传递的实际参数。
* `arguments` 有 `length` 属性，可以通过下标访问，但它不是数组，而是类数组数据结构
```javascript
function fn() {
	console.log(arguments.length, arguments[2]) //4 3
}
fn(1,2,3,4)
```
* `arguments` 可以作为参数别名使用，二者相互影响
```javascript
function user(name,age) {
	console.log('age', age, arguments[1]) //age 5 5
	arguments[1] = 10
	console.log('age', age, arguments[1]) //age 10 10 
	age = 7
	console.log('age', age, arguments[1]) //age 7 7
}

user('hel', 5)
```
* <font color="#a33">`arugments` 对象作为函数参数别名会影响代码的可读性，在 Javascript 提供的严格模式中无法使用</font>
