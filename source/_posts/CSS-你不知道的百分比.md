---
title: CSS 你不知道的百分比
date: 2020-09-21 15:33:02
tags:
- CSS
---

```javascript
document.body.offsetHeight
// 0
```
* 宽设置百分比
> 根据父元素的宽，浏览器端，根元素 html、body 宽为屏可视区的宽

* 高设置百分比
> 根据父元素的高运算，如上 code ，浏览器 body 未设置高，所以设置百分比要特别注意。

```CSS
html, body {
  height: 100%;
}
```

* padding 设置百分比
> 根据父元素宽运算
