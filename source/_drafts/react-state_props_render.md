---
title: react 重要组成成员
date: 2021-03-09 09:21:31
tags:
- react
---

react 是单向数据流，自顶向下数据流，由父节点向子节点由上到下传递
React 组件可以看成一个状态机
React 定义框架的目的是权权通过更新 React 组件状态，实现重新渲染用户界面操作
状态机 —— 组件通过与用户交互，实现不同的状态，然后通过渲染 UI 保证用户界面和数据的一致性
React 组件是被定义为具有状态（state）的

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
<!--more-->

## React 组件中的成员 state 和 props
### 组件的 state
使用 React 内置的 setState() 修改 state，**每当使用 setState() 时，React 会将需要更新的 state 合并后放入状态队列，触发调和过程（Reconciliation）**，而不是立即更新 state，再根据新的状态结构重新渲染 UI 界面，React 会根据差异对界面进行最小化重新渲染

1.  state 设计的几点原则；
    state 代表一个组件 UI 呈现的完整的最小状态集
    - 渲染组件时所用到的数据的来源
    - 用作组件 UI 展现形式的判断依据，反应组件的 UI 变化
      <font color="red">没有任何中间状态，也不存在通过其他状态计算来的中间状态
    - 组件的整个生命周期中保持不变，若是，则不是一个状态
  ```JavaScript
  import React,{Component} from 'react';

  export default class pop extends Component {
    // construct 用于初始化组件的 state及绑定事件
    construct(props) {
      // 调用父类的 construct 方法，即调用 react.Component 的构造方法。用于完成 react 组件的初始化工作，保证 props 传入组件
      super(props);
      this.state = {
        isShow: false, // 展现形式的判断依据
        pop: '弹层', // 渲染时所用到的数据
        purchaseList: [],
        totalCost: 0 // 根据 purchaseList 中 的 price 计算得来的，属性于中间多余属性。
      }
    }

    render() {
      const {isShow, pop} = this.state;
      return (
        isShow ? <div>{pop}</div> : null
      )
    }
  }
  ```

2. 正确修改 state
state 需要通过 setState 修改，`this.state.name = 'helen'` 不会触发 render 渲染
> 不要依赖当前的 state 计算下一个 state，`state 更新是异步的`
```javascript
object.assign({
  previousState,
  {quantity: this.state.quantity + 1},
  {quantity: this.state.quantity + 1}
})
```
如上 React出于性能原因，会把多次的 state 修改合并成一次，也就是说组件的 state 并不是调用 setState 时立即修改的，而是把要修改的状态合并到一个队列中一次执行。应付造成，quantity 只加了一次，而不是设计的加 2。正确的修改方式如下：
```javascript
this.setState((preState, props) => {
  quantity: preState.quantity + 1
})
```
preState 当前更新状态的前一个状态，props 当前最新的 props

state 更新是一个合并的过程，调用 setState 修改组件时，只需要传入发生改变的 state，react 会把修改的 state 与原 state 合并，未修改的保持不变
```javascript
this.state = {...this.state, {title: 'modify title'}}
```

### 关于 state props 与使用普通属性的一些忠告
> 触发沉浸的属性是 props 和 state，props 来自父级，只读属性，不能修改。如果需要修改须在父级 setState 修改，state 维护组件内部属性。
1.  变量是否从 props 中获取，若是，则不是一个属性；
2.  变量是否在组件的整个生命周期中保持不变，若是，则不是一个状态；
3.  状态是否是从在组件 render 中使用，未使用，则不是一个状态，适合用普通属性。

```JavaScript
import React, {Component} from 'react';

export default class timer extends Component {
  construct(props) {
    super(props);
    this.state = {
      date: new Date()
    }
    this.timer = null; // 非 props 或 state，是一个普通属性
  }

  componentDitMount() {
    // timer 不用于渲染
    this.timer = setInterval(() => {
      this.setState({
        date: newDate()
      })
    }, 1000)
  }

  componentWillUnMount() {
    if (this.timer) {
      clearInterval(this.timer)
    }
  }

  render() {
    return (
      <div>{this.state.date.toString()}</div>
    )
  }
}
```

<font color="red">简而言之，就是 state 如同函数内的变量，props 如同入参，state 和 props 共同触发 ui 渲染</font>
<font color="darkgreen">this.setState({...}) 修改 state，获取 state 属性 this.state.xx</font>
setState() 两个参数， prevState 和 props 分别用于传递前一个 state 和 props 参数

