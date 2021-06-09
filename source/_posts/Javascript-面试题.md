---
title: JavaScript 面试题
date: 2019-03-07 14:33:19
categories:
- 技术
tags:
- JavaScript
- 面试题
- ES6
---

## 变量声明 let const var
{%table table-striped%}
| 名称    | 定义 |作用域     | 赋初值 |
| :------------- | :------------- | :---- | :---- |
| let       | 声明变量 | 块级作用域       | 可以不赋初值<br/>编译时声明并初始化为初值为 undefined |
| const       | 声明常量 | 块级作用域       | 必须赋初值 |
| var       | 声明变量 | 局部变量是函数作用域；<br/>全局变量作用域为该程序 | 不赋初值时，<br/>变量提升，代码执行前创建 js 环境，声明变量，变量赋初值为 undefined |
{% endtable %}

*`notice:`*
1.	let 和 var 都是用来声明变量的，但作用域不同。
2.	let 和 const 都是 ES6 开始正式加入 JavaScript 规范的
3.	let 编译时执行声明初始化，未编译时处于 temporal dead zone，此时变量 is not defined;
4.	const 声明常量必须赋初值(Uncaught SyntaxError: Missing initializer in const declaration)；
5.	const 声明的常量值不能修改。

##	1.	let
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

> let 声明的变量在定义编译执行时初始化。变量未声明之前不会初始化。这一时段称为暂存死区（Temporal Dead Zone, TDZ）。

```javascript
console.log(a);
console.log(b);

var a; // undefined
let b; // Uncaught ReferenceError: b1 is not defined
```

##	2.	const
> `Uncaught SyntaxError: Missing initializer in const declaration` const 声明的变量必须赋初值
	`Uncaught ReferenceError: C is not defined` const 声明块级作用域变量

### const 声明的变量值不能修改
```javascript
const variableTemplate = 30
try {
  variableTemplate = 22
} catch (error) {
  console.log(error) // TypeError: Assignment to constant variable.
}
console.log(variableTemplate)
```
### const 只读引用，只是变量的标识符不能重新分配
> 只是变量标识符不能重新分配
```javascript
const VARIABLE = 'assignment value'
VARIABLE = 'assignment value' // TypeError: Assignment to constant variable
const VARIABLE = 'Assignment to constant variable' // Uncaught SyntaxError: Identifier 'VARIABLE' has alreay been declared
const EN_OBJECT = {}
EN_OBJECT.ATTRIBUTE = 1
```

##	3.	var
JavaScript 根据赋值自动转换数据类型;
JavaScript 中 `+` 可用来链接字符串，如果运算中涉及字符串，结果为字符串链接。其它运算符（-、\*、\/）正确计算数字字符串;
JavaScript 中，null 乘任何数为 0;
JavaScript 代码程序执行前会先创建执行环境，变量、函数等被创建。代码运行时赋初值。这就是 JavaScript 的提升机制（hoisting）。

> var 声明变量可以不赋初值，默认值为 undefined；
	var 声明的变量没有块级作用域

### 与 var 不同 let 与 const 在全局声明定义的变量不会绑定到全局对象上（如 浏览器的全局对象 `window`)
```javascript
var a = 1
let b = 2
const c = 3
console.log(window.a, window.b, window.c) // 1 undefined undefined
```

### let 和 var 定义的变量作用域不同
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
