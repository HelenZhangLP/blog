---
title: react-state与生命周期
date: 2021-03-09 09:21:31
tags:
- react
---

React 组件可以看成一个状态机
React 定义框架的目的是权权通过更新 React 组件状态，实现重新沉浸用户界面操作

状态机 —— 组件通过与用户交互，实现不同的状态，然后通过渲染 UI 保证用户界面和数据的一致性

React 状态初始化
```javascript
class ReactStateCom extends React.Component {
  constructor() {
    super(props)  // 将 Props 参数添加到构造方法
    this.state = {params: '状态机'}
  }

  render() {
    return <div>{this.state.params}</div>
  }
}
```
通过 setState() 方法来更新状态（state）
setState() 两个参数， prevState 和 props 分别用于传递前一个 state 和 props 参数


生命周期
React 组件中，生命周期可基本分为三个状态：
Mounting：已开始挂载 DOM
Updating：重新渲染组件 DOM
UnMounting：已卸载真实的 DOM

关于生命周期的方法
ComponentWillMount()：在渲染前调用，可以在客户端，也可以在服务端
ComponentDidMount()：第一次渲染后调用，只作用于客户端
ComponentWillUpdate()：在组件接收到新的 props 参数或者 State 状态，但没有渲染时被调用。**该方法初始化时不会被调用**
ComponentDidUpdate()：组件完成更新后会立即调用，另外该方法。**该方法初始化时不会被调用**
ComponentWillMount()：在组件从 Dom 中移除之前会被立刻调用。

自顶向下数据流
React 组件是被定义为具有状态（state）的

react 条件渲染
设计人员创建不同的组件来封装各种业务需求，然后依据需求的不同状态，渲染对应状态下的局部内容
1. 使用 if 条件判断，分别渲染两种状态下的组件
2. 逻辑与运算符条件渲染

react 创建、转换、使用列表
React 列表中使用 key 帮助 react 框架识别元素的改变，识别一个元素 key 最好是在其所在列表中独一无二
React 特取有效性，key 值权权会传递信息给 React 框架，用户的自定义组件无法读取
React 表单与HTML表单默认行为相同，在 React 环境中执行 HTML 代码仍然有效

HTML 表单是自己管理自己的状态，根据用户输入修改状态。而 React 组件属于受控组件，它的状态由 state 管理，需要改变时，通过 setState 修改其状态
