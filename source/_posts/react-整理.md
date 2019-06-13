---
title: react 整理
date: 2019-03-19 18:06:55
tags:
- javaScript
- react
---
### React 列表渲染
#### keys
> 给数组中的每个元素赋予确定标识，当 DOM 中的某些元素被增加或删除时，可以帮助 react 识别发生变化的元素。

```javaScript
  items.map((item, index) => {
    <li key={item.id}>{item.text}</li>
    {/* <li key={index}>{item.text}</li> 没有 id 用索引赋 key */}
  })
```
- [ ] ***` 列表排序时，不建议用索引 index 赋 key 值，因为会导致渲染很慢 `***
- [ ] ***` map() 方法的内部调用元素时，一定要为每一个元素加上 key。 `***
- [ ] ***` map() 方法的内部调用元素时，一定要为每一个元素加上 key。 `***
### React state
> React 中把组件看成一个状态机（state machines）。React 里，通过更新组件 state 重新渲染用户界面，不需要操作 DOM，类组件使用 props 调用基础构造函数。

```mermaid
graph LR;
    用户界面交互-->状态;
    状态-->UI渲染;
    UI渲染-->|用户界面与数据保持一致|数据;
```
this.setState 之后发生了什么？ React 将传入的参数与组件当前状态合并，触发调和过程（Reconciliation），经过调和过程，React 重新构造 dom ，并将新老状态进行对比，最小化渲染。

### React 生命周期渲染
```mermaid
graph TM;
  componentWillMount-->render;
  render-->componentDidMount;
  componentDidMount-->render
```
