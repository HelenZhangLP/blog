---
title: NodeJs 学习笔记
date: 2019-05-31 14:32:44
tags:
- JavaScript
- node
---

## NodeJS
Node.js 是一个开源的、跨平台的 JavaScript 运行时环境。

### vm 模块
The node:vm module enables compiling and running code within V8 Virtual Machine context
vm 模块可以在 V8 虚拟机中编译运行代码
```javaScript
vm.runInNewContext(code)
/**
 * code <string> 要运行和编译的 JavaScript 代码（The JavaScript code to compile and run）
 * contextObject <Object> 一个要被上下文化的对象（An object that will be contextified）, 如果未定义，会创建一个新对象（if undefined, a new object will be created.）
 * options <Object> | <string> 
 */
```
---
```javaScript
vm.runInNewContext(code, sandbox, "sea-debug.vm")
```

### module

### http 模块
```JavaScript
// 1、导入 http 模块
const http = require('http')
// 2、创建 Web 服务
const server = http.createSever()
// 3、绑定 request 事件
server.on('request', (req, res) => {
    const url = req.url
    let str = '<h1>Not Found</h1>'
    if (url === '/' || url === '/index.html') {
        str = '<h1>首页</h1>'
    } else if (url === '/about.html') {
        str = '<h1>关于</h1>'
    }

    // 处理响应数据乱码的问题
    res.setHeader('content-type', 'text/html; charset=utf-8')
    // 响应发送给客户端
    res.end(str)
})

//4、 启动服务
server.listen(port, host, () => {
    console.log(`http://${host}:${port} 已启动`)
})
```

### path 模块
> 提供用于处理文件路径和目录路径的实用工具

```JavaScript
const path = require('path')
```

- [x] path.resolve()
> 将路径或片断序列解析为绝对路径
```JavaScript
// 不传参
path.resolve()  // '/Users/lipingzhang/gitProject/blog'
path.resolve(1) // TypeError [ERR_INVALID_ARG_TYPE]: The "path" argument must be of type string. Received type number
path.resolve('a','b') // '/Users/lipingzhang/gitProject/blog/a/b'
path.resolve('/temp','new') //  '/temp/new'
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif') // '/Users/lipingzhang/gitProject/blog/wwwroot/static_files/gif/image.gif'
```
- [x] path.join(<string>)
> 使用平台特定分隔符将所有给定 path 片段连接在一起，规范化生成路径
```JavaScript
path.join() // '.' 表示当前目录
path.join(process.cwd(), 'hexo.json') // '/Users/lipingzhang/Desktop/hexo-cli的副本/a.json'
```
### process
> 全局变量，提供有关当前 Node.js 进程信息，并对其进行控制，不需要 require
#### process - 信号事件
Interrupt from keyboard
SIGINT 在终端运行时，可以被所有平台支持，通常可以通过 <Ctrl>+C 触发(虽然这个不能配置)。 当终端运行在raw模式，它不会被触发。
```JavaScript
process.on('SIGINT', function() {
  // callback function
});
```
