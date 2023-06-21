---
title: JavaScript 变量
tags:
  - JavaScript
date: 2018-05-29 10:13:25
---


## JavaScript 变量
> JavaScript 通过 var,let,const 关键字定义变量

### const
在不需要重新赋值的特殊变量，指向一个固定的值时使用 const 声明变量，如此可避免代码不必要的变更，且为 JavaScript 引擎的性能优化提供便利。
> 通过 const 定义的变量必须赋初值，且不可变，只能赋值一次。可以修改 const 声明的引用类型变量，但不能重写。

#### <font color="#a33">Uncaught SyntaxError: Missing initializer in const declaration</font>
```JavaScript
const MAX_TIMES
```
> `const` 声明中缺失初值。**也就是说使用 `const` 声明变量时必须赋初值。**

#### <font color="#a33">Uncaught TypeError: Assignment to constant variable.</font>
```JavaScript
const MAX_TIMES = 3
MAX_TIMES = 5
```
> 分配给了常量值，即通过 `const` 声明的变量值不可以修改

#### 常量为引用类型
```JavaScript
const obj = {}
obj.att = 'attribute'

const arr = []
arr[0] = 1

console.log(obj, arr) // {att: 'attribute'}att: "attribute"[[Prototype]]: Object [1]
```

#### <font color="#a33">Uncaught ReferenceError: A is not defined</b>访问块级作用域
```JavaScript
{
	const A = 'A'
	var B = 'B'
}
console.log(A,B)
```

### var
> 通过 var 定义的变量，可变，且可多次修改赋值。仅在距离最近的函数内或全局作用域内注册。
```JavaScript
var global = 'global'

function globalFn() {
	var fn = 'fn'
	if (true) {
		var block = 'block'
	}
	
	console.log(fn, block) // fn block
}
globalFn()
// console.log(global, fn, block)
```
> <font color="#a33">Uncaught ReferenceError: fn is not defined</font> 全局不能访问函数作用域内部变量。
> 函数可以访问块级作用域内部变量 


### let
> 通过 let 定义的变量，可变，同样可多次修改赋值

```JavaScript
let global = 'global'

function globalFn() {
	let fn = 'fn'
	if (true) {
		let block = 'block'
	}
	
	console.log(global, fn, block) // Uncaught ReferenceError: block is not defined
}
globalFn()
// console.log(global, fn, block) // Uncaught ReferenceError: fn is not defined
```
> <font color="#a33">Uncaught ReferenceError: block is not defined</font> 函数**不可以**访问块级作用域内部变量 

## 总结
{%plantuml%}
@startmindmap
* 变量声明
** var
 *** 可变性
  ****[#yellowgreen] <b>可变</b>
 *** 块级作用域
  ****[#darkRed] <b>不存在</b>
** let
 *** 可变性
  ****[#yellowgreen] <b>可变</b>
 *** 块级作用域
  ****[#yellowgreen] <b>存在</b>
** const
 *** 可变性
  ****[#darkRed] <b>不可变</b>
 *** 块级作用域
  ****[#yellowgreen] <b>存在</b>
@endmindmap
{%endplantuml%}