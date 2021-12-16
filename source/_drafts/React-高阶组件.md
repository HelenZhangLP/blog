---
title: React 高阶组件
tags:
- react
---

### React.PureComponent `数据比较`
> 浅层对比 prop 和 state 的方式来实现 shouldComponentUpdate 方法 <br />
> 在深层数据结构发生变化时调用 forceUpdate() 确保组件被正确地更新 <br />
> 使用 immutable 对象加速嵌套数据对比

### React.Fragment
> 不额外创建 DOM 元素的情况下， `render()` 方法中返回多个元素
```javascript
render() {
  return (
    <Fragment>
      <p>1</p>
      <h1>2</h1>
    </Fragment>
  )
}

// React v16.2.0
render() {
  return (
    <>
      <p>1</p>
      <h1>2</h1>
    </>
  )
}
```
高阶组件是一种设计模式
是一种基于 React 的组合特性而形成的设计模式
HOC 自身不是 React API 的一部分
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧

**参数为组件，返回组件为新组件的函数**
> const EnhancedComponent = higherOrderComponent(WrappedComponent)

高阶组件是将组件转换为另一个组件

为什么高阶组件有用
如何编写自己的 HOC 函数

高阶组件和函数子组件都是设计模式
可以实现更多场景的组件利用

## 使用 HOC 解决横切关注点问题（Cross Cutting Concerns）
