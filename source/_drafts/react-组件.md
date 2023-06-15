---
title: React Component
date: 2021-03-05 10:22:34
tags:
- React
---

组件命名采用 PascalCase 命名规则。
类组件是动态组件，除父组件更新，触发 props 更新外，还可以通过 this.setState 或 this.forceUpdate 修改组件状态

```mermaid
mindmap
      组件
        A(静态组件)
        B[动态组件]
          c))hooks 函数组件((
          d)类组件(  

```
## 函数组件
* 函数组件是静态组件，父组件更新，子组件才会更新，无法基于组件内部的操作控制更新。<i>不具备”[状态](/2021/03/09/React-State/)、[ref](/2021/05/11/React-Refs/)、[周期函数]()等“</i>
* 函数组件有属性、插槽、父组件可以控制重新渲染；
* 渲染流程简单，渲染速度快；
* 基于 FP（函数式编程）思想设计，提供更细粒度的逻辑组织和复用。

## 类组件
* 具备“状态，[ref](2021/05/11/React-Refs/)，属性、插槽“ 等，可灵活控制组件更新，基于钩子函数也可灵活掌控不同阶段处理不同事项；
* 渲染流程繁琐，渲染速度慢；
* 基于 OOP（面向对象）思想设计，更方便实现继承

## Hooks 组件
* 让函数组件动态化，React 16.8 新增特性，只能运用到函数组件中。

## 组件渲染机制
```mermaid
 flowchart LR
 jsx[jsx 视图] -->|babel-preset-react-app 转义| react[react 原生]
 react -->|"运行 React.createElement..."| virtualDOM[virtualDOM]
 virtualDOM -->|root.render| realDOM[真实DOM]
```

### 经过 babel-preset-react-app 转义为 react 原生为：
```javascript
React.createElement(ComponentDEMO, {
  className: "App-header",
  style: {
    color: '#a33'
  },
  title: "function component demo",
  data: [1, 2, 3],
  times: 3
});
```

