---
title: 我所见过的 npm 库
date: 2019-07-24T15:55:16.000Z
tags:
- npm
---

# postcss-pxtorem

# dotenv

NodeJs 运行时加载不同的配置，`process.env.DB_HOST` 获取环境变量，程序启动时，从文件加载环境变量时就需要用到 dotenv 库。

## 用法 创建 .env

```shell
# .env
DB_HOST = localhost
DB_USER = root
DB_PASS = 123456
```

NODE 中运行

```javascript
const dotenv = require('dotenv')
dotenv.config()

// 程序中使用环境变量
const db = require('db')
db.connect({
  host: process.env.DB_HOST,
  username: process.env.DB_USER,
  password: process.env.DB_PASS
})
```

# debug 专门控制日志输出的库

判断 DEBUG 环境变量，调整运行环境控制日志是否输出。DEBUG 对环境变量进行解析，允许我们选择性的控制输出哪些日志模块，解决控制台日志堆积的问题。