### react 官方建议把 state 当作不可变对象
state 中包含的所有状态都应该是不可变对象，state 中的某个状态发生变化时，应该重新创建这个状态对象，并非是修改原来的状态。将状态类型分为三种情况创建新的状态对象：
1.  状态类型是数字、字符串、布尔值、null、undefined
直接给修改状态赋新值
```JavaScript
this.setState({
  quantity: 1,
  title: 'react',
  success: true
})
```
2.  状态类型是数组
> 状态发生变化时重新创建状态对象，数组方法中，push, pop, shift, unshift, splice 都是 `在原数组的基础上修改的`，concat、slice、filter 是`返回一个新数组`。
```javascript
/* books 是要修改的状态，类型为数组 */

// 使用 preState、concat 创建新数组
// 箭头之后的圆括号用来实现换行（MDN）
this.setState((preState) => ({
  books: preState.books.concat(['react guide'])
}))

// ES6 spread syntax
this.setState((preState) => ({
  books: [...preState.books, 'react guide']
}))
```
用 `slice` 从数组中截取一部分元素作为新状态；用`filter` 过滤部分元素作作为新状态
```javascript
this.setState((preState) => ({
  books: preState.books.slice(1,3)
}))

this.setState(preState => ({
  books: preState.books.filter(item => {
    return item !== 'react'
  })
}))
```

3.  状态类型是不包含字符串、数组的普通对象
避免修改原对象的方法，使用返回一个新对象的方法
```javascript
// 使用 ES6 Object.assgin 方法
this.setState(preState => ({
  owner: Object.assgin({}, preState.owner, {name: 'helen'})
}))

// 使用对象扩展语法 object spread properties
this.setState(preState => ({
  owner: {...preState.owner, {name: 'helen'}}
}))
```
react 推荐组件状态不可变的原因在于，返回新对象可以避免原有对象组件状态不小心情况下修改，导致错误，方便管理、调试。还有出于性能考虑，当组件对象不可变时，在组件的 shouldComponentUpdate 方法中仅需要对比前后对象的引用便可以判断是否发迹，避免不必要的 render.

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

## React render
render 方法用于渲染虚拟 DOM，返回 ReactElement 类型
元素是 React 应用的最小单位，<u>用于描述界面展示的内容</u>。元素只是组件的构成，一个元素可以构成一个组件，多个元素可以构成一个组件。
<font color="red">render() 方法是一个类组件**必须**拥有的特性，其返回一个 jsx 元素，并且**外层一定要有一个单独的元素包裹**</font>

### 1. render() 返回元素数组
react16 开始**支持返回数组组件**，不需要在列表中包含一个额外的元素，render() 方法中可以直接返回元素数组
```javascript
render() {
  return [
    <div key="id-1">demo1</div>
    <div key="id-2">demo2</div>
    <div key="id-3">demo3</div>
  ]
}

// 或者如下写法
render() {
  return (
    <>
      <div key="id-1">demo1</div>
      <div key="id-2">demo2</div>
      <div key="id-3">demo3</div>
    </>
  )
}
```
~`<></>` 是 React 16 中，React.Fragment 的简写形式但对，于部分前端支持不好，不建议使用~
```javascript
render() {
  return (
    <React.Fragment>
      <div key="id-1">demo1</div>
      <div key="id-2">demo2</div>
      <div key="id-3">demo3</div>
    </React.Fragment>
  )
}
```
**`<React.Fragment></React.Fragment>` 完整写出，可以达到标签不可见，减少 DOM 元素嵌套**

### 2. render() 返回字符串
```javascript
render() {
  return 'return string'
}
```
### 3. render() 方法中的变量与运算符 &&
```javascript
render() {
  return (
    true && <div>ifShow</div>
  )
}
```
### 4. 三目运算符
```javascript
render() {
  return (
    true ? 'signed in' : 'not login'
  )
}
```

## React 条件渲染
设计人员创建不同的组件来封装各种业务需求，然后依据需求的不同状态，渲染对应状态下的局部内容
1. 使用 if 条件判断，分别渲染两种状态下的组件
2. 逻辑与运算符条件渲染

react 创建、转换、使用列表
React 列表中使用 key 帮助 react 框架识别元素的改变，识别一个元素 key 最好是在其所在列表中独一无二
React 特取有效性，key 值权权会传递信息给 React 框架，用户的自定义组件无法读取
React 表单与HTML表单默认行为相同，在 React 环境中执行 HTML 代码仍然有效

HTML 表单是自己管理自己的状态，根据用户输入修改状态。而 React 组件属于受控组件，它的状态由 state 管理，需要改变时，通过 setState 修改其状态
