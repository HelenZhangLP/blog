---
title: 虚拟DOM和性能优化
date: 2020-12-02 17:32:56
tags:
- react
---

React 执行效率高的原因在于它的`虚拟DOM机制`

## 虚拟 DOM
前端性能优化的重要一条：**尽量减少对DOM操作**
DOM 是对结构化广西的抽象表达，web 环境中，DOM 是对 html 文本的抽象描述，每个 html 元素对应一个 DOM 节点，html 元素的层级关系也体现在了 DOM 树上。在 DOM 进行增删改操作，都会引起浏览器对网页的重新布局和重新渲染，这个过程很耗时。
**软件开发中遇到的所有问题都可以通过增加一层抽象解决** 虚拟 DOM 是 DOM 效率低下的抽象。虚拟 DOM 是用 javascript 描述的DOM 元素。
```html
<div className='foo'>hello</div>
```
```JavaScript
{
  type: 'div',
  props: {
    className: 'foo',
    children: 'hello'
  }
}
```
<!--More-->
## diff 算法
react 的调合过程（Reconciliation）react 采用声明式的 API 描述 UI 结构，组件状态或属性更新，组件的 react 方法会返回一个新的虚拟 DOM 对象表述新的 ui 结构。 Diff 算法中，比较新的虚拟 DOM 和 旧的虚拟 DOM，再将 diff 的结果更新到真实 DOM 上。
React 基于两个假设，降低算法的时间复杂度，进行比较：
1.  如果两个元素的类型不同，那么它们将生成两个不同的树
2.  为列表中元素设置 key 属性，key 标识对应元素在多次 render 过程中是否发生变化
react 从根结点比较两棵树的差异，根结点不同，两棵树不同，具体如下：
### 当根结点是不同类型时
根结点变化，react 会认为新的树和旧的树完全不同，不会继续比较其它属性和子节点，直接拆掉整个树重建。重建后的 DOM 树会整体更新到真实的 DOM 树中。需要操作大量 DOM 更新效率低。
虚拟 DOM 结点类型分为两类：
1.  像 `div/p` 等 DOM 元素类型；
2.  react 组件类型，如 react 自定义组件。
    旧的 react 组件实例在 componentWillUnMount 中调用，新的组件实例在 componentWillMount 和 componentDidMount 中调用。
包裹组件的 dom 元素变量，a 组件改变 b 组件，或组件中的元素变化，都是结点类型发生变化。
### 当根结点相同的 DOM 类型时
根节点相同，属性不同，react 只更新需拟 DOM 树和真实 DOM 树对应的节点。
### 当根结点是相同组件类型时
如果两个根节点是相同类型的组件，对应的组件实例不会被销毁，只会执行更新操作。并且在 componentWillReceiveProps 和 componentWillUpdate 中调用组件，更新变化的属性。
比较完根节点，react 会以同样的方式递归比较子节点，当然子节点又是它子节点的根节点。如此递归，走到比较完两棵树上的所有节点，计算出差异更新到 DOM 树。
对于列表，react 提供了 key 的属性，key 是为了帮助 react 提高 diff 算法效率的。每次渲染后，只要 key 不变，react 认为是只一节点。如下 code，`<li key='first'>first</li>` 与 `<li key='second'>second</li>` 两个元素 key 值没有变，只是位置变化，react 判断出为新增节点。从而避免了大量渲染。**不要用 index 索引做 key 值，一旦数组开头新增，大量 key 失败，从而引用大量重新渲染**
```html
<li key='third'>third</li>
<li key='first'>first</li>
<li key='second'>second</li>
```
## 性能优化
1.  使用生产环境版本库
`npm run start` 使用的是开发环境版本的 react 库
`npm run build` 构建生产环境 react 库，其它第三方也执行生产环境版本构建(生产环境 NODE_ENV = 'production')
一般第三方库都会根据 process.env.NODE_ENV 这个变量决定开发环境和生产环境执行
如果是自己编写 Webpack 的构建配置，在生产环境构建时，需要在 Webpack 的配置项中包含以下插件配置：
```JavaScript
plugins: [
  new webpack.DefinePlugin({
    'process.env': {
      NODE_ENV: JSON.stringify('production')
    }
  }),
  new UglifyJSPlugin()
]
```
2.  避免不必要的组件渲染
重写 shouldComponentUpdate 方法避免不必要的组件渲染
{%plantuml%}
"state or props change"  --> "render"
"render" --> "render": 子组件 props 未变化
"render" --> "子组件 componentWillReceiveProps": 子组件的 props 变化
"子组件 componentWillReceiveProps" --> "子组件 render"
{%endplantuml%}
3.  使用 key

## 性能检测工具

1. React Develop Tools for Chrome 检测页面使用的 react 代码版本是生产环境版本，如果地址栏 react icon 是黑色，代表当前为生产版本 react，红色则为开发环境 react
2.  chrome performance tab 观察组件挂载、更新、卸载过程及各阶段的时间
  * 确保运行在开发模式下
  * 打开 Chrome 开发者工具，切换到 performance 窗口，单击 Record 按钮开始统计。
  * 在页面执行需要分析的操作，最好不要超过 20 秒，否则导致 chrome 卡死。
  * 单击 stop 按钮结束统计，user timing 查看统计结果
3. why-did-you-update
用来比较 state 和 props 的变化，从而发现组件 render 方法不必要的调用
  * 安装 `npm i why-did-you-update --save-dev`
  * 使用
  ```JavaScript
  import React from 'react';
  if(process.env_NODE_ENV !== 'production') {
    const { whyDidYouUpdate } = require('why-did-you-update')
    whyDidYouUpdate(React)
  }
  ```
