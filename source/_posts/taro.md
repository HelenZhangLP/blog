---
title: taro 坑系列
date: 2019-01-25 15:52:09
categories:
- 技术
tags:
- 前端
- 框架
- taro
---
### taro 坑
#### 坑 - 1
```JavaScript
/Users/lipingzhang/Project/**/**/src/pages/order/detail/index编译失败！
TypeError [ERR_INVALID_ARG_TYPE]: The "path" argument must be of type string
    at assertPath (path.js:28:11)
    at Object.relative (path.js:1265:5)
    at pageDepComponents.forEach.depComponent (/Users/lipingzhang/.nvm/versions/node/v9.2.0/lib/node_modules/@tarojs/cli/src/weapp.js:1326:64)
    at Array.forEach (<anonymous>)
    at componentMap.forEach.component (/Users/lipingzhang/.nvm/versions/node/v9.2.0/lib/node_modules/@tarojs/cli/src/weapp.js:1320:31)
    at Array.forEach (<anonymous>)
    at realComponentsPathList.forEach.component (/Users/lipingzhang/.nvm/versions/node/v9.2.0/lib/node_modules/@tarojs/cli/src/weapp.js:1319:24)
    at Array.forEach (<anonymous>)
    at buildSinglePage (/Users/jo32/.nvm/versions/lipingzhang/v9.2.0/lib/node_modules/@tarojs/cli/src/weapp.js:1316:30)
    at <anonymous>
```
<!-- more -->
#### for 坑 - 1 - 原因
> 组件 render 方法返回 null 时会引起编译错误
> `/order/detail/index` 该页面使用了组件 atCountDown，该组件编译错误
### for 坑 - 1 - 解决方法
** 升级 tarojs tarojs-cli 为 1.8 **
```
* npm install @tarojs@1.8 *
* npm install @tarojs-cli@1.8 *
```
