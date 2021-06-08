---
title: JavaScript-对象
date: 2019-08-28T10:42:54.000Z
tags:
  - JavaScript
---

今天遇到了一个小问题，'blur' 事件失焦时，vue 并没有把接口请求回来的data回填到页面。 现在来分析分析：

- [ ] blur 的锅，no ，it isn't！因为吧，失焦事件的回调函数执行了；
- [x] 异步请求回调成功后赋值失败，打完断点发现吧，没这回事。赋值成功。 嘿嘿，不卖关子了！上代码

<!-- more -->

 ```html
<template>
  <el-form-item label="姓名：" prop="name">
    <el-input v-model="form.name" @input="handlerAccount" @blur="checkSame" />
  </el-form-item>
</template>
```

```javascript
data() {
  return {
    form: {},
  }
}

methods: {
  checkSame() {
    this.form.account = 'test1';
  }
}
```

> 这里先简单的说说，vue 实现双像绑定用的是 Obeject.defineProperty() 来监听属性变动， 需要监听的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter和getter，data 中的 form 没有 account 属性，checkSame 方法直接给 form 添加新属性，通过 `this.form.account` 添加的属性没有 setter 和 setter和getter 方法，不具备监听。所以双向绑定也就无从谈起了。修改方法有以下两种：

--------------------------------------------------------------------------------

--------------------------------------------------------------------------------

- 修改方式 -- 初始化每个需要双向绑定的属性及对象属性

```javascript
// 初始化每个需要双向绑定的属性及对象属性
data() {
  return {
    form: {
      account: ""
    },
  }
}

methods: {
  checkSame() {
    this.form.account = 'test1';
  }
}
```

- 通过 Object.assign 给 form 添加新属性

```javascript
// 通过 Object.assign 给 form 添加新属性
data() {
  return {
    form: {
      account: ""
    },
  }
}

methods: {
  checkSame() {
    this.form = Object.assign({}, {
      account: 'test1'
    })
  }
}
```
