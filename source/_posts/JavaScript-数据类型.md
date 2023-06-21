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

## JavaScript 数据类型
### 基本类型
基本类型直接代表了最底层的语言实现，是种非对象无方法与属性的数据，有 7 种原始数据类型。
{%plantuml%}
@startmindmap
* 数据类型
** 原始数据类型(primitive data type)
*** string
*** number
*** bitint
*** boolean
*** undefined
*** null
*** Symbol
@endmindmap
{%endplantuml%}
<font color="#f33">所以object/NaN 是否属于基本类型？？？ECMAScript 标准定义了7种数据类型：</font>

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

## JavaScript 数据类型的 length 属性
### Uncaught SyntaxError: Invalid or unexpected token
`1.length` 基本数据类型，number 没有 length
### Uncaught SyntaxError: Unexpected token '.'
`{}.length` 把字面量对象赋值给一个变量，再取 length