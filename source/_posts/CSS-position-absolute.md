---
title: CSS position absolute
date: 2020-10-12 12:40:01
tags:
- CSS
---

## position: absolute 与 float 一样，使用后脱离文档流。

```html
<div style="width: 300px; height: 100px; background: #efefef;">
  <div style="width: 100px; height: 50px; background: #390;"></div>
</div>
```
![position-absolute-1](/images/css/position-absolute-1.jpg)
<!--more-->

> 1. **`具有破坏性`** 子元素加入 position: absolute; 至父元素高度塌陷
```html
<div style="width: 300px; height: 100px; background: #efefef;">
  <div style="width: 100px; height: 50px; background: #390; position: absolute"></div>
</div>
```
![position-absolute-2](/images/css/position-absolute-2.jpg)

> 2. **`具有包裹性`** 父元素使用 position: absolute，父元素包裹子元素
```html
<div style="width: 300px; height: 100px; background: #efefef; position: absolute">
  <div style="width: 100px; height: 50px; background: #390;"></div>
</div>
```
![position-absolute-3](/images/css/position-absolute-3.jpg)


position 与 margin 的关系
