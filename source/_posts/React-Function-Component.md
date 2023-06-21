---
title: React 函数组件
date: 2023-06-15 13:11:43
tags:
- React
- Function
---


## 函数组件
返回 jsx 视图（JSX 元素，VirturalDOM 虚拟对象）的函数。
<span class='custom-box custom-box-939'>特性：</span>
* 函数组件是静态组件，父组件更新，子组件才会更新，无法基于组件内部的操作控制更新。<i>不具备”[状态](/2021/03/09/React-State/)、[ref](/2021/05/11/React-Refs/)、[周期函数]()等“</i>
* 函数组件有属性、插槽、父组件可以控制重新渲染；
* 渲染流程简单，渲染速度快；
* 基于 FP（函数式编程）思想设计，提供更细粒度的逻辑组织和复用。

设置的属性值不是<span class='custom-box custom-box-933'>字符串</span>格式，<span class='custom-box custom-box-393'>要基于`{}`进行嵌套</span>
### 定义函数组件
```JavaScript
function demo1() {
    return <>
        <h3>这是一个函数组件</h3>
    </>
}
export default demo1
```

### 调用函数组件
```javascript
  <ComponentDEMO className="App-header" style={{color: '#a33'}} title="function component demo" data={[1,2,3]} times={3} />
```