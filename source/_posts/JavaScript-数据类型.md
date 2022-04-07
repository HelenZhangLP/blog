---
title: JavaScript 数据类型
date: 2019-02-08 10:38:30
categories:
- 技术
tags:
- JavaScript
---

![alt](/images/jsDataTypes.jpg)
与《JavaScript高级程序设计》不同，这张图中没有 null。我这边先就图中的信息作一个解析，图中，将数据类型分为值类型与引用类型。
<!-- more -->
http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html

## JavaScript 数据类型
### 基本类型
<font color="#f33">所以object/NaN 是否属于基本类型？？？ECMAScript 标准定义了7种数据类型：</font>

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

## 7 种语言类型
* Undefined
  > 为什么有些编程规范要求用 void 0 代替 undefined
  > 
  **用 void 运算把任意一个表达式变成 undefined**<br />
  **undefined 用来表示从未赋值的自然状态**<br />
  变量赋值前是 Undefined 类型，值为 undefined <br />
  undefined 不是关键字，为了避免无意篡改，建议使用 void 0 获取 undefined
* Null
* Boolean 用于表示逻辑上的真与假
* String 文本数据，最大长度 2^53-1。
String 是字符串的 UTF16 编码，charAT/charCodeAt/length 等方法针对的都是 UTF16 编码。<br />
  `字符串的最大长度实际上受字符串的编码长度影响`
  > Note: 现在的字符集国际标准，字符是以 Unicode 的方式表示的，每个 Unicode 的码点表示一个字符，理论上，Unicode 的范围是无限的。UTF 是 Unicode 的编码方式，规定了码点在计算机中的表示方法。常见的有 UTF-8 和 UTF-16
  > Unicode 码点通常用 U + ??? 表示，其中 ??? 是十六进制的码点值。
  > BMP 0-65536（U+0000 - u+FFFF) 的码点被称为基本字符区域。
  
Javascript 中的字符串是永远无法变更，一旦构造出来，无法用任何方式改变字符串的内容，所以字符串具胡值类型的特性。

Javascript 字符串把每个 UTF16 单元当作一个字符处理，处理非 BMP 的字符时，要格外小心。
* Number 数学中的有理数（正整数、0、负整数）
Symbol
Object

Null 类型值为 null，表示空值


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

## JavaScript 数据类型的 length 属性
### Uncaught SyntaxError: Invalid or unexpected token
`1.length` 基本数据类型，number 没有 length
### Uncaught SyntaxError: Unexpected token '.'
`{}.length` 把字面量对象赋值给一个变量，再取 length
