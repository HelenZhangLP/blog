---
title: exports and module.exports
date: 2019-03-26 14:30:43
tags:
- JavaScript
- es6-modules
- node
---
### exports 和 module.exports
[参考文档](http://nodejs.cn/api/modules.html#modules_exports_shortcut)
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
