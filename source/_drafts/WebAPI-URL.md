---
title: WebAPI-URL
tags:
- WebAPI
---

URL api 用于解析、构造、规范化、编码[URL](/2023/02/24/URL/)
提供了允许轻松阅读和修改 URL 组件的属性
<span color="#a33">如果浏览器不支持 URL 构造函数，可使用 window 中的 window.URL 属性</span>
```JavaScript
const url = new URL(url, [base])
```

