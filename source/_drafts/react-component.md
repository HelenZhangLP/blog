---
title: react 组件通信
date: 2020-11-29 19:43:42
tags:
- react
---

## 组件通信
1.  组件在 componentDidMount 中与服务器通信
2.  父子组件与兄弟组件通信

## 组件与服务器通信
1.  组件挂载阶段通信
  componentDidMount、componentWillUnMount、constructor 应该在这三个生命周期的哪个中与服务器通信呢？
  * constructor 用于初始化组件的 state 以及绑定事件处理等工作，在这里与服务器通信初始化与获取数据后赋值相互污染
  * componentWillMount 这个方法是组件挂载到 DOM 前调用，且只会调用一次，这个方法中调用 this.setState 不会引起组件重新渲染，虽然 componentWillMount 在 componentDidMount 前执行，但两个重合周期的执行时间差微乎其微。另一种说法，当组件在服务器端渲染时，componentWillMount 会被调用两次，一次服务端，一次浏览器。在 componentWillMount 里面通信，会发送多余的数据请求
  * componentDidMount 组件挂载到了 Dom，Dom 也已经渲染完成，调用 api 最安全，也是官方推荐与服务器通信的生命周期。可以保证获取到数据时，组件已经挂载到了Dom, 这时直接操作 Dom 也是安全的
2.  组件更新阶段通信
  props 中的某一依据发生变化，这里组件需要重新获取数据更新。componentWillReceiveProps 在 props 引起的组件更新过程中调用。方法参数 nextProps 是父组件传递给当前组件的新的 props. setState 并不会触发 componentWillReceiveProps。

## 组件通信
### 1.  父子组件通信
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
### 2.  子组件向父组件通信
通过父组件向子组件传递函数的特性，利用回调函数来实现子组件向父组件通信。
> 一般情况下回调函数与 setState() 成对出现

```javascript
function Child(props) {
  return <input type="text" onChange={props.handlerChange} />
}

class App extends React.component {
  constructor() {
    this.setState = {
      text: ''
    }
    this.handlerChange = this.handlerChange.bind(this)
  }
  handlerChange(e) {
    this.setState({
      text: e.target.value
    })
  }
  render() {
    return (
      <React.Fragment>
        <Child handlerChange = {this.handlerChange} />
      </React.Fragment>
    )
  }
}
```
### 3.  跨级组件通信
多层组件嵌套时，用 props 层层传递可以实现通信，但不优雅。<u>可以使用 context 来实现跨级父子组件通信</u>
context 为了共享对于一个组件树而言是“全局性”的数据，可以尽量减少逐层传递，<u>但并不建议使用 context</u>
<font color="red">结构复杂时，全局变量不易追踪源头，会导致混乱，不易维护</font>

4.  非嵌套组件通信

4.  延伸
## 组件的 ref 属性
