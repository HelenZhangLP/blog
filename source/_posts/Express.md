---
title: Express
tags:
  - WebServer
  - node
  - Express
date: 2023-06-08 11:38:09
---



[Express 中文网](expressjs.com.cn)，是用于开发 <span class='custom-box custom-box-393'>Web 网站服务器</span> 与 <span class='custom-box custom-box-393'>API 接口服务器</span>
## 认识 `express`
> <span class='custom-box custom-box-933'>基于 Node.js 平台</span><span class='custom-box custom-box-339'>快速、开放、极简</span>的 <span class='custom-box custom-box-933'>Web 开发框架。</span>
是 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法，类似于 NodeJs 中内置的 http 模块，<span class='custom-box custom-box-933'>专门用来创建 Web 服务器的</span>

### 安装 express
```bash
$ npm i express@4.17.1
```

### 使用 `express` 创建 web 服务器
```mermaid
 flowchart LR
 s1[导入] --> s2[创建 Web 服务器]
 s2 --> s3[启动 Web 服务器]
```
---
```JavaScript
// 1. 导入 express
const express = require('express')
// 2. 创建 web 服务器
const app = express()
// 3. 启动 web 服务
express.listen(80, ()=>{
    console.log('express server running at http://127.0.0.1')
})
```
### Express 的基本使用
#### 1. 监听 GET 请求 
通过 app.get(url, function(req, res){}) 方法，监听客户端的 GET 请求
```JavaScript
...
/**
 * 参数1：客户端请求的 URL 地址
 * 参数2：请求对应的处理函数
 *  req: 请求对象
 *  res: 响应对象
 */ 
app.get('请求 URL', function(req, res){})
...
```
#### 2. 监听 POST 请求
```JavaScript
/**
 * 参数1：客户端请求 URL
 * 参数2：请求对应的处理函数
 *  req: 请求对象
 *  res: 响应对象
 */ 
app.post('请求 URL', function(req, res) {})
```
#### 3. SEND 将处理好的内容发送给客户端
```JavaScript
app.get('/user/:id', (req, res) => {
    res.send({...}) // 向客户端发送请求内容
})
app.post('/user', (req, res) => {
    res.send('success') // 向客户端发送文本信息
})
```

#### 4. 请求参数
<span class='custom-box custom-box-393'>查询参数</span> 通过 req.query 获取客户端（/user/age=15&gender=male）
<span class='custom-box custom-box-393'>查询动态参数</span>  通过 req.params 获取客户端（/user/1），动态参数可以是多个

```JavaScript
// 注意 url 参数要加 ?(?age=15&gender=male) 【clientUrl http://127.0.0.1:9000/users/?age=15&gender=male】
app.get('/users/?age=15&gender=male', (req, res) => {
    console.log(req.query, 'req.query') // { age: '15', gender: 'male' } req.query
    res.send({id: 1})
})

// req.params 获取动态参数 /user/:id 【clientUrl http://127.0.0.1:9000/user/1】
app.get('/user/:id/:name', (req, res) => {
    console.log(req.params, 'req.params')
    res.send({id:1})
})
```
## 能够使用 `express.static()` 快递托管静态资源
<span class='custom-box custom-box-393'>express.static() 用于创建静态资源服务器</span> 

```JavaScript
...
// 可以将 public 目录下的图片、CSS文件、JavaScript 文件对外开放访问
app.use(express.static('public'))
...
```
### 托管多个静态资源
多次调用 `express.static()` 函数，托管多个静态资源目录。
<span class='custom-box custom-box-933'>访问静态资源时，多个目录中有相同名称的资源时，`express.static()` 函数会根据目录的添加顺序查找所需的文件</span>

