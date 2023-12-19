---
title: JavaScript-数字类型
date: 2012-01-08 10:38:30
categories:
- 技术
tags:
- JavaScript
---

## JavaScript 数据类型
### 基本类型（也称原型数据类型，值类型）
基本类型直接代表了最底层的语言实现，是种非对象无方法与属性的数据，有 7 种原始数据类型。
{%plantuml%}
@startmindmap
* 数据类型
** 基本数据类型/原始数据类型(primitive data type)/值类型
*** string
*** number
*** boolean
*** undefined
*** null
*** Symbol
*** bigint
** 引用类型
*** object
**** {}
**** []
**** /^$/
*** function
@endmindmap
{%endplantuml%}
<font color="#f33">所以object/NaN 是否属于基本类型？？？ECMAScript 标准定义了7种数据类型：</font>

## 基本数据类型 —— number
> number 数字类型的值可以是正数、负数、零以及小数；也是可以 Infinity 无穷大；还可以是 NaN(not a number) 是 number 类型但不是一个有效数字。

### isNaN 检测参数是否为有效数字
<b class="custom-box-339">`isNaN([value])`</b><span class="custom-box-993">检测一个值[value]是否为“非有效数字”，返回 true 代表[value]为非有效数字，false 则代表[value]为有效数字。</span>
```JavaScript
// 如果参数[value]不是数字类型的值，则浏览器会默认采用“基于 Number([value]) 进行隐式转换”的方式，将参数转换为数字类型。
if([value] !== typeof number) {
  Number([value]) // 浏览器基于 Number([value]) 实现数据类型转换
}
```

### Number([value]) 参数转换为数字详解
* 字符串转换为数字类型
  > Number(''), 则转换结果为 0;
    Number('1'), 则转换结果为 1;
    Number('1px'), 则转换结果为 NaN;
<div class="custom-box-933">字符串中带除 ‘.’ 以外的非有效数字，转换结果都为 NaN</div>
 
  > Number('.1.1'), 多个小数点为无效数字，所以转换结果依然是 NaN

* 布尔类型转换为数字类型
  > Number(true) 结果为 1；
    Number(false) 结果为 0；

* BigInt 可以转换为数字 
  > Number(1n) 结果为 1；

* Symbol 类型的值不参转换为数字，<font color="#f33">Uncaught TypeError: Cannot convert a Symbol value to a number</font>

* 引用类型通过 Number([value]) 转换为数字
  * 获取引用类型的 [Symbol.toPrimitive] 属性值
  * 没有 Symbol.toPrimitive 该属性，则获取当前引用类型的 valueOf 原始值
  * 如果没有原始值，则将通过 toString 将其转换为字符串，最后将字符串转换为 Number


### parseInt/parseFloat 将其它类型数据转换为数字类型
> parseFloat([value])/parseInt([value]) 入参 value 必须为字符串数字类型，如果不是浏览器会通过 [value].toString() 隐式转换。
转换方式是从字符串左侧第一个字符开始查的有效数字字符，遇到非有效数字停止查找。返回有效数字。如果未找到有效数字，则返回 NaN.

### Number.prototype.toFixed([N]) 保留小数点后几位
### Number.prototype.toExponential
> 返回一个以指数表示法表示该数字的字符串
### toPrecision

#### Symbol
`Symbol([description])` description 对 symbol 的描述，可用于调试但不是访问 symbol 本身
```javascript
const symbol1 = Symbol()
typeof symbol1 // "symbol1"
```
* **布尔值（boolean）**，有2个值分别是：true 和 false.
* **null**，一个表明 null 值的特殊关键字。 `JavaScript 是大小写敏感的，因此 null 与 Null、NULL或变体完全不同`。
```javascript
  console.log(null) // null
  console.log(Null) // ReferenceError: Null is not defined
  console.log(NULL) // ReferenceError: NULL is not defined
```
* **undefined**，和 null 一样是一个特殊的关键字，undefined 表示变量未定义时的属性。
* **数字（number）**，整数或浮点数，例如： 42 或者 3.14159。
* **字符串（string）**，字符串是一串表示文本值的字符序列，例如："Howdy" 。
* **代表（Symbol）** ( 在 ECMAScript 6 中新添加的类型).。一种实例是唯一且不可改变的数据类型。
  
值类型或称基本数据类型，可用 `typeof` 判断值类型
* undefined
  ```bash
  > typeof undefined
    'undefined'
  ```
  
* null
  ```bash
  > typeof null
  'object'
  ```
  
* number、boolean、string
  ```bash
  > typeof undefined
  'undefined'
  > typeof 1
  'number'
  > typeof ''
  'string'
  > typeof true
  'boolean'
  ```

### 引用类型 
部分基本类型的包装对象
String/Number/Boolean/Object
Function,Array,Date,RegExp,Error

> 引用类型，用 `instanceof` 判断引用类型

## JavaScript 所有数据类型取反运算
`Boolean Number String Object Function Array Date RegExp Error Symbol`

```javascript
!null // true
!undefined // true
!NaN //true

!0 // true
!'' // true
!false // true
```
> 除几上几种取反为 true，其它均为 false。二种情况</br>
> 1.  基础数据类型中，数字大于 0，非空字符串，取返回为 true
> 2.  其它可归为对象类型，字面量 [],{} 与基本类型的包装类型以及其它如 Date,Error 返回值都为对象，对对象取反，返回值为 false

## 数据结构
## JavaScript 数据类型的 length 属性
### Uncaught SyntaxError: Invalid or unexpected token
`1.length` 基本数据类型，number 没有 length
### Uncaught SyntaxError: Unexpected token '.'
`{}.length` 把字面量对象赋值给一个变量，再取 length

## JS 输出方案
### 浏览器弹窗
> 一般用于与客户交互，给出一些提示。<span class="custom-box-933">alert 的参数如果不是数字类型会先转换成数字类型再输出</span>
* alert([content]) 
* confirm() 
* prompt

```JavaScript
alert({}) //[object, Object]
alert([1,2,3]) // 1,2,3
alert(BigInt(1)) // 1
```
<font color="#f33">Uncaught TypeError: Cannot convert a Symbol value to a string</font>
```JavaScript
alert(Symbol())
```

### 控制台输出
> 一般用于开发中开发者调试代码，<span class="custom-box-933">console.log 中，参数是什么类型，控制台输出什么类型</span>
* console.log
* console.time / console.timeEnd
* console.dir
* console.table

### website 页面输出
* document.write
* [element].innerHTML/innerText
* [input].value