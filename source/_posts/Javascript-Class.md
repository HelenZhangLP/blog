---
title: JavaScript Class
date: 2019-03-20 17:06:19
tags:
- JavaScript
- ES6
---

## 定义类
### 类声明
用 class 关键字名声明一个类
```JavaScript
class Mermaid {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
}
```
<span class='custom-box custom-box-393'>constructor 不需要实参时，可以不定义。</span>
<font color="red">类声明不会像函数声明一样会提升，需要先声明之后才可以访问。否则，报错：Uncaught ReferenceError: Mermaid is not defined</font>

```JavaScript
new Mermaid();

class Mermaid {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
}
// Uncaught ReferenceError: Mermaid is not defined
```
### 类表达式
```JavaScript
/* 匿名类 */
let Mermaid = class {
  constructor() {}
}

/* 命名类 */
let Everything = class Everything {
  constructor() {
    console.log('Everything is posible')
  }
}
```
### 类的私有属性
<span class='custom-box custom-box-939'>私有属性可枚举</span>

```JavaScript
class Mermaid {
  type = 'flowchart'
}
console.log(new Mermaid()) // 打印 Mermaid 类的实例
/**
 * Mermaid {type: 'flowchart'}
 * type: "flowchart"
 *  [[Prototype]]: Object 
 */
```
### 类的私有方法
<span class='custom-box custom-box-939'>私有方法可枚举</span>

```JavaScript
class Mermaid {
  type = 'flowchart',
  getType = () => this.type
}
console.log(new Mermaid()) // 打印 Mermaid 类的实例
/**
 * Mermaid {type: 'flowchart'}
 * getType: () => this.type
 * type: "flowchart"
 * [[Prototype]]: Object 
 */
```
### 原型方法（公有方法）
<span class='custom-box custom-box-939'>公有方法不可枚举</span>

```JavaScript
class Mermaid {
  type = 'flowchart',
  getType = () => this.type
  render() {}
}
console.log(new Mermaid()) // 打印 Mermaid 类的实例
/**
 * Mermaid {type: 'flowchart'}
 * getType: () => this.type
 * type: "flowchart"
 * [[Prototype]]: Object 
 */
```
### 原型属性（公有属性）
<span class='custom-box custom-box-939'>公有属性可以枚举</span>

```JavaScript
class Mermaid {
  type = 'flowchart',
  getType = () => this.type
  render() {}
}
Mermaid.prototype.defaultType = 'flowchart'
console.log(new Mermaid()) // 打印 Mermaid 类的实例
/**
 * Mermaid {type: 'flowchart'}
 * getType: () => this.type
 * type: "flowchart"
 * [[Prototype]]: Object 
 */
```
### 静态方法与静态属性
<span class='custom-box custom-box-393'>`static` 定义一个类的静态方法，静态方法通常用于为一个应用程序创建工具函数。调用静态方法不需要实例化，不能通过类实例调用静态方法。</span>

```JavaScript
class Mermaid {
  type = 'flowchart',
  getType = () => this.type
  render() {}
  static id = 1
  static getId() {return this.id}
}
Mermaid.prototype.defaultType = 'flowchart'
console.log(new Mermaid()) // 打印 Mermaid 类的实例
console.log(Mermaid.id) // 1
console.log(Mermaid.getId()) // 1
/**
 * Mermaid {type: 'flowchart'}
 * getType: () => this.type
 * type: "flowchart"
 * [[Prototype]]: Object 
 *    constructor
 *      id: 1
 *      getId: ƒ getId()
 */
```