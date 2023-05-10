---
title: React APIs
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

## Children
Children lets you manipulate and transform the JSX you received as the children prop.
Children 可以操作和转换 props.children 中接收到的 JSX

### 卡槽了解一下
就是我们通常需要传递结构给子组件，将组件的子元素传递给子组件，如下：
* 先从子组件调用的方法说起吧
```javaScript
// index.js
<Com /> // 第一种
<Com><div>结构</div></Com> // 第二种
<Com><div>结构一</div><div>结构二</div></Com> // 第三种
```
---
```javaScript
// com.js
function Com(props){
	// children 可能存在 3 种情况：children = undefined; children = Object;  children => Array
	let {children} = props
	
	// 需求： children.length > 1 时，children[0] 和 children[1] 分别渲染在页面顶部和页面底部

	return <>
		{children[0]}
		<div>页面内容</div>
		{children[1]}
	</>
}
```
> children.length > 1 时，children[0] 和 children[1] 分别渲染在页面顶部和页面底部
<font color="red">props.children 可能存在 3 类值：children = undefined; children = Object;  children => Array，所以现在需要处理的问题兼容处理 children 取值问题</font>

#### 方法1：

```javaScript
// com.js
function Com(props){
	// children 可能存在 3 种情况：children = undefined; children = Object;  children => Array
	let {children} = props
	
	// 需求： children.length > 1 时，children[0] 和 children[1] 分别渲染在页面顶部和页面底部
	if (!children) {children = []}
	if (children.length === 1) {children = [children, null]}

	return <>
		{children[0]}
		<div>页面内容</div>
		{children[1]}
	</>
}
```

#### 方法2：children = Children.toArray(children)
```javaScript
// com.js
import {Children} from 'react'
function Com(props){
	// children 可能存在 3 种情况：children = undefined; children = Object;  children => Array
	let {children} = props
	
	// 需求： children.length > 1 时，children[0] 和 children[1] 分别渲染在页面顶部和页面底部
	children = Children.toArray(children)

	return <>
		{children[0]}
		<div>页面内容</div>
		{children[1]}
	</>
}
```
#### children.forEach 的使用案例
使用具名 slot 对父组件传递的插槽进行分类
```javaScript
// com.js
import {Children} from 'react'
function Com(props){
	// children 可能存在 3 种情况：children = undefined; children = Object;  children => Array
	let {children} = props, headerSlot=[], footSlot=[], otherSlot=[]
	
	// 需求： children.length > 1 时，children[0] 和 children[1] 分别渲染在页面顶部和页面底部
	Children.forEach(children, element => {
		console.log(element)
		const {slot} = element.props
	})

	return <>
		{children[0]}
		<div>页面内容</div>
		{children[1]}
	</>
}
```


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
