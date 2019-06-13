---
title: CSS 面试题
date: 2019-03-25 15:35:18
tags:
- css
---
- [ ] 使用css实现一个自适应浏览器大小并且宽和高的比是2:1，有几种实现方法？
> * width 与 height 百分比是根据父元素计算的，`<body>` 是根据宽一般是浏览器的宽，高一般为 0，不同浏览器有所差异，这里不细说。
> * padding 百分比是相对于包含块的宽度
指定一个值时 该值指定四个边的内边距
- [x] 用 padding 撑 50% 的高

<!-- more -->

```CSS
.father {
  width: 100%; // 根据父元素计算
  padding: 25% 0; // 根据该块的宽计算其占用宽间
  background-color: #f90;
}
.son {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #90f;
}
```
- [x] 使用 vw 为单位
> * vw: viewport width, 1vw = 视窗宽度的 1%
> * vh: viewport height, 1vh = 视窗高度的 1%
> * vmin: vh 与 vw 哪个小取哪个
> * vmax: vh 与 vw 哪个大取哪个

```CSS
.warp {
  width: 100vw;
  height: 50vw;
  background-color: #f90;
}
```
