---
title: react history
tags:
- react
---

传统 ui 操作关注太多细节

{%plantuml%}
@startmindmap
+ DOM API
++ Selectors
+++ Basics
+++ Hierarchy
+++ Basic Filters
++ Attributes/CSS
+++ Attributes
+++ css
+++ Dimensions
++ Manipulation
+++ Copying
+++ DOM Insertion, Around
+++ DOM Insertion, Inside
+++ DOM Removal
++ Traversing
+++ Filtering
+++ Miscellaneous Traversing
+++ Tree Traversal
@endmindmap
{%endplantuml%}

<!--more-->

React 整体刷新页面，不需要关心细节

应用程序状态分散在各处，难以追踪和维护

{%plantuml%}
@startmindmap
+ 1 个新概念
+ 4 个必须的 api
+ 单向数据流
+ 完善的错误提示
@endmindmap
{%endplantuml%}

广度优先、分层比较
根节点开始比较
属性变化及顺序
节点类型发生变化
节点跨层移动

两个假设
1. DOM 结构相对稳定
2. 类型相同的兄弟节点可以被唯一标识
