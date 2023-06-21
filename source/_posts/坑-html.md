---
title: 坑-html
date: 2018-06-26 15:14:20
tags:
- Html
- 坑
---

## web 页面生命周期
### DOMContentLoaded
纯 html 被完全加载解析时，DOM 加载完毕，样式表、图片等其它资源可以未加载时，触发 DOMContentLoaded
```javascript
document.addEventListener('DOMContentLoaded', ready)
```
### load
浏览器加载了所有的资源，包括图像、样式表等
### beforeunload/unload
离开当前页面时触发
## 当前页不需要点击跳转，其它页面链接至指定位置
`<a id="factoryPattern" href="#factoryPattern">工厂模式——<font color="#f99">解决复用</font></a>`
