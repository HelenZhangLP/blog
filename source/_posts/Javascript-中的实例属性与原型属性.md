---
title: Javascript 中的实例属性与原型属性
date: 2019-02-08 10:38:30
categories:
- 技术
tags:
- Javascript
---

想对 Promise 作一个深入了解，`console.dir(Promise)`，得出以下结果。

{% qnimg javascript-property-instance.png title: javascript-knowlege extend:?imageView2/2/w/500 %}

上图可以看到 Promise 即有实例属性、方法，又有原型属性方法。接下来，我要归整归整，Javascript 中的实例属性方法与原型属性方法。

### Javascript 数据类型
最新的 ECMAScript 标准定义了7种数据类型：
- [ ] 6 种基本数据类型
  * **布尔值（Boolean）**，有2个值分别是：true 和 false.
  * **null**， 一个表明 null 值的特殊关键字。 `JavaScript 是大小写敏感的，因此 null 与 Null、NULL或变体完全不同`。
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
- [ ] 对象类型

### 对象类型
JavaScript 中的所有对象都来自 Object, Object 其实就是一个构造函数
