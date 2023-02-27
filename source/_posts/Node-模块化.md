---
title: 模块化
date: 2019-03-26 14:30:43
tags:
- JavaScript
- es6-modules
- node
---

> 参考：
> *	[Nodejs](http://nodejs.cn/api/modules.html#modules_exports_shortcut)
> * [tutorials](https://www.tutorialsteacher.com/nodejs/nodejs-module-exports)

## CommonJS 模块 
Node.js 打包 JavaScript 代码的原始方式
> Node.js 中每个文件都被视为一个单独的模块

NodeJs 环境运行以下代码，测试代码保存在 index.js 文件下
```JavaScript
var a = 1
console.log(global.a)
```
```bash
$ node index.js
undefined
```
以上代码若运行在浏览器环境，`widnow.a` 必定输出 1，浏览器中默认绑定在全局 `window` 对象上。但在 NodeJs 环境中 <span class="custom-box custom-box-933">在 NodeJs 中，存在模块化，它会将当前文件，按模块的方式进行加载或导出</span>相当于：
```JavaScript
(function() {
	var a = 1
	console.log(window.a)
})()
```
即在函数环境中访问全局环境的变量。<span class="custom-box custom-box-939"></span>
### exports && module.exports


## exports 和 module.exports
nodeJS 官网中对 require() 有一段假设实现，如下：
```JavaScript
function require() {
  const module = { exports: {} };
  ((module, exports) => {
    function sFn() {}
    exports = sFn
    module.exports = sFn
  })(module, module.exports);
  return module.exports;
}
```
#### 回过头来看定义
1.  exports
```JavaScript
// utils.js
exports.cssLoaders = function (options) {
  options = options || {}

  var cssLoader = {
    loader: 'css-loader',
    options: {
      minimize: process.env.NODE_ENV === 'production',
      sourceMap: options.sourceMap
    }
  }
}

// vue.loader.config.js
var utils = require('./utils')
var config = require('../config')
var isProduction = process.env.NODE_ENV === 'production'

module.exports = {
  loaders: utils.cssLoaders({
    sourceMap: isProduction
      ? config.build.productionSourceMap
      : config.dev.cssSourceMap,
    extract: isProduction
  }),
  postcss: [
    require('autoprefixer')({
      browsers: ['iOS >= 7', 'Android >= 4.1']
    })
  ]
}
```
> exports 导出的是函数，引入文件 require() 之后直接调用
> module.exports 导出的是一个对象

### export && export default