### 执行 React.createElement 生成 virtualDOM
```javascript
{   
  "$$typeof": Symbol(react.element)
  "key": null,
  "ref": null,
  "type": demo1(),
  "props": {
      "className": "App-header",
      "style": {
          "color": "#a33"
      },
      "title": "function component demo",
      "data": [
          1,
          2,
          3
      ],
      "times": 3
  },
  "_owner": null,
  "_store": {}
}
```
### root.render 方法将虚拟 DOM 转换为真实 DOM，渲染到页面。
  [具体实现原理见：github/handle.js](https://github.com/HelenZhangLP/react-18/blob/master/src/JSX/handle.js)

##  react 中创建组件的三种方式
### 1.  ES5 写法：React.createClass()
> React.createClass() 创建一个组件类，该类**接收一个对象参数**
  <u>对象参数中声明 render() 方法</u>
  render() 方法返回一个组件实例

```javascript
  // React.createClass 是一个工厂函数
  var Component = React.createClass({
    propTypes: {
      initialValue: React.PropTypes.string
    },
    defaultProps: {
      initialValue: ''
    },
    getInitialState: function() {
      return {
        text: 'ES5 创建 react 组件'
      }
    },
    onModify: function() {
      thi.setState({
        text: '正确绑定 this 到 React 实例，会导致一定的性能开销'
      })
    },
    render: function() {
      return <div onClick={this.onModify}>this.state.text</div>
    }
  })
```
<font color="red">**createClass 内的方法会正确绑定 this 到 React 类的实例上，会导致一定的性能开销**</font>

### 2.  ES6 写法：React.Component
创建有状态组件，能更好的实现代码利用
<span class='custom-box custom-box-933'>ES6之前：</span>**需要手动绑定 this**

```javascript
import React from 'react';
class Test extends React.component {
  constructor(props) {
    super(props)
    this.initialState = {
      text: 'initial state'
    }
    // 手动绑定 this 方法 一
    this.onModify = this.onModify.bind(this)
  }

  onModify() {
    this.setState({
      text: 'ES6 之前需要手动绑定 this'
    })
  }

  render() {
    return (
      {/*
        手动绑定 this 之方法二
        <div onClick={this.onModify.bind(this)}>this.state.text</div>
        手动绑定 this 之方法三
        <div onClick={() => {
          this.onModify()
        }}>this.state.text</div>
        */}
      <div onClick={this.onModify}>this.state.text</div>
    )
  }
}
```
<span class='custom-box custom-box-933'>ES6：</span>**使用箭头函数，继承当前实例上下文**

```JavaScript
import React from "react";
import {Button} from 'antd'

export default class Demo extends React.Component {
    state = {
        num: 0
    }

    componentDidMount() {
        console.log(this, 'componentDidMount')
    }

    /**
     * handleBtn1() {...} 相当于给 Demo.prototype.handleBtn，即给原型上公共方法
     * this = undefined
     * this.handleBtn1.bind(this) 指定 this 实例
     */
    handleBtn1() {}
    
    /**
     * 相当于添加【实例属性】Demo.hadnleBtn = ()=>{}
     * 箭头函数的this继承实例自上下文
     */
    handleBtn = () => {
        console.log(this)
        let {num} = this.state
        this.setState({
            num: ++num
        })
    }

    render() {
        return <div>
            <Button type="primary" onClick={this.handleBtn}>按钮{this.state.num}</Button>
        </div>
    }
}
```

### 3.  无状态函数写法，又称线组件 SFC
> 可读性好，大大减少代码代码量
  无状态函数式组件搭配箭头函数，更简洁，它没有 state 和生命周期
  无状态函数组件需要生命周期时，**搭配高阶组件（HOC）**实现
  无状态组件作为高阶组件的参数，高阶组件内放需要的生命周期和状态
  无状态函数式组件负责展示

```javascript
// 无状态函数组件
const Test = (props) => (<div>{props.name}</div>)

import React from 'react'
export const Test = (ComponentTest) => {
  return class extends React.Component {
    constructor(props) {
      super(props)
    }
    componentDidMount() {}
    render() {
      return (<ComponentTest {...this.props}) />
    }
  }
}
```

## ~函数组件(摘自 reactjs 16，该观点个人不赞成)~
```javascript
function reactComponent() {
  return <div>Hello, React Component</div>
}
```
~react 组件与 jsx 实现功能基本一致，但从设计角度上为时过早，还是推荐使用 React 组件方式。~
> ~原因是：React 组件与 Props 结合使用可以实现更灵活的功能。~

## 3. 组合组件
React 可以在自身定义中引用其它组件，构成组合组件。好处是使用同一组件来抽象出任意层次的细节。
```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no,shrink-to-fix=no" />
    <title>组合组件</title>
    <script src="https://unpkg.com/react@16/umd/react.development.js" charset="utf-8"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js" charset="utf-8"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/antd/4.13.0/antd.js" charset="utf-8"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/antd/4.13.0/antd.css">
  </head>
  <body>
    <div id="root"></div>
    <script type="text/babel">
      let root = document.querySelector('#root');
      const {Input, Button} = antd

      function FormTitle() {
        return <h1>UserLogin</h1>
      }

      function UserID() {
        return <Input placeholder="UserID" />
      }

      function UserName() {
        return <Input placeholder="UserName" />
      }

      function Submit() {
        return <Button type="primary" block> Submit </Button>
      }

      function FormLogin() {
        return (
          <div>
            <FormTitle />
            <UserID />
            <UserName />
            <Submit />
          </div>
        )
      }

      ReactDOM.render(<FormLogin />, root)
    </script>
  </body>
</html>
```

### 4-3.  React 切分提取
切分提取出逻辑清晰、高度复用的小组件，利于代码修改并且便于维护


React 框架在组件代码重用方面，具有组合和继承模式。React 官方推荐设计人员在实际项目中尽量使用组合模式。
React 基于 ES6 规范标准设计，也支持继承关系 extends 关键字可以实现继承

当多个组件需要反映相同的变化数据，建议将共享状态提升到最近的共同父组件中 **状态提升**

babel 是 javascript 的编译器，主要用来将 ES6+ 代码转换为 ES5 版本的 javascript 语法，保证最新 ES6+ 语法能在旧版本浏览器或其它环境中运行。
webpack 是 javascript 应用程序静态模块打包器
webpack 将 javascript 应用程序视为模块
webpack 通过 loader 转换文件，plugin 注入勾子，最终输出由多个文件合并而成的模块化项目

webpack 对于 web 服务支持
> npm i webpack-dev-server --save
package.json add "start: webapck-dev-server --open --mode development"
