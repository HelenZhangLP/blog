---
title: react 组件
date: 2020-11-29 19:43:42
tags:
- react
---
## 组件的 state
1.  state 设计的几点原则；
2.  正确修改 state;
## 组件通信
1.  组件在 componentDidMount 中与服务器通信
2.  父子组件与兄弟组件通信

<!--more-->
## 组件的 state
### 适合的 state
* state 代表一个组件 UI 呈现的完整状态集
  - 渲染组件时所用到的数据的来源
  - 用作组件 UI 展现形式的判断依据
```JavaScript
import React,{Component} from 'react';

export default class pop extends Component {
  // construct 用于初始化组件的 state及绑定事件
  construct(props) {
    // 调用父类的 construct 方法，即调用 react.Component 的构造方法。用于完成 react 组件的初始化工作，保证 props 传入组件
    super(props);
    this.state = {
      isShow: false, // 展现形式的判断依据
      pop: '弹层' // 渲染时所用到的数据
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
* state 代表一个组件 UI 呈现的最小状态集，state 中所有的状态都用于反映组件 ui 变化，没有任何多余的状态，也不应该存在通过其他状态计算而来的中间状态。
```JavaScript
import React, {Component} from 'react';

export default class purchaseCart extends Component {
  construct(props) {
    super(props);
    // 错误的 state 案例
    this.state = {
      purchaseList: [],
      totalCost: 0 // 根据 purchaseList 中 的 price 计算得来的，属性于中间多余属性。
    }
  }
}
```
### `demo 显示当前组件并每秒钟更新`

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
> 触发沉浸的属性是 props 和 state，props 来自父级，只读属性，不能修改。如果需要修改须在父级 setState 修改，state 维护组件内部属性。
1.  变量是否从 props 中获取，若是，则不是一个状态；
2.  变量是否在组件的整个生命周期中保持不变，若是，则不是一个状态；
3.  状态是否是从在组件 render 中使用，未使用，则不是一个状态，适合用普通属性。

### 正确修改 state
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

## 组件与服务器通信
1.  组件挂载阶段通信
  componentDidMount、componentWillUnMount、constructor 应该在这三个生命周期的哪个中与服务器通信呢？
  * constructor 用于初始化组件的 state 以及绑定事件处理等工作，在这里与服务器通信初始化与获取数据后赋值相互污染
  * componentWillMount 这个方法是组件挂载到 DOM 前调用，且只会调用一次，这个方法中调用 this.setState 不会引起组件重新渲染，虽然 componentWillMount 在 componentDidMount 前执行，但两个重合周期的执行时间差微乎其微。另一种说法，当组件在服务器端渲染时，componentWillMount 会被调用两次，一次服务端，一次浏览器。在 componentWillMount 里面通信，会发送多余的数据请求
  * componentDidMount 组件挂载到了 Dom，Dom 也已经渲染完成，调用 api 最安全，也是官方推荐与服务器通信的生命周期。可以保证获取到数据时，组件已经挂载到了Dom, 这时直接操作 Dom 也是安全的
2.  组件更新阶段通信
  props 中的某一依据发生变化，这里组件需要重新获取数据更新。componentWillReceiveProps 在 props 引起的组件更新过程中调用。方法参数 nextProps 是父组件传递给当前组件的新的 props. setState 并不会触发 componentWillReceiveProps。
## 组件通信
1.  父子组件通信
  父子组件间的通信是由 props 完成的
  ```javascript
   // 父组件
   import React from 'react';
   import UserList from './component/UserList';

   export default class User extends React.Component {
     //... 省略
     // props 将 users 传递给子组件 UserList
     handleName(name) {
      // 调用修改用户信息接口
      // console.log(name)
     }
     render() {
       return (
         <UserList users = {this.state.users}>
       )
     }
   }

   // 子组件
   import React from 'react';
   export default class UserList extends React.Component {
     modifyName(event) {
        const { value } = event.target
        // 子组件通过 props 调用父组件方法
         this.props.handleName(value)
       }
      // 返回代表该组件的 ui react 元素
      render() {
        const { users = [] } = this.props;
        return [
            users.map(item => (
              <div key={item.key}>
                <input value={item.name} onChange={this.modifyName.bind(this)} />
              </div>
            ))
        ]
      }
   }
  ```
2.  兄弟组件通信
3.  context
4.  延伸
## 组件的 ref 属性
