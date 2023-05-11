---
title: React Props
tags:
  - React
date: 2023-05-08 23:12:52
---


"Render Props" 是一种在 react 组件之间<u>使用一个值为函数的 prop 共享代码</u>的简单技术

## 定义函数组件
```javascript
function demo1(props) {
    return <>
        <h3>这是一个函数组件</h3>
    </>
}

export default demo1
```
## 调用函数组件
入口 index.js 调用组件
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import ComponentDemo from '@/src/ComponentDemo'
b
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(<><ComponentDemo title="A" data="b" /></>)
```

## 修改 props 属性
### props 不能通过 props.attr = 'attribute' 修改
> 因为 props 被冻结，不能通过 props.attr = 'attribute' 修改、扩展
```javascript
function demo1(props) {
    // props.title = 'modify props attribute'
    /**
     * {
            "value": "function component demo",
            "writable": false,
            "enumerable": true,
            "configurable": false
        }
     */
    console.log(Object.getOwnPropertyDescriptor(props, 'title'))
    console.log(Object.isFrozen(props)) // true
    console.log(Object.isExtensible(props)) // false
    console.log(Object.isSealed(props)) // true
    return <>
        <h3>这是一个函数组件</h3>
    </>
}

export default demo1
```
## props 修改方法
> let a = props.a; a = 'b'
```javascript
function demo1(props) {
    let a = props.a; a = 'b'
    return <>
        <h3>这是一个函数组件{b}</h3>
    </>
}

export default demo1
```

## 设置 props 默认值
```javascript
function demo1(props) {
    const {c} = props
    return <>
        <h3>这是一个函数组件{c}</h3>
    </>
}

// 设置默认值，设置数据格式，必传项等
demo1.defaultProps = {
    c: 'test default attribute'
}

export default demo1
```
## 设置 props 检验
```javascript
function demo1(props) {
    return <>
        <h3>这是一个函数组件</h3>
    </>
}

// 设置默认值，设置数据格式，必传项等
demo1.propsType = {
    title: PropTypes.string.isRequired,
    data: PropTypes.oneOfType([
        PropTypes.number,
        PropTypes.string
    ])
}

export default demo1
```

## 使用 Render Props 来解决横切关注点(Cross Cutting Concerns)
如何将一个组件封装的状态或行为共享给其他需要相同状态的组件

render prop 是一个用于告知组件需要渲染什么内容的函数 prop

## 使用 props 而非 render
render prop 是因为模式才被称为 render prop，<u>`任何被用于告知组件需要渲染什么内容的函数 prop 在技术上都可以被 称为 render prop`</u>
