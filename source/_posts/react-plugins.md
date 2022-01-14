---
title: react plugins
date: 2021-05-25 10:15:56
tags:
- react
---

## [<font color='#f33'>Variable @root-entry-name is undefined</font>](https://ant.design/docs/react/customize-theme-variable-cn)
> 为了实现 CSS Variable 并保持原始用法兼容性，我们于 dist/antd.xxx.less 文件中添加了 @root-entry-name: xxx; 入口注入以支持 less 动态加载对应的 less 文件。一般情况下，你不需要关注该变化。但是，如果你的项目中直接引用了 lib|es 目录下的 less 文件。你需要在 less 入口处配置 @root-entry-name: default; （或者 @root-entry-name: variable;）以使 less 可以找到正确的入口。

## react-dev-inspector
从页面上识别 react 组件，直接跳转到本地 ide 代码片段上
> 可以自定义开关键值，或者在devtool里面通过window.REACT_DEV_INSPECTOR_TOGGLE()开启

