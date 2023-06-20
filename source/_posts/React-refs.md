---
title: React Refs
tags:
  - React
  - Component
date: 2021-05-11 20:46:13
---


## 受控组件
基于修改数据/状态，让视图更新，达到需要的效果

## 非受控组件
基于 `ref` 获取 DOM 元素，操作 DOM 实现需求和效果

### 非受控组件应用场景：
*   管理焦点、文本选择、媒体播放
*   触发强制动画
*   集成第三方 DOM 库

### 基于 ref 可以实现的功能：
*   赋值给标签，获取 DOM 元素；
*   赋值给类子组件，获取子组件的属性、方法
*   赋值给函数子组件，获取子组件的 DOM 元素【配合 React.forwardRef 进行转发】

## 类组件 ref 相关
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
<span class='custom-box custom-box-339'>原理：</span>创建一个实例属性 refFn，DOM 元素直接挂载实例属性 refFn 上 。

```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    componentDidMount() {
        console.log(this.refFn)
        this.refFn.style.backgroundColor = '#668999'
    }

    render() {
        return <>
            <h2 ref={x => this.refFn = x}>ref 设置为一个函数</h2>
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
        console.log(this.cef) // cef : {current: h2}
        this.cef.current.style.backgroundColor = '#ff9923'
    }

    render() {
        return <>
            <h2 ref={this.cef}>React.createRef 创建非受控组件</h2>
        </>
    }
}
```

## 类子组件中使用 Ref 获取 DOM 并进行业务处理
获取当前调用组件创建的实例，后面可以根据实例获取子组件中的相关信息
```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    componentDidMount() {
        console.log(this.Child) // 获取到的是子组件 <Child /> 实例
        this.Child.div.style.backgroundColor = "#ac9000"
    }

    render() {
        return <>
            <Child ref={x => this.Child = x} />
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

## 获取函数子组件的 DOM 并进行业务处理
<font color='red'>Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?</font>
<span class='custom-box custom-box-393'>解决：</span>需要配合`React.forwardRef`，用于获取函数组件内的某个元素。

```JavaScript
import React from "react"
export default class RefsDemo extends React.Component {
    componentDidMount() {
        this.ChildFn.style.backgroundColor = "#a33"
    }

    render() {
        return <>
            <h2>函数组件</h2>
            <ChildFn ref={x => this.ChildFn = x} />
        </>
    }
}

const ChildFn = React.forwardRef(function ChildFn(props, ref) {
    return <div ref={ref}>函数组件</div>
})
```

## 函数组件中使用 ref
* 1 `ref={x => dom = x}` 方式，将 DOM 元素赋值给 dom 变量
* 2 通过 【dom = React.createRef() 创建 ref 对象 && dom.current 获取 dom && ref = {dom} 绑定jsx dom 元素】 创建 ref 对象获取 DOM
* 3 useRef hooks 创建 ref 对象 && dom.current 获取 dom && ref = {dom} 绑定jsx dom 元素
[具体代码实现见]()

<span class='custom-box custom-box-933'>对比 React.createRef 与 useRef 在函数组件中的使用，React.createRef 更新时创建新的 ref 对象，浪费性能。useRef 则不会。[具体代码实现见]()</span>

## 父子组件通信
### 子组件为类组件
```JavaScript
class Child extends React.Component {
    render() {
        return <div>It's Child Component</div>
    }
}

function Demo() {
    let component = useRef(null)
    useEffect(() => {
        console.log(component.current) // 打印子组件实例
    })
    return <div>
        <Child ref={component}/>
    </div>
}
```
### 子组件为函数组件
<span class='custom-box custom-box-933'>子组件为函数组件，函数组件用 React.forwardRef 进行转发</span>

```JavaScript
const ChildFn =  React.forwardRef(function ChildFn (props, ref) {
    return <div ref={ref}>It's function component</div>
})

function DemoFn() {
    let component = useRef(null)
    useEffect(() => {
        console.log(component.current)
    })
    return <div>
        <ChildFn ref={component} />
    </div>
}
```

### useImperativeHandle 钩子，实现父组件调用子组件属性和方法
>
```JavaScript
/**
 * refparams 父组件传过来的 ref
 * createHandle 要传给父组件的属性和方法对象
 * [deps] 依赖项，依赖项的值发生变化，返回最新的属性和方法传给父组件；如果没有依赖项，只要子组件 render，都会将属性和方法返回给父组件。
 */ 
useImperativeHandle(refparams, createHandle, [deps])
```

具体实现
```JavaScript
const ChildFn =  React.forwardRef(function ChildFn (props, ref) {
    let [state, setState] = useState('函数组件中使用状态')

    // 返回子组件方法给父组件
    useImperativeHandle(ref, ()=>{
        return {
            state,
            setState
        }
    })
    return <div ref={ref}>It's function component</div>
})

function DemoFn() {
    let component = useRef(null)
    useEffect(() => {
        console.log(component.current)
    })
    return <div>
        <ChildFn ref={component} />
    </div>
}
```