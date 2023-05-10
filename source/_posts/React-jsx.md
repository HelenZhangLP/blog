---
title: react-jsx
date: 2021-03-03 13:06:14
tags:
- React
---

## JSX 介绍
JavaScript XML 缩写为 JSX
JSX 作为 JavaScript 语法扩展，支持自定义属性，并具有很强的扩展性
<span class='custom-box custom-box-933'>React 中使用 JSX 语法，必须引用 ‘babel.js’ 解析 JSX</span>
<span class='custom-box custom-box-393'>`<script type="text/babel"></script>` 浏览器内置的 JavaScript 解释器不解析标签里的脚本代码，由 `babel` 解析，避免 react 与原生 javascript 代码混淆</span>

## JSX 作为单独文件引入
```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <!-- react core code，when deploying, replace 'react.development.js' with 'react.production.min.js' -->
    <script src="https://unpkg.com/react@16/umd/react.development.js" charset="utf-8"></script>
    <!-- react dom when deploying replace 'react-dom.development.js' with 'react-dom.production.min.js' -->
    <script src="https://unpkg.com/react-dom@16/umd/react-dom-development.js" charset="utf-8"></script>
    <!-- Don't use it in production -->
    <script src="https://unpkg.com/babel-standalone@5.16.0/babel.min.js" charset="utf-8"></script>
    <title></title>
  </head>
  <body>
    <div id="root"></div>
    <!-- import.js 是 babel 编写的 js 代码 -->
    <script src="./import.js" type="text/babel"></script>
  </body>
</html>
```

### 变量命名
* 驼峰命名（camelCase/PascalCase）
* 烤肉串命名（kebab-case）
* 下划线命名

### JS 表达式
```javaScript
const name = 'arithmetic'
const span = (
  <span>{name}</span>
)
```
|数据类型|渲染结果|案例|
|---|---|---|
|Number|渲染输入内容|{1} --> 1|
|String|渲染输入内容|{"hello"} --> hello|
|boolean|渲染内容为空|{true} --> |
|null|渲染内容为空|{null} --> |
|undefined|渲染内容为空|{undefined} --> |
|Symbol|渲染内容为空|{Symbol} --> |
|BigInt|渲染内容为空|{10n} --> |
|对象|error|<font color="red">Objects are not valid as a React child (found: object with keys {}). If you meant to render a collection of children, use an array instead.|
|数组|数组元素分别渲染为字符串|{[1,2]}-->12|
|函数|error|<font color="red">Warning: Functions are not valid as a React child. This may happen if you return a Component instead of <Component /> from render. Or maybe you meant to call this function rather than return it</font>|

`除行内样式和JSX 虚拟 DOM 对象外，{} 语法中均不支持使用对象类型`


### JS 循环表达式
```javaScript
  [].map(item => <div>{item}</div>)
```

### JS 条件表达式 —— 三元运算符
```javascript
const span = (
  <span>{true ? 1 : 2}</span>
)
```
### JSX 对象表达式
```javascript
const userInfo = {
  name: 'arithmetic',
  age: 1
}
const span = (
  <span>{userInfo.name}</span>
)
```
### JSX 函数表达式
```javaScript
const userInfo = {
  name: 'arithmetic',
  age: 1
}
function getUserName() {
  return userInfo.name
}

const span = (
  <span>{getUserName()}</span>
)
```
### JSX 增强函数表达式
```javascript
const userInfo = {
  name: 'arithmetic',
  age: 1
}

function getAge() {
  if (userInfo.age > 1) {
    return Number(userInfo.age) + 1
  } else {
    return Number(userInfo.age) + 2
  }
}

const span = (
  <span>{getAge()}</span>
)
```
### JSX 数组表达式
```javascript
const userInfo = {
  name: 'arithmetic',
  age: 1
}
function getAge() {
  if (userInfo.age > 1) {
    return Number(userInfo.age) + 1
  } else {
    return Number(userInfo.age) + 2
  }
}
const lis = [
  <li>name: {userInfo.name}</li>
  <li>{getAge()}</li>
]
const ol = (
  <ul>{lis}</ul>
)
```
### JSX 表达式样式
```javascript
let rootDiv = document.querySelector('#root');
const nameStyle = {
  fontFamily: 'Arial',
  color: '#f90'
}
const ageStyle = {
  fontFamily: 'sens-serif',
  color: '#f99'
}
const userInfo = {
  name: 'arithmetic',
  age: 1
}
function getAge() {
  if (userInfo.age > 1) {
    return Number(userInfo.age) + 1
  } else {
    return Number(userInfo.age) + 2
  }
}
var lis = [
  <li style={nameStyle}>name: {userInfo.name}</li>,
  <li style={ageStyle}>age: {getAge()}</li>
]
const ol = (
  <ul>{lis}</ul>
)

ReactDOM.render(ol, rootDiv)
```

### JSX 引入多个样式类表达式
```javaScript
const Demo = (props) => (
  <div className={`btn` `btn-${props.theme}`}>className</div>
)
```
