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


