---
title: redux
date: 2019-03-11 10:16:53
categories:
- 技术
tags:
- redux
---

## Redux 理解
Redux 设计之初是为了更好的维护 Web 状态，React 框架实现了 State。React + Redux
Redux 设计的核心是 store 的概念。store 将 State/Action/Reducer 联系在一起。<font color="#911" face="黑体" size="3">`redux 中有且只有一个 store，store 是唯一的`</font>
Action 是负责把数据从应用传到 Store 的载体，是 Store 数据的唯一来源，通过 store 的 dispatch() 方法实现
Reducer 将 State 的变化和相应的 Action 发送到 store，然后返回新的 state 给 store
Reducer 本质上一个纯函数，接收 state and action，执行相应操作后，返回新的 state。但切忌做以下操作：
* 修改传入参数；
* 执行有副作用的操作，如请求 API 和 路由跳转等；
* 调用非纯函数，如 Date.now()、Math.random() 等不确定值操作

> Redux 负责状态管理、React 负责视图渲染，Redux 中 store 是核心对象，在管理 Reducer 与 State 的同时，还要处理 Dispath 存入的 Action，最终订阅 React 视图更新。

{%plantuml%}
component “React 组件” as ReactComponent #7e9;line.dashed;line:green
rectangle "Redux" as outer #fff;line.dashed {
  card Action #aliceblue;line.dashed;line:blue;text:blue;
  rectangle "Store" as inner #fff;line.dashed {
    card Reducer #aliceblue;line.dashed;line:blue;text:blue;
    card State #aliceblue;line.dashed;line:blue;text:blue;
  }

  Action --> Reducer #black;text:red : action.type
  Reducer --> State #black;text:red : return new state
}
ReactComponent --> Action #black;text:red : Dispatch
State --> ReactComponent #black;text:red : Update
{%endplantuml%}
<!--more-->

> 1. Redux 其实是一个 `Javascript 状态容器`，可预测状态管理
  2. Redux 并不属于 React 框架，完全独立。可与大多数前端框架完美结合
  3. /**
       * reducer(state, action){}
       * state 状态，action 用户操作 action.type 用户操作类型
       * store 通过 Redux.createStore(reducer)
       * store.subscribe(render) 实现用户订阅功能，当状态发生变化时，自动渲染视图 render 通过 innerHTML 实现
       * 操作不同，dispatch 触发不同的 action {type: 'ACTIONTYPE'}

## demo
```html
<script type="text/babel">
  var root = document.querySelector('#root')

  // TODO: define reducer
  function reducerCounter(state, action) {
    if (typeof state === 'undefined') return 0
    switch (action.type) {
      case 'INCREMENT':
        return state + 1
        break;
      case 'DECREMENT':
        return state < 1 ? 0 : state - 1
      default:
        return state
    }
  }

  // TODO: create store
  var store = Redux.createStore(reducerCounter)
  var counterNumber = document.getElementById('count-number')

  // TODO: render
  function render() {
    counterNumber.innerHTML = store.getState().toString()
  }

  render()
  store.subscribe(render) //监听一些变化

  // TODO: dispatch
  document.getElementById('increment').addEventListener('click', function() {
    store.dispatch({type: 'INCREMENT'})
  })

  document.getElementById('decrement').addEventListener('click', function() {
    store.dispatch({type: 'DECREMENT'})
  })

  document.getElementById('incrementIfOdd').addEventListener('click', function() {
    if (store.getState() % 2 !== 0) {
      store.dispatch({type: 'INCREMENT'})
    }
  })

  document.getElementById('incrementAsync').addEventListener('click', function() {
    setTimeout(function(){
      store.dispatch({type: 'INCREMENT'})
    },1000)
  })
</script>
```

## React-Redux 中的 Provider 容器组件
传递参数多的时采用 React-Redux 库的 Provider 组件容器解决，传递太多参数造成的性能问题，同时还提供了一个 connect 方法来连接与 Store 对象
{%plantuml%}
component "React 组件" as ReactComponent #7e9;line.dashed;line:green
rectangle "Redux" as outer #fff;line.dashed {
  card "Action" #aliceblue;line.dashed;line:blue;text:blue
  rectangle "Store" as inner #fff;line.dashed {
    card "Reducer" #aliceblue;line.dashed;line:blue;text:blue
    card "State" #aliceblue;line.dashed;line:blue;text:blue
  }
  Action --> Reducer #black;text:red : action.type
  Reducer --> State #black;text:red : return new state
}
ReactComponent --> Action #black;text:red : mapDispatchToProps
State --> ReactComponent #black;text:red : mapStateToProps
{%endplantuml%}
Provider 组件将 Store 对象包装在顶层容器中，就可以被其子组件继承。

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
