---
title: Javascript 面试题
date: 2019-03-07 14:33:19
categories:
- 技术
tags:
- Javascript
- 面试系列
---
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
#### var、let、const
> let && const 声明常量不会成为 window 属性
```javascript
var a = 1
let b = 2
const c = 3
console.log(window.a, window.b, window.c) // 1 undefined undefined
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
