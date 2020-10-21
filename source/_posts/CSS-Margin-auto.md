---
title: CSS Margin auto
date: 2020-10-19 14:06:30
tags:
- CSS
---

众所周知，margin: auto 可以使`块级元素`水平居中。这里要探讨的是为什么会居中，原理是什么？翻看了[MDN](https://wiki.developer.mozilla.org/zh-CN/docs/Web/CSS/margin)，相关描述只有一句，“让浏览器自己选择一个外边距”。

自动占满可用空间，可用空间平均分配，占满后元素居中，如下图。
![margin:auto](/images/css/margin-auto-1.jpg)

<!--more-->

## margin: auto + position: absolute 实现居中
首先声明：只标题代码是无法实现居中的，下面进行一次解析

1.  以下两张图片，说明了，当元素设置 position: absolute，就相当于复制其定位元素的可用空间，但当前元素的可用空间仍为当时元素 content width + padding + border + margin
![margin:auto](/images/css/margin-auto-2.jpg)
---
![margin:auto](/images/css/margin-auto-3.jpg)

2.  left: 0; right: 0; 相当于可用空间扩展为其定位元素的可用空间(也就是上图中的 200px 或 1280px)
3.  margin: auto 进行平均分配可用空间，实现居中
![margin:auto](/images/css/margin-auto-4.jpg)
