---
title: react 整理
date: 2019-03-19 18:06:55
tags:
- JavaScript
- react
---

## react Error
### react 渲染实体字体 &#247; &#215;
```JavaScript
{key: String.fromCharCode(215), id: 'times'},
```
### react Error
<font color="red">Warning: render(): Rendering components directly into document.body is discouraged, since its children are often manipulated by third-party scripts and browser extensions. This may lead to subtle reconciliation issues. Try rendering into a container element created for your app</font>
> 是在创建ReactDOM.render()时,放置的容器使用了document.body || document.getElementsByTagName('body')[0]等引起的错误，这样写会把第三方其他js给覆盖掉。
**public/index.html 里面加入一个 div, 给出唯一的 id，避免覆盖，webpack 中 htmlWebpackPlugin 中加入这个模板**



## React 列表渲染

#### keys
React 中使用 keys 标识列表中元素的删除、添加或移除。React Diff 算法中借助 key 标识同级元素是新增还是移动，避免不必要的渲染。

```JavaScript
  items.map((item, index) => {
    <li key={item.id}>{item.text}</li>
    {/* <li key={index}>{item.text}</li> 没有 id 用索引赋 key */}
  })
```
`注意：`最好不要用索引（index）作 key 值，因为一旦有新增、删除或移动等变化，会导致大量元素失效。进而造成不必要的重新渲染，损耗性能。可以使用 item.id 作为 key 值

### React state
React 中把组件看成一个状态机（state machines）。React 中，constructor 是最先执行，且执行一次。state 在 constructor 构造函数中初始化。其它地方采用 `this.setState()` 更新组件状态。
React 里，通过更新组件 state 重新渲染用户界面，不需要操作 DOM，类组件使用 props 调用基础构造函数。
```JavaScript
import React, {Component} from 'react';

export default class Test extends Component {
  constructor(super) {
    super();
    // 初始化 state
    this.state = {}
  }
}
```

```mermaid
graph LR;
    用户界面交互-->状态;
    状态-->UI渲染;
    UI渲染-->|用户界面与数据保持一致|数据;
```
this.setState 之后发生了什么？
调用 setState 函数后，React 将 setState 参数与组件当前状态合并，触发调和过程（Reconciliation）. 经过调和过程，React 会以相对高效的方式构建 React Dom 树，得到 Dom 树后，React 会将新树与老树进行对比，找出差异节点，从而根据差异最小化渲染。

重新构造 dom ，并将新老状态进行对比，最小化渲染。

### React 生命周期渲染
```mermaid
graph TM;
  componentWillMount-->render;
  render-->componentDidMount;
  componentDidMount-->render
```

1.  虚拟 DOM 机制，降低 UI 的操作
2.  JSX 语法使用

### React Hooks
不需要用到 class 中的继续，而且几乎很少在外部调用一个类的实例，组件方法在内部调用或通过生命周期调用

#### React Hooks —— 函数组件 + 状态
函数之外的空间保存状态，检测变化，触发函数组件重新渲染
把某个目标结果钩到某个可能会变化的数据源或者事件源上，那么当被钩到的数据或事件发生变化时，产生这个目标结果的代码会重新执行，产生更新后的结果

hooks 逻辑复用在 class 时期需要高阶组件处理
```javascript
const withWindowSize = Component => {
  // 产生一个高阶 WrappedComponent 包含监听窗口大小的逻辑
  class WrappedComponent extends React.PureComponent {
    constructor(props) {
      super(props)
      this.state = {
        size: this.getSize()
      }
    }
    
    getSize() {
      return window.innerWidth > 1000 ? "large" : "small"
    }
    
    handleResize = () => {
      const size = this.getSize()
      this.setState({
        size
      })
    }
    
    componentDidMount() {
      window.addEventListener('resize', this.handleResize)
    }
    
    componentWillUnmount() {
      window.removeEventListener('resize', this.handleResize)
    }
    
    render() {
      // 窗口大小传递给真正的业务逻辑组件
      return <Component size={this.state.size} />
    }
  }
  return WrappedComponent
}
```
