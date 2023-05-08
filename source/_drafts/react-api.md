---
title: React API
date: 2020-11-25 15:56:00
tags:
- React
---

## React.createClass 用于注册一个组件
The deprecations introduced in 15.x have been removed from the core package. 

## React.createElement
> 创建并返回指定类型的新 React 元素。
> 其中的类型参数既可以是标签名字符串（如 'div' 或 'span'），也可以是 React 组件 类型 （class 组件或函数组件），或是 React fragment 类型。
> 使用 JSX 编写的代码将会被转换成使用 React.createElement() 的形式。
```
React.createElement(
  type,
  [props],
  [...children]
)
```
### eg.
```
React.createElement(
	'<>', // fragment 类型
	{className: 'antd-form'},
	react.createElemtn(
		FormComponent,
		{
			data: ['name': 'hel']
		}
	),
	React.createElement(
		'li', // 标签字符串
		null,
		'hello react'
	)
)
```

## ReactDOM.render()
### React16 render vs React18 render
```javaScript
// react16
import react from 'React'
import ReactDom from 'react-dom'

ReactDOM.render(<>...</>, document.getElementById('root'))
```
---
```javaScript
// React18
import react from 'React'
import ReactDOM from 'react-dom/client'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(<>...</>)
```
[createElement and render 手动实现](https://github.com/HelenZhangLP/react-18/blob/master/src/JSX/handle.js)
> 在 React 18 中，render 函数已被 createRoot 函数所取代。

## ReactDOM.createRoot()
```javaScript
createRoot(container[, options]);
```
Create a React root for the supplied container and return the root. The root can be used to render a React element into the DOM with render:
为提供的容器创建React根并返回根。根可以用于通过render将React元素渲染到DOM中：
```
const domContainer = document.querySelector('#like_button_container');
const root = ReactDOM.createRoot(domContainer);
root.render(e(LikeButton));
```
The root can also be unmounted with unmount:
```
root.unmount();
```

<!-- React 核心 API
*   CreateElement
*   render
*   Component

引入虚拟 DOM、状态、单向数据流
以组件为核心，用组件搭建 UI

数据
组件状态
UI

开发环境
node npm
webpack
bable
eslint

JSX 一种描述 UI 的 JavaScript 扩展语法，React 采用这种语法描述组件的 UI
jsx 中表达式的两种应用场景：
1.  通过表达式给标签属性赋值
2.  通过表达式定义子组件

基本语法
标签类型
JavaScript 表达式
标签属性 -->
