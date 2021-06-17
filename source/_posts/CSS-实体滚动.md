---
title: CSS 动画实现滚动
date: 2020-04-23 13:04:18
tags:
- CSS
---

### CSS 动画实现滚动

> animation: animation-name（动画名称） animation-duration（动画完成一个周期花费的秒、毫秒）

```css
.marquee {
  width: 1070px;
  height: 30px;
  white-space: nowrap;
  animation: roll-animation 50s
}

// 规定动画
@keyframes animation-roll {
  from: {
    z-index: 50px;
  }

  to: {
    z-index: -100px;
  }
}
```
