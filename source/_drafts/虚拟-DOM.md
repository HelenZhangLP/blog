---
title: Virtual DOM
tags:
---

## 操作 DOM（Document Object Model） 的几种方式
### JavaScript 原生获取 DOM
```JavaScript
document.getElementById('id')
document.querySelector('#id')
document.getElementsByName('user')
```
[更多 Javascript 获取 DOM方式](/2021/06/11/JavaScript-Docment-Object-Model/)

### jQuery 获取 DOM
```JavaScript
$('#id')
$('.class')
$('div')
```

> 以上两种方式都是直接操作 DOM 元素达到视图更新的效果

## Vue 更新视图的方式
```Vue
<template>
  <div id="app">
    <h1>name: {{name}}</h1>
  </div>
</template>

<scirpt>
  export default {
    data() {
      return {
        name: 'helen'
      }
    },
    methods: {
      changeName() {
        this.name = 'zhangLP'
      }
    }
  }
</scirpt>
```
> Vue 改变数据 `this.name = 'zhangLP'` 触发视力改变