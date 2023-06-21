---
title: Vue N 问
date: 2020-10-21 18:36:52
tags:
- Vue
---

## 从头实现 Vue 高级性能（Vue Workshop:Advanced Features from the Groupd up）
### Intro
### Fundamentals: Reactivity
### Fundamentals: Writing Plugins
### Fundamentals: Render Function
### State Management
### Routing
### Form Validation
### i18n

### vue 的特点

* 轻量级框架
> 自动追踪依赖模板表达式和计算属性，提供 MVVM (Mode-View-ViewMode) 双向绑定和可组合的组件系统，API 简单、灵活、易上手

* 双向数据绑定
> 双向数据绑定是 vue.js 的核心，其专注于 View 层。ViewMode 负责连接 View 和 Mode，保证视图和数据一致。 ViewMode 要做的是：MODE Listen and Data Bindings

* 指令
> vue.js 与 页面交互主要通过指令完成，指令的作用是当表达式值改变时，相应将其形为应用到 DOM 上

* 组件化

* 客户端路由

* 状态管理

<!--more-->
---

### 使用 npm 方式搭建 vue 单页面应用
1.  验证 npm 是否正确安装
```bash
$   npm -v  #查看 npm 版本
```
2.  安装 vue
```bash
$   npm install vue
```
3.  安装 vue-cli 脚手架
```bash
$   npm install --global vue-cli
```
4.  初始化一个基于 webpack 的新模板
```bash
$   vue init webpack my-project
```
5.  启动项目，验证搭建结果
```bash
$   cd my-project
$   npm install  #安装依赖包
$   npm run dev
```
  **`看到下图，恭喜你，环境搭建成功`**

![vue init page](https://www.runoob.com/wp-content/uploads/2017/01/56219E04-D156-43EC-AC59-BFE7E38A62C3.jpg)

---
