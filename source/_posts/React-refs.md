---
title: React-refs
tags:
  - React
  - Component
date: 2023-05-11 20:46:13
---


## 受控组件
基于修改数据/状态，让视图更新，达到需要的效果

## 非受挫组件
基于 `ref` 获取 DOM 元素，操作 DOM 实现需求和效果
### 获取 DOM —— `<h2 ref='title'>Refs 非受控组件</h2>`
<span class='custom-box custom-box-933'>“refs”已弃用。ts(6385)</span>
<span class='custom-box custom-box-339'>React.StrictMode 下</span><font color='red'>react-dom.development.js:86 Warning: A string ref, "title", has been found within a strict mode tree. String refs are a source of potential bugs and should be avoided. We recommend using useRef() or createRef() instead.</font>
<span class='custom-box custom-box-339'>原理：</span>react 私有属性 refs 新增属性 refs: {title: h2}

```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    componentDidMount() {
        console.log(this.refs.title) // <h2>Refs 非受控组件</h2>
        this.refs.title.style.backgroundColor = '#62ab00'

    }

    render() {
        return <>
            <h2 ref='title'>Refs 非受控组件</h2>
        </>
    }
}
```
### 获取 DOM —— `<h2 ref={x => this.refFn = x }>Ref 赋值函数形式定义非受控组件</h2>`
<span class='custom-box custom-box-339'>原理：</span>创建一个实例属性 refFn，DOM 元素直接挂载实例属性 reffn 上 。

```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    componentDidMount() {
        console.log(this.refs.title) // <h2 ref='title'>Refs 非受控组件</h2>
        this.refs.title.style.backgroundColor = '#62ab00'
        console.log(this.refFn)
        this.refFn.style.backgroundColor = '#668999'
    }

    render() {
        return <>
            <h2 ref='title'>Refs 非受控组件</h2>
            <h2 ref={x => this.refFn = x}>ref 设置为一个函数</h2>
            <h2>
        </>
    }
}
```
### 获取 DOM —— `React.createRef()`
```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    cef = React.createRef() // 创建 ref

    componentDidMount() {
        console.log(this.refs.title) // <h2 style="background-color: rgb(98, 171, 0);">Refs 非受控组件</h2>
        this.refs.title.style.backgroundColor = '#62ab00'
        console.log(this.refFn) // <h2 style="background-color: rgb(102, 137, 153);">ref 设置为一个函数</h2>
        this.refFn.style.backgroundColor = '#668999'
        console.log(this.cef) // cef : {current: h2}
        this.cef.current.style.backgroundColor = '#ff9923'
        console.log(this)
    }

    render() {
        return <>
            <h2 ref='title'>Refs 非受控组件</h2>
            <h2 ref={x => this.refFn = x}>ref 设置为一个函数</h2>
            <h2 ref={this.cef}>React.createRef 创建非受控组件</h2>
        </>
    }
}
```

## 给类组件设置 ref
获取当前调用组件创建的实例，后面可以根据实例获取子组件中的相关信息
```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    cef = React.createRef() // 创建 ref

    componentDidMount() {
        console.log(this.Child) // 获取到的是子组件 <Child /> 实例
        this.Child.div.style.backgroundColor = "#ac9000"
        console.log(this)
    }

    render() {
        return <>
            <Child ref={x => this.Child = x} />
            <ChildFn ref={x => this.ChildFn = x} />
        </>
    }
}

class Child extends React.Component {
    state = {
        content: 'Child 子组件'
    }
    render() {
        return <div ref={x => this.div = x}>{this.state.content}</div>
    }
}
```

## 给函数组件设置 Ref
<font color='red'>Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?</font>
<span class='custom-box custom-box-393'>也就是说需要配合`React.forwardRef`，用于获取函数组件内的某个元素。</span>

```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    cef = React.createRef() // 创建 ref

    componentDidMount() {
        console.log(this.Child) // 子组件 <Child /> 实例
        console.log(this.ChildFn) // <div>函数组件</div>
        this.ChildFn.style.backgroundColor = "#a33"
        console.log(this)
    }

    render() {
        return <>
            <Child ref={x => this.Child = x} />
            <ChildFn ref={x => this.ChildFn = x} />
        </>
    }
}

class Child extends React.Component {
    state = {
        content: 'Child 子组件'
    }
    render() {
        return <div ref={x => this.div = x}>{this.state.content}</div>
    }
}

const ChildFn = React.forwardRef(function ChildFn(props, ref) {
    return <div ref={ref}>函数组件</div>
})
```
## 非受控组件应用场景：
*   管理焦点、文本选择、媒体播放
*   触发强制动画
*   集成第三方 DOM 库
