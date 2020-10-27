---
title: Javascript 面试题
date: 2019-03-07 14:33:19
categories:
- 技术
tags:
- Javascript
- 面试题
---

### 新增变量声明 let const
####	1.	let
>	声明一个块级作用域的本地变量

```javascript
if (true) {
	var a = 1;
	let b = 2;
	const C = 3;
}
console.log(a) // 1
console.log(b) // Uncaught ReferenceError: b is not defined
console.log(C) // Uncaught ReferenceError: C is not defined
```

> let 声明变量可以不赋初值，默认值为 undefined
```javascript
var a; // undefined
let b; // undefined
const c; // Uncaught SyntaxError: Missing initializer in const declaration
```
####	2.	const
> `Uncaught SyntaxError: Missing initializer in const declaration` const 声明的变量必须赋初值
	`Uncaught ReferenceError: C is not defined` const 声明块级作用域变量

####	3.	var
> var 声明变量可以不赋初值，默认值为 undefined；
	var 声明的变量没有块级作用域

#### var 和 let 定义变量时都会赋初值为是 undefined，不同的是 let 声明的变量在定义编译执行时初始化。
```Javascript
console.log(a); // undefined
console.log(b); // Uncaught ReferenceError: b is not defined

var a = 1;
let b = 2;
```

#### 与 var 不同 let 与 const 在全局声明定义的变量不会绑定到全局对象上（如 浏览器的全局对象 `window`)
```javascript
var a = 1
let b = 2
const c = 3
console.log(window.a, window.b, window.c) // 1 undefined undefined
```

#### let 和 var 定义的变量作用域不同
```javascript
function testVarScope() {
	var a = 1;
	{
		var a = 2;
	}
	console.log(a, 'var')
}

testVarScope(); // 2 "var"
/**
 *	var 声明的变量在同一函数作用域中为同一变量，所以值会修改
 */

function testLetScope() {
	let a = 1;
	{
		let a = 2;
	}
	console.log(a, 'let')
}

testLetScope(); // 1 "let"

/**
 *	{} 为一个作用域，{} 中的 a 变量与 函数中首先声明的 a 为两个变量，值不会相互修改
 */
```

### 一、 const
> TypeError: Assignment to constant variable
```javascript
const variableTemplate = 30
try {
  variableTemplate = 22
} catch (error) {
  console.log(error) // TypeError: Assignment to constant variable.
}
console.log(variableTemplate)
```

### 二、 const 只读引用，只是变量的标识符不能重新分配
> 只是变量标识符不能重新分配
```javascript
const VARIABLE = 'assignment value'
VARIABLE = 'assignment value' // TypeError: Assignment to constant variable
const VARIABLE = 'Assignment to constant variable' // Uncaught SyntaxError: Identifier 'VARIABLE' has alreay been declared
const EN_OBJECT = {}
EN_OBJECT.ATTRIBUTE = 1
```

### 三、 作用域 + promise && promise.then
```javascript
var msg = 1
{
	let msg = 2
}
console.log(msg)
var p1 =  new Promise(resolve => {
	msg = 3
	resolve(msg)
	console.log(4)
})
console.log(5)
var p2 = p1.then(res => {
	console.log(res)
})

console.log(p1 === p2)
// 1 4 5 false 3

/* p1 gng p2 不相等的原因 */
// [[PromiseStatus]]: "resolved"
// [[PromiseValue]]: 3
console.log(p2)
console.log(p1)
// [[PromiseStatus]]: "resolved"
// [[PromiseValue]]: undefined
```
### 四、 箭头函数作用域

```javascript
function fun() {
	return {
		name: 'fn',
		show: ()=>{
			console.log(this.name)
		}
	}
}

var name = 'fun'
fun().show()
```

### 五、正则相关 - 题目如下
```javascript
const str='version2.1 version2.2';
const regx = /(\w+)(\d)\.(\d)/g;

str.match(regx)
/**
 [ 'version2.1', 'version2.2' ]
 */
regx.exec(str)
/** [ 'version2.1',
  'version',
  '2',
  '1',
  index: 0,
  input: 'version2.1 version2.2',
  groups: undefined ]
	*/
```
1.	console.log(str.match(regx)) 结果如上，因为有正则中有 `g` 全局搜索标识符。 所以结果如上代码。
2.	console.log(regx.exec(str)) 结果如上。
3.	match 和 exec 的区别：
  * match 是 String(String.prototype.match()) 的原型方法；exec 是 RegExp（RegExp.prototype.exec()） 的原型方法。
  * 在 regx 不加 `g` 标识符（global）时，他们的结果是相同的，返回一个数组。数组元素包括匹配字符串、匹配组、index 匹配的位置， input 原始字符串， groups;
  * 在 regx 加 `g` 标识符（global)）时，结果如上。`str.match(regx)` 返回的是所有匹配的字符串数组；`regx.exec(str)` 返回的第一组匹配的数组，并记录 `lastIndex` 在 regx 中，再次执行 `regx.exec(str)` 会返回下一组匹配结果。以此类推，直到最后结果为 null。

### 六、 数组排序
> 对数组 [10, 20, 1, 2] 进行排序
```javascript
[10, 20, 1, 2].sort(); // [ 1, 10, 2, 20 ]

```

### 六、 在一个构造方法中可以使用super关键字来调用一个父类的构造方法。
```javascript
// ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```
