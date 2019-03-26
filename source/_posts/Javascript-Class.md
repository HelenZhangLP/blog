---
title: Javascript Class
date: 2019-03-20 17:06:19
tags:
- JavaScript
- ES6
---
### 定义类
#### 类声明
用带有 class 关键字的类名声明一个类
```javaScript
class Mermaid {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
}
```
> 类声明不会像函数声明一样会提升，需要先声明之后才可以访问。否则，报错：

```javaScript
new Mermaid();

class Mermaid {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
}
// Uncaught ReferenceError: Mermaid is not defined
```
#### 类表达式
```javaScript
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
> super 关键字调用父类构造函数
#### 静态方法
> `static` 定义一个类的静态方法，静态方法通常用于为一个应用程序创建工具函数。调用静态方法不需要实例化，不能通过类实例调用静态方法。
