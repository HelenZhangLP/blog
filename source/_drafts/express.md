---
title: Express
tags:
- express
---

## 认识 `express`
> <span class='custom-box custom-box-933'>基于 Node.js 平台</span><span class='custom-box custom-box-339'>快速、开放、极简</span>的 <span class='custom-box custom-box-933'>Web 开发框架。</span>
是 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法，类似于 NodeJs 中内置的 http 模块，<span class='custom-box custom-box-933'>专门用来创建 Web 服务器的</span>

### 安装 express
```bash
```
### 使用 `express` 创建 web 服务器
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
## 能够使用 `express.static()` 快递托管静态资源
## `express` 路由
## 能够使用 `express` 路由精简项目结构
## `express` 中间件
## 能够使用常见的 `express` 中间件
## 使用 `express` 写接口
## 能够使用 `express` 创建 API 接口
## 能够使用 `express` 中启用 cors 跨蹃资源共享
