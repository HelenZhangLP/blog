---
title: react + fetch + mockjs
date: 2020-12-02 13:23:44
tags:
- React
- mockjs
---

## 环境准备
1. `mockjs` 生成随机数据、拦截 Ajax 请求。[文档见](https://github.com/nuysoft/Mock/wiki/Getting-Started)
  * 安装
  > npm install --save-dev mockjs

2.  `fetch` http 请求方式，是 XMLHttpRequest 的一种替代方案，它的 api 是基于 Promise 设计的 nodejs 中使用需要引入 node-fetch.
3.  `fetch-mock` 拦截 fetch 请求
  * 安装
  > npm install --save-dev fetch-mock
<!--more-->
## 搭建
1.  目录结构
{%plantuml%}
salt
{
  {T
    + index.js
    + app.js
    + api
      ++ index.js
    + mock
      ++ index.js
      ++ mock.js
      ++ data
        +++ agentList.js
  }
}
{%endplantuml%}
按照以上目录结构整理代码，以下是 demo
* `/index.js`
```javascript
import mock from './mock/index'
mock.start();
```
* `/mock/index.js`
```javascript
import mock from './mock'
export default mock
```
* `/mock/mock.js`
```javascript
import fetchMock from 'fetch-mock';
import {agents} from './data/agentList'

export default {
  start() {
    fetch.get('/agentList', ()=>{
      return agents
    })
  }
}
```
* data 中 mock 假数据 `/mock/data/index.js`
```javascript
import mock from 'mockjs';
const Random = mock.Random;
let agents = []
for (let i=0; i<10; i++) {
  agents.push(Mock.mock{
    key: Random.guid()
  })
}
export { agents }
```
* fetch 调用 api `/api/agentList.js`
```javascript
export const getAgentList = () => {
  return fetch('/agentList').then(response => {
    return response.json();
  })
}
```

* 组件中调用 `/app.js`
```javascript
// 省略其它代码
componentDitMount() {
  getAgentList().then(data => {
    this.setState({
      data
    })
  })
}
```
