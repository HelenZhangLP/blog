---
title: Render Props
tags:
- react
---

"Render Props" 是一种在 react 组件之间<u>使用一个值为函数的 prop 共享代码</u>的简单技术

为什么 render Prop 是有用的
如何自己写一个 render Props

## 使用 Render Props 来解决横切关注点(Cross Cutting Concerns)
如何将一个组件封装的状态或行为共享给其他需要相同状态的组件

render prop 是一个用于告知组件需要渲染什么内容的函数 prop

## 使用 props 而非 render
render prop 是因为模式才被称为 render prop，<u>`任何被用于告知组件需要渲染什么内容的函数 prop 在技术上都可以被 称为 render prop`</u>
