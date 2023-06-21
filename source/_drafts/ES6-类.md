---
title: ES6 类
date: 2019-02-11 16:57:10
categories:
- 技术
- JavaScript
tags:
- ES6
- 对象
---

## class 类声明
声明创建一个基于原型继承的具有给定名称的新类
<font color="#f33">
*   类声明不允许再次声明已存在的类，否则抛出类型错误 
*   类声明体在严格模式下运行。构造函数是可选的
*   类声明不可以提升
    </font>
    
> ES6 开始提供类语法，严格的说，是模拟基于类的面向对象。<u>本质上 JavaScript 仍是基于原型的面向对象，ES6 的语法，主要是提供标准化的类仿真方式。通过语法糖，让程序变得简洁</u>
    
```JavaScript
class Component {
//    constructor 是定义实例的初始流程
    constructor() {}
    
    // object.defineProperties() 定义属性，定义私有性设定
    // ES2020 实验草案中，拉回了定义私有类字段 #privateMethod() / static #private_static_field
    
    // 用来区别 class 组件和 function 函数组件的属性
    static isReactComponent = true
    
    // 异步更新队列，push 一个任务
    setState() {}
}
```
class 定义的类只能用 new 来建立实例，直接以函数的调用方式，如 Component(),Component.call() 等会发生 TypeError

<font color="#f99">test</font>