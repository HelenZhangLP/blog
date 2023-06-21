---
title: React 高阶组件
tags:
- react
---

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
