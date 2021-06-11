---
title: JavaScript-事件对象
date: 2021-01-19 16:52:04
tags:
- JavaScript
---

事件流描述从页面中接受事件的顺序
## 事件冒泡 Event bubbling
当一个元素接收到事件时，会把它接收到的事件逐级向上传播给它的祖先元素，一直传到【顶层对象】
【顶层对象】
IE9以上、Chrome、Safari 事件冒泡顶层对象是 window
IE7/IE8 事件冒泡顶层对象是 Document

事件冒泡对所有浏览器默认存在，且由元素 html 结构决定。所以即使定位或浮动脱离文档流的元素也是存在冒泡现象的
使用事件源对象的事件属性绑定事件函数及使用html标签事件属性绑定事件函数的事件流都是冒泡

[comment]: <> (## 事件捕获 Event capturing)