```JavaScript
...
app.use(express.static('public'))
app.use(express.static('files'))
```
### 挂载路径前缀
在托管的静态资源访问路径前 <span class='custom-box custom-box-393'>挂载路径前缀</span>，避免因挂载顺序问题，导致不能访问到指定资源的问题。
```JavaScript
...
app.use('/public',express.static('public'))
```
### nodemon
<span class='custom-box custom-box-933'>问题：</span>在编写 `node.js` 项目时，修改项目代码后，需要 `ctrl+c` 停止项目，再重启。
[nodemon](https://www.npmjs.com/package/nodemon)工具，可以监听项目变化，项目代码修改后，nodemon 自动重启项目，方便开发调试。
```bash
$ npm install -g nodemon
```

### pm2
服务持久化管理的 <span class='custom-box custom-box-939'>即使终端关闭，服务器也在，电脑重启后，服务器关闭。</span>
```bash
$ npm i pm2 -g #安装 pm2
$ pm2 start server.js --name TASK #启动
$ pm2 restart TASK #重启
$ pm2 stop TASK #暂停
$ pm2 delete TASK #删除
```
## `express` 路由
路由指的是 <span class='custom-box custom-box-393'>映射关系</span>，在 Express 中，路由指的是 <span class='custom-box custom-box-393'>客户端请求与服务器处理函数的**<u>映射关系</u>**</span>
Express 中路由由 <span class='custom-box custom-box-393'>请求类型(METHOD:POST/GET),请求 URL 地址（PATH）, 处理函数（HANDLE）</span>组成。
```JavaScript
...
app.METHOD(PATH, HANDLE)
...
```
### 路由的匹配过程
当请求到服务器之后，需要经过路由的匹配，匹配成功后，才会调用对应的处理函数
匹配时，按照路由的顺序进行，请求类型与请求 URL 匹配成功，Express 将请求转交给 handle 函数。

```mermaid
 flowchart LR
 start((客户端请求))
 subgraph app
  req1["app.get('/', fn1)"]
  req2["app.get('/', fn2)"]
  req3["app.get('/', fn3)"]
  reqMore["更多..."]
 end
 subgraph 处理函数
  fn1["fn1(req, res){// 处理函数}"]
  fn2["fn2(req, res){// 处理函数}"]
  fn3["fn3(req, res){// 处理函数}"]

  req1 -->|分发| fn1
  req2 -->|分发| fn2
  req3 -->|分发| fn3
 end
 start --> app
```
### 模块化路由
为了方便对路由进行模块化管理，<span class='custom-box custom-box-933'>Express 不建议将路由直接挂载在 app 上，推荐将路由抽离为单独的模块</span>
#### 创建路由模块
```mermaid
 flowchart LR
 subgraph routerModulejs
 step1[导入 express] --> step2[创建路由对象]
 step2 --> step3[挂载路由]
 step3 --> step4[导出路由]
 end
 subgraph indexjs
 step5[导入路由模块] --> step6[注册路由模块]
 end
 step4 --> step5
```
```JavaScript
// routerModule.js
// 1. 导入 express
// 2. 创建路由对象
// 3. 挂载路由
// 4. 导出路由 
```
#### 使用路由模块
```JavaScript
// index.js
...
const router = require('./routerModule.js')
app.use('/api', router) // 注册路由模块
...
```
## 能够使用 `express` 路由精简项目结构
## `express` 中间件
### 什么是中间件
中间件（Middleware），业务流程的**中间处理环节**。
### 中间件的处理流程
当一个请求到达 Express 服务器之后，可以连续调用多个中间件，从而 <span class='custom-box custom-box-393'>对这次请求进行<u>预处理</u></span>。
```mermaid
 flowchart LR
 client((客户端))
 subgraph Express服务器
  middleware1([中间件1]) -->|"next()"| middleware2([中间件2])
  middleware2 -->|"next()"| middleware3([中间件3])
  middleware3 -->|"next()"| middleware4([中间件...])
  middleware4 -->|"next()"| ending[["<b>路由</b><br/>处理完毕，响应这次请求"]]
 end
 client -->|请求| middleware1
 ending -->|响应| client
```
### Express 中间件的格式
Express 中间件，<span class='custom-box custom-box-393'>本质上是一个带有 `next` 参数的 funtion 处理函数</span>
#### 定义一个中间件
```JavaScript
var express = require("express")
var app = express()

/**
 * 挂载中间件
 * app.get http method for which the middleware function applies
 * '/' Path(route) for which the middleware function applies
 * function(req, res, next){} the middleware function
 *  req http request argument to the middleware function,called 'req' by convention
 *  res http response argument to the middleware function, called 'res' by convention
 *  next callback argument to the middleware funtion, called 'next' by convention
 */ 
app.get('/', function(req, res, next){
    // next 函数是实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件或路由
    next()
})
```
<span class='custom-box custom-box-933'>中间件函数的形参列表中，必须包含一个 next 参数。</span>路由处理函数中只包含 req 和 res。

### 全局生效的中间件
客户端发起的任何请求，到达服务器之后，都会触发的中间件，叫做全局生效的中间件。
<span class='custom-box custom-box-393'>通过 `app.use()` 中间件函数，可定义一个全局生效的中间件</span>

```JavaScript
    // 定义中间件函数
    cont middleware = (req, res, next) {
        // 流转关系转交给下一个中间件或路由
        next()
    }
    // 注册全局生效的中间件函数
    app.use(middleware)
```
中间件简化形式
```JavaScript
// 全局中间件的简化形式
app.use((req, res, next) => {
    next()
})
```
#### 中间件的作用
多个中间件之间，共享同一份 req 和 res。基于这个特性，我们可以在上流的中间件中，统一为 req 或 res 对象添加自定义的属性或方法，供下游的中间件或路由进行使用。
```JavaScript
// 中间件与路由之间共享 req/res 实例
app.use((req, res, next)=>{
    req.receivedTime = Date.now()
    next()
})

app.get('/', (req, res) => {
    console.log('请求到达服务器的时间：', req.receivedTime)
})
```
### 定义多个中间件
```JavaScript
 app.use((res, req, next) => {console.log('第一个中间件')})
 app.use((res, req, next) => {console.log('第二个中间件')})
 app.get('/', (req, reqs) => {console.log('请求路由')})
 // 第一个中间件 --> 第二个中间件 --> 请求路由
```
### 局部生效的中间件
> 不使用 app.use() 定义的中间件为局部生效中间件。
```JavaScript
const middleware = function(req, res, next) {
    console.log(`this is a locally effective middleware`)
    next()
}

app.get('/', middleware, function(req, res) {
    res.send('Index')
})
```
### 多个局部生效的中间件
```JavaScript
    const middleware1 = function(req, res, next) {
        console.log(`this is the first locally effective middleware`)
        next()
    }
    const middleware2 = function(req, res, next) {
        console.log(`this is the second locally effective middleware`)
        next()
    }

    /**
     * 以下两种写法都可以使用局部中间件
     */ 
    app.get('/', middleware1, middleware2, function(req, res) {
        console.log('Index')
    })
    app.get('/users', [middleware1, middleware2], function(req, res) {
        console.log('Index')
    })
```
### 中间件使用注意事项
*   一定要在路由之前注册中间件；
*   客户端发送过来的请求，可以连续调用多个中间件进行处理；
*   执行完中间件的业务代码后，不要忘记调用 next() 函数；
*   为了防止代码逻辑混乱，调用 next() 函数之后不要再写其它代码；
*   连续调用多个中间件时，多个中间件之间，共享 req 和 res;

### 中间件的分类
错误级别中间件的作用：捕获整个项目中发生的异常错误，从而防止项目异常崩溃。
> 错误级别中间件的 function 中，<span class='custom-box custom-box-339'>必须包含4个形参，</span>分别是 err,req,res,next 

<span class='custom-box custom-box-933'>错误级别中间件必须在所有路由之后注册</span>

```JavaScript
// 1 导入 express 模块
const express = require('express')
// 2 创建 express 服务器实例
const server = express()

// 4 定义路由
server.get('/', (req, res) => {
    // 定义错误
    throw new Error('服务器错误')
    res.send('响应请求')
})

// 5 中件间捕获整个项目中发生的异常错误，从而防止项目异常崩溃。
server.use((err, req, res, next) => {
    console.log('错误：' + err.message)
    res.send('Error：' + err.message )
})

// 3 调用 app.listen 方法，指定端口号启动 web 服务器
server.listen(8090, function() {
    console.log(`Express server running at http://127.0.0.1:8090`)
})
```
### Express 内置中间件
Express 4.16.0 版本开始，Express 内置了 3 个常用中间件
*   `express.static` 快速托管静态资源（html,css,图片等）的内置中间件；
*   `express.json` 解析 JSON 格式的请求体数据（4.16.0+ 可用）[demo for express.json](https://github.com/HelenZhangLP/demo/blob/master/node/src/demo9/index.js)
*   `express.urlencoded` 解析 URL-encoded 格式的请求体数据（4.16.0+ 可用）[demo for express.urlencoded](https://github.com/HelenZhangLP/demo/blob/master/node/src/demo10/index.js)
```JavaScript
// 配置解析 application/json 格式数据的内置中间件
app.use(express.json())
// 解析 application/x-www/form-urlencoded 格式数据中间件
app.use(express.urlencoded({extended: false}))
```

这些中间件，极大的提高了 Express 项目的开发效率及体验。

#### 第三方中间件 body-parser
第三方中间件可按需下载配置，从而提高开发效率。
body-parser 用于解析请求体数据，在 express@4.16.0 之前经常使用
```mermaid
 flowchart LR
 install[npm install body-parser] --> require[导入中间件 body-parser]
 require --> use["app.use() 注册使用中间件"]
