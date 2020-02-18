---
title: Javascript 之数据类型
date: 2019-02-08 10:38:30
categories:
- 技术
tags:
- Javascript
---

想对 Promise 作一个深入了解，`console.dir(Promise)`，得出以下结果。

{% qnimg Javascript-property-instance.png title: Javascript-knowlege extend:?imageView2/2/w/500 %}

上图可以看到 Promise 即有实例属性、方法，又有原型属性方法。接下来，我要归整归整，Javascript 中的实例属性方法与原型属性方法。

<!-- more -->
http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html
### Javascript 数据类型
纠结了多日，一直没能给出一个自己认为合理的表述方式，直到看到下图：
<!--{% qnimg jsDataTypes.jpg title: Javascript-knowlege extend: ?imageView2/2/w/500 %}-->
![alt](/images/Javascript/jsDataTypes.jpg)
与《Javascript高级程序设计》不同，这张图中没有 null。我这边先就图中的信息作一个解析，图中，将数据类型分为值类型与引用类型。
- [x] 值类型或称基本数据类型，可用 `typeof` 判断值类型
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
- [-] 引用类型，用 `instanceof` 判断引用类型

最新的 ECMAScript 标准定义了7种数据类型：
- [ ] 6 种基本数据类型
  * **布尔值（Boolean）**，有2个值分别是：true 和 false.
  * **null**，一个表明 null 值的特殊关键字。 `Javascript 是大小写敏感的，因此 null 与 Null、NULL或变体完全不同`。
  ```bash
    > null
      null
    > Null
      ReferenceError: Null is not defined
    > NULL
      ReferenceError: NULL is not defined
  ```
  * **undefined**，和 null 一样是一个特殊的关键字，undefined 表示变量未定义时的属性。
  * **数字（Number）**，整数或浮点数，例如： 42 或者 3.14159。
  * **字符串（String）**，字符串是一串表示文本值的字符序列，例如："Howdy" 。
  * **代表（Symbol）** ( 在 ECMAScript 6 中新添加的类型).。一种实例是唯一且不可改变的数据类型。

> 除了 null 与 undefined 以外，其它基本类型都有相应的包装对象
- [ ] 对象类型

### 对象类型
Javascript 中的几乎所有事物都是对象：字符串、数值、数组、函数等，
