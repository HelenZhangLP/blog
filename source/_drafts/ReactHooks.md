---
title: ReactHook
tags:
- react
---

函数组件实现动态组件类组件的功能
React 最新版本 v18.2，Hook 是 React 16.8 新增特性。可以在不编写 class 的情况下使用 state 以及其他 React 特性。
引入 Hook 的动机——解决看起来不相关的问题
Hoos 是执行函数，产生函数上下文

## useState
在函数组件中使用状态，并且后期基于状态修改，进而更新组件
```JavaScript
useState(initialValue)
```

### 在组件之间复用状态逻辑很难附加到组件的途径
没有提供将可复用性行为
<u>React 需要为共享状态逻辑提供更好的原生途径</u>

### 复用组件难以理解

### 难以理解的 class
