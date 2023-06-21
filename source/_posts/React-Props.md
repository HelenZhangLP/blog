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

### React Props 只读
> props 只读属性，需要添加 readOnly，或通过 onChange 属性修改 state，或将从 props 中取出的属性设置为 defaultValue

将上面示例代码中的 readOnly 属性去掉，代码如下：
```javascript
...
function Test(props) {
  return <input type="text" value={props.test} />
}
...
function FormLogin() {
  return (
    <div>
      <FormTitle />
      <UserID userId="001" />
      <UserName userName="Audery" />
      <Submit />
      <Test test="1" />
    </div>
  )
}
```
`Warning: Failed prop type: You provided a 'value' prop to a form field without an 'onChange' handler. This will render a read-only field. If the field should be mutable use 'defaultValue'. Otherwise, set either 'onChange' or 'readOnly'.
    in input (created by Test)
    in Test (created by FormLogin)
    in div (created by FormLogin)
    in FormLogin`
属性类型错误，给没有 onchange 事件的表单提供了 value 属性，如果字段可变的使用 defaultValue，否则，设置 onChange 或 readOnly

### React Props 默认值
> 在 react 类组件中定义一个默认 props——defaultProps（函数组件可以通过 参数传递 props），使用 defaultProps 默认值来实现 React Props 应用

```javascript
class ImgTest extends React.Component {
  render() {
    return <img src={this.props.src} alt="" style={this.props.style}/>
  }
}

ImgTest.defaultProps = {
  src: "https://www.baidu.com/img/PCpad_012830ebaa7e4379ce9a9ed1b71f7507.png",
  style: {
    margin: "0 auto",
    width: "270px",
    height: "129px"
  }
}
```

## React Props
是组件之间的桥梁，负责组件之间的通信
> props 不可改变，props 的值只能从默认属性和父组件中传递过来

```html
<script type="text/babel">
  let root = document.querySelector('#root');
  const {Input, Button} = antd

  function FormTitle() {
    return <h1>UserLogin</h1>
  }

  function Test(props) {
    return <input type="text" value={props.test} readOnly />
  }

  function UserID(props) {
    return <Input placeholder="UserID" value={props.userId} />
  }

  function UserName(props) {
    return <Input placeholder="UserName" value={props.userName} />
  }

  function Submit() {
    return <Button type="primary" block>Submit</Button>
  }

  function FormLogin() {
    return (
      <div>
        <FormTitle />
        <UserID userId="001" />
        <UserName userName="Audery" />
        <Submit />
        <Test test="1" />
      </div>
    )
  }

  ReactDOM.render(<FormLogin />, root)
</script>
```