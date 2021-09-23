---
title: about .babelrc
tags:
- babel
---

> .babelrc 为 babel（ES6 转 ES5 的转码器） 的配置文件

Babel 会在正在被转录的文件的当前目录中查找一个 .babelrc 文件。 如果不存在，它会遍历目录树，直到找到一个 .babelrc 文件，或一个 package.json 文件中有 "babel": {} 。

在 options 中使用 "babelrc": false 来停止查找行为，或者提供--no-babelrc CLI 标志。

```json
{
  "presets": [
    ["env", {
      "modules": false, //默认都是支持 CommonJS
      "targets": {
        "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
      }
    }],
    "stage-2"
  ],
  "plugins" : [
    "transform-vue-jsx",
    ["transform-runtime",
      {
        "helpers": false,
        "polyfill": false,
        "regenerator": true,
        "moduleName": "babel-runtime"
      }]
    ]
}
```
`json 不支持 commit` 以下是对上面代码的解释说明

```
# presets 预设
# plugins 插件
```

不同的转译器作用不同的配置项，大致可分为以下三项：
1.语法转译器
主要对 JavaScript `最新的语法糖`进行编译，并不负责转译新增的 API 和全局对象。
而 Promise,Iterator,Generator,Set,Maps,Proxy,Symbol 等全局对象，以及一些定义在全局对象的方法（比如 includes/Object.assign 等）并不能被编译。

> @babel/preset-env 转译包

2.API 和全局对象转译器
负责转译新增的 API 和全局对象，保证在浏览器的兼容性。比如Promise,Iterator,Generator,Set,Maps,Proxy,Symbol 等全局对象，以及一些定义在全局对象的方法（比如 includes/Object.assign 等）具体可查询definitions.js




