---
title: Javascript 模块化
date: 2019-03-26 10:04:17
tags:
- Javascript
- commonjs
- amd
- cmd
- es6 modules
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
CommonJS 由 Javascript 社区 2009 年提出的包含模块、文件、IO、控制台在内的一系列标准。Node.js 的实现中采用了 CommonJS 标准的一部分，并在其基础上作了一些调整。
[参考 CommonJS 规范](http://Javascript.ruanyifeng.com/nodejs/module.html#toc0)


####  1. 概述认识 `module` 对象
> 1.  每个文件为一个模块，每个模块有自己的作用域。该模块定义的变量、函数、类均为私有，其它模块不可见。不会污染全局作用域
> 2.  CommonJS 规范规定，每个模块内部， `module` 代表当前模块。`module` 是一个对象，属性 `exports` 是对外接口。加载模块，则是加载 `module.exports` 属性。

`Node` 内部提供一个 `Module` 构造函数，所有模块都是 `Module` 的实例
```javascript
function Module(id, parent) {
    this.id = id; // 模块识别符，即带有绝对路径的模块文件
    this.parent = parent; // 返回是一个对象，为调用该模块的模块
    this.children = []; // 返回一个数组，表示该模块需要调用哪些模块
    this.filename = '/filename'; // 带有绝对路径模块的文件名
    this.loaded = true; // 返回一个布尔值，表示模块是否已经完成加载
    this.exports = {}; // 表示该模块输出的值
    this.paths = [];
}
```
Node 环境中 `console.log(module)` 会看到以下结果
```
Module {
  id: '.',
  path: '/Users/zhangliping/Desktop/demo/demo/commonJs',
  exports: {},
  parent: null,
  filename: '/Users/zhangliping/Desktop/demo/demo/commonJs/index.js',
  loaded: false,
  children: [],
  paths: [
    '/Users/zhangliping/Desktop/demo/demo/commonJs/node_modules',
    '/Users/zhangliping/Desktop/demo/demo/node_modules',
    '/Users/zhangliping/Desktop/demo/node_modules',
    '/Users/zhangliping/Desktop/node_modules',
    '/Users/zhangliping/node_modules',
    '/Users/node_modules',
    '/node_modules'
  ]
}
```

####  2. 导出
CommonJS 中，通过 `module.exports` 导出模块中的内容
```Javascript
// example.js
const variable = 'this is a variable'
const fn = function() { return 'this is a function' }
const obj = {}

// 将以上变量、函数、对象，对过 module.exports 暴露出去
module.exports.variable = variable
exports.fn = fn
module.exports.obj = obj

// { variable: 'this is a varialbe', fn: [Function: fn], obj: {} }
```
或
```javascript
module.exports = {
  variable: 'this is a variable',
  fn: function() { return 'this is a function'},
  obj: {}
}
// { variable: 'this is a variable', fn: [Function: fn], obj: {} }
```
- ***`错误案例 demo 1`***
> 以下写法错误，会导致 exports 指向新的对象引用，module.exports 还是原对象引用，是个空对象，所以 name 属性并不会导出。
```javascript
exports = {
  name: 'new object'
}
// {}
```
- ***`错误案例 demo 2`***
> 以下 demo 将 `exports` 和 `module.exports` 混合使用，原因在于先导出了一个函数，然后 `module.exports` 重新赋值为另一个对象，之前的对象属性丢失。
```javascript
exports.fn = function() {
  return 'this. is a function'
}
module.exports = {
  variable: 'this is a variable'
}
// { variable: 'this is a variable' }
```
- ***`错误案例 demo 3`***
> 先打印出 end, 再打印出 `exports` 的内容，说明这个 exports 之后的内容也会被执行，为提高代码可读性，`module.exports` 写在文件尾部
```javascript
exports.fn = function() {
  return 'this. is a function'
}
console.log('end')
// end
// { fn: [Function] }
```

#### 3. 导入
```javascript
// example.js
console.log('test the number of times the export file is executed')
exports.hello = 'instruction: export the function of hello'
```
```Javascript
// index.js
const example = require('./example')
console.log('wait a moment..., require the js file of example again')
require('./example')
```
---
```
[@zhangliingdembp:demo (master)]$ node ./commonJs/index.js
test the number of times the export file is executed
wait a moment..., require the js file of export again
[@zhangliingdembp:demo (master)]$ node ./commonJs/index.js
```
- ***error-1 Error: Cannot find module 'export'***
> `require('example')` 导致以上报错，导致入的文件要给出文件路径 `require('./example')`

`结论：`
> 1.  所有代码都运行在模块作用域，不会污染全局作用域。
> 2.  模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。`module.loaded` 默认为 false，如果加载过的话，该值会被设置为 true，模块代码将不再执行。
> 3.  模块加载的顺序，按照其在代码中出现的顺序。

require 函数接收表达式，可以利用该特性将动态加载模块到全局
```javascript
const needModules = ['moduleA','moduleB','moduleC',...]
needModules.map(item => {
  require('./'+item);
})
```

### 三、AMD规范
### 四、CMD规范
### 五、ES6模块化
2015 年 6 月，TC39 标准委员会正式发布 ES6(ECMAScript 6.0) 以后，Javascript 有了 `模块` 的概念。
