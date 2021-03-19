---
title: react组件
date: 2021-03-05 10:22:34
tags:
- react
---

## 1. 函数组件
```javascript
function reactComponent() {
  return <div>Hello, React Component</div>
}
```
react 组件与 jsx 实现功能基本一致，但从设计角度上为时过早还是推荐使用 React 组件方式。
> 原因是：React 组件与 Props 结合使用可以实现更灵活的功能。

## 2. 类组件
```javascript
class ReactComponent extends React.Component {
  render() {
    return <div>Hello, React Component</div>
  }
}
```

<!-- more -->

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

## 4. React Props
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
### 4-1.  React Props 只读
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

### 4-2.  React Props 默认值
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

### 4-3.  React 切分提取
切分提取出逻辑清晰、高度复用的小组件，利于代码修改并且便于维护

非受控组件应用场景：
管理焦点、文本选择、媒体播放
触发强制动画
集成第三方 DOM 库

通过与 state 无关的 refs 实现

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

## React Router 与单页应用
React 框架路由解决方案
React Router 路由用来保持页面 UI 与 URL 地址的同步
单页应用的基础
单页应用——将视图和数据设计在一个页面中显示，且不同视图的切换也在唯一一个页面完成。用户看不到页面发生跳转
React Router 是基于 React 框架开发的库，因此使用时需要通过 npm 包管理来安装开发包 react-router-dom，安装成功后就可以在应用中添加视图和数据流
