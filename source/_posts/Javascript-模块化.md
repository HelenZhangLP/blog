---
title: Javascript 模块化
date: 2019-03-26 10:04:17
tags:
- Javascript
- Commonjs
- AMD
- CMD
- ES6 modules
---
Javascript 起初只为了验证表单，后来加入了动画等。只需要在 html 中添加 `<script></script>` 标签即可。随着前端复杂度的提高，对于前端代码的`可读性、可扩展性`有较高的要求，就需要分多模块。这一阶段就是无模块化阶段：
### 一、无模块化
无模块化阶段代码如下：
```html
<script src="jquery.js"></script>
<script src="jquery_scroller.js"></script>
<script src="main.js"></script>
...
```
> 1.  变量、对象、函数均绑在全局，`污染全局作用域` 会出现命名冲突的问题。
> 2.  `依赖关系不明显`，开发手动处理各模块依赖，维护成本加大。

<!-- more -->

### 二、CommonJS规范
[参考 CommonJS 规范](http://javascript.ruanyifeng.com/nodejs/module.html#toc0)
- [ ] ***概述***
> 1.  每个文件为一个模块，每个模块有自己的作用域。该模块定义的变量、函数、类均为私有，其它模块不可见。
> 2.  CommonJS 规范规定，每个模块内部， `module` 代表当前模块。`module` 是一个对象，属性 `exports` 是对外接口。加载模块，则是加载 `module.exports` 属性。Demo 如下：

```javascript
// example.js
const variable = 'this is a varialbe'
const fn = function() { return 'this is a function' }
const obj = {}

// 将以上变量、函数、对象，对过 module.exports 暴露出去
module.exports.variable = variable
exports.fn = fn
module.exports.obj = obj
```
- [ ] ***`require` 加载模块***

```javascript
const example = require('example.js')
```
> 1.  所有代码都运行在模块作用域，不会污染全局作用域。
> 2.  模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
> 3.  模块加载的顺序，按照其在代码中出现的顺序。

每个模块内部，都有一个module对象，代表当前模块。它有以下属性。

module.id 模块的识别符，通常是带有绝对路径的模块文件名。
module.filename 模块的文件名，带有绝对路径。
module.loaded 返回一个布尔值，表示模块是否已经完成加载。
module.parent 返回一个对象，表示调用该模块的模块。
module.children 返回一个数组，表示该模块要用到的其他模块。
module.exports 表示模块对外输出的值。

### 三、AMD规范
### 四、CMD规范
### 五、ES6模块化
