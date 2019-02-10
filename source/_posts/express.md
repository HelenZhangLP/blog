---
title: express
date: 2019-02-06 11:27:34
categories: 技术
tags:
- Node
- express
---

```javascript
<%- partial('_partial/ocean') %>
```
ejs 引发的一系列问题
`partial()` 是 express 框架方法，先了解下 express。
[expressjs 官网](http://www.expressjs.com.cn/)是这样介绍的，它是 Web 应用程序
Express 是一个保持最小规模的灵活的 Node.js Web 应用程序开发框架，为 Web 和移动应用程序提供一组强大的功能。
在官网中没有找到 `partial()` 方法，只能看看野史了。
百度找到了[NodeJS - Express 3.0下ejs模板使用 partial展现 片段视图](http://yijiebuyi.com/blog/e503a402ffac43ca1cbaba9d4317b54d.html)
<!-- more -->
[github 发现](https://github.com/expressjs/express/wiki/Migrating-from-2.x-to-3.x)

```javascript
<%- partial('_partial/ocean') %> // 在 3.x 中该方法已经移除
<%- include '_partial/ocean' %> // 用 include 替代
```
