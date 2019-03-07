---
title: vue.use 与 vue.component
date: 2019-02-25 15:54:14
categories:
- 技术
tags:
- 前端框架
- Vue
---

Error - 1 弹层对象取自列表，列表数据在弹层编辑修改，编辑部分数据后，取消或关闭弹层，列表数据修改。
产生原因：vue 双向绑定
解决办法：copy Object, 对象重新 copy 一份
```javascript
handlerUpdate(item) {
  // 更新 item
  this.dialogVisible = !this.dialogVisible
  this.dialogStatus = 'edit'
  this.temp = Object.assign({}, item) // copy obj
  this.$nextTick(() => {
    this.$refs['dataForm'].clearValidate()
  })
},
```
``` javascript
const vm: Component = this
```
以上写是不是有点奇怪，是的。我没见过，它是 [flow.js](https://flow.org/)