```
[demo for body-parser](https://github.com/HelenZhangLP/demo/blob/master/node/src/demo11/index.js)

### 自定义中间件
<span class='custom-box custom-box-939'>手动模拟 express.urlencoded 中间件，解析 POST 提交到服务器的表单数据</span>
```mermaid
 flowchart TB
 step1[定义中间件] --> step2[监听req的data事件]
 step3[监听req的end事件] -->|使用 qureystring | step4[模块解析请求体数据]
 step5[将解析的数据对象挂载到 req.body] --> step6[封装自定义对象为模块]
 step2 --> step3
 step4 --> step5
```
---

```JavaScript
// bodyparse.js
// 自定义中间件
const qs = require('qureystring')
function middleware(req, res, next) {
    // 监听 req 对象的 data 事件，获取客户端发送到服务器的数据
    // 数据量大时，无法一次性发送完毕，客户端会进行数据切割，分批发送到服务器。req 的 data 事件可能会触发多次，需要每次触发 data 事件时，获取到的数据只是完整数据的一部分，需要手动拼接接收的数据

    let str = '' // 存储客户端发送过来的请求体
    // 监听 req 的 data 事件，拼接客户端发送回来的数据
    req.on('data', (chunk) => {
        str += chunk
    })

    // 监听 req 的 end 事件；TODO：把字符串格式的请求体数据，解析成对象格式
    req.on('end', () => {
        const body = qs.parse(str)
        req.body = body;
        next()
    })
}
module.exports = middleware

// ---------- next page ----------
// index.js
const bodyparse = require('./bodyparse')
// 注册使用中间件
app.use(bodyparse)
```

### 跨域中间件
```mermaid
 flowchart LR
 install[安装 npm install cors] --> require["require('cors')"]
 require --> registeration["app.use(cors())"]
```