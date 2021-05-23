---
title: react 生命周期
tags:
- react
---

React 组件中，生命周期可基本分为三个状态

### Mounting 组件的挂载
```mermaid
graph LR
A(React 组件渲染) --> B(构造DOM元素)
B --> C(展示到页面)
```
<!--more-->
如上过程称为**组件挂载**
执行的相关生命周期方法：
1.  getDerivedStateFromProps

Mounting：已开始挂载 DOM
Updating：重新渲染组件 DOM
UnMounting：已卸载真实的 DOM

关于生命周期的方法
ComponentWillMount()：在渲染前调用，可以在客户端，也可以在服务端
ComponentDidMount()：第一次渲染后调用，只作用于客户端
ComponentWillUpdate()/UNSAFE_componentWillUpdate
<font color="Orange">
  `componentWillUpdate has been renamed，and is not recommended for use
  Move data fetching code or side effects to ComponentDidUpdate
  Rename componentWillUpdate to UNSAFE_componentWillUpdate to suppress this warning in non-strict mode，In React 18.x, only the UNSAFE_ name will work. to rename all deprecated lifecycles to their new names, you can run 'npx react-codemod rename-unsafe-lifecycles' in your project source folder.`
</font>
> 在组件接收到新的 props 参数或者 State 状态，<u>渲染之前调用</u>。**该方法初始化时不会被调用**
  ComponentWillUpdate 未来会废弃
  平缓过渡 React 16.3 版本中，为不安全的生命周期引入别名 UNSAFE_componentWillUpdate
  废弃旧版本的生命周期保留至 React 17

UNSAFE_componentWillUpdate + getDerivedStateFromProps
<font color="red">
  `Unsafe legacy lifecycles will not be called for components using new components APIs
  App uses getDerivedsTATEfromProps() but also contains the following legacys UNSAFE_componentWillUpdate`
</font>

getDerivedStateFromProps()
><u>在组件实例化及接收到新的 props 后调用</u>，返回一个对象去更新 state，或者返回 null 不更新，<u>用于确认当前组件**是否需要重新渲染**</u>这个生命周期将作为 componentWillReceiveProps() 的安全替代者

getSnapshotBeforeUpdate()
><u>更新之前被调用</u>**这个生命周期返回值将作为第3个参数传递给 componentDidUpdate() 方法**<font color="red">对于保存滚动位置有用</font>

<font color="red">Warning: App: getSnapshotBeforeUpdate should be used with ComponentDidUpdate(). This component defines getSnapshotBeforeUpdate() only</font>

ComponentDidUpdate()：组件完成更新后会立即调用，另外该方法。**该方法初始化时不会被调用**
ComponentWillMount()：在组件从 Dom 中移除之前会被立刻调用。
