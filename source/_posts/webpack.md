---
title: Webpack
date: 2021-03-29 14:36:25
tags:
- 打包工具
---

Webpack 是一个模块打包器，能够根据模块的依赖关系递归地构建一个依赖关系图（Dephendency Graph），当中包含了应用程序的所有模块，最后打包成一个或多个bundle
Webpack 打包出的静态资源在 HTML 中引用。Webpack 能将 CSS 和图片等打包到同一个包；
打包前还能对文件进行预编译；
可以配置多个入口，将包拆分
还能进行热替换

四个核心概念
* entry(入口)
* output(输出)
* loader(转换器)
* plugins(插件)

<!-- more -->

## Error: Cannot find module 'webpack-cli/bin/config-yargs'
code: 'MODULE_NOT_FOUND'
> webpack-dev-server --open --mode development
  "webpack-cli": "^4.6.0"

<font color="red">**降版本，将 webpack-cli 版本降至 3**</font>
```
$ npm i webpack-cli@3 -D --force
```
