---
title: redux
date: 2019-03-11 10:16:53
categories:
- 技术
tags:
- redux
---
哈哈，请问你是要闹哪样？
`import { connect } from '@tarojs/redux';`
redux 文档里查了没找着，原来是 tarojs 的方法，借机认识下这哥们。
### connect
> tarojs/redux 提供的 connect 方法将 redux 与我们的页面进行连接
connect 方法接受两个参数
`mapStateToProps` 与 `mapDispatchToProps`
* mapStateToProps，函数类型，接受最新的 state 作为参数，用于将 state 映射到组件的 props
* mapDispatchToProps，函数类型，接收 dispatch() 方法并返回期望注入到展示组件的 props 中的回调方法

#### Demo
```Javascript
import { connect } from '@tarojs/redux';
function mapStateToProps(state) {
  return {
    reducer: state.bindphoneReducer,
  };
}

@connect(mapStateToProps)
```

Javascript 状态容器
### connect
