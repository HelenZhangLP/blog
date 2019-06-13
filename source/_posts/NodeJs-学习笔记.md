---
title: NodeJs 学习笔记
date: 2019-05-31 14:32:44
tags:
- javascript
- node
---
### path
> 提供用于处理文件路径和目录路径的实用工具

```javascript
const path = require('path')
```
<!-- more -->
- [x] path.resolve()
> 将路径或片断序列解析为绝对路径
```javascript
// 不传参
path.resolve()  // '/Users/lipingzhang/gitProject/blog'
path.resolve(1) // TypeError [ERR_INVALID_ARG_TYPE]: The "path" argument must be of type string. Received type number
path.resolve('a','b') // '/Users/lipingzhang/gitProject/blog/a/b'
path.resolve('/temp','new') //  '/temp/new'
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif') // '/Users/lipingzhang/gitProject/blog/wwwroot/static_files/gif/image.gif'
```
- [x] path.join(<string>)
> 使用平台特定分隔符将所有给定 path 片段连接在一起，规范化生成路径
```javascript
path.join() // '.' 表示当前目录
path.join(process.cwd(), 'hexo.json') // '/Users/lipingzhang/Desktop/hexo-cli的副本/a.json'
```
### process
> 全局变量，提供有关当前 Node.js 进程信息，并对其进行控制，不需要 require
#### process - 信号事件
Interrupt from keyboard
SIGINT 在终端运行时，可以被所有平台支持，通常可以通过 <Ctrl>+C 触发(虽然这个不能配置)。 当终端运行在raw模式，它不会被触发。
```javascript
process.on('SIGINT', function() {
  // callback function
});
```
