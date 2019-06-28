---
title: cookie sessionStorage localStorage
tags:
- window
---

Storage
## [window.localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)
只读的 localStorage 属性允许你访问一个 Document 源（origin）的对象 Storage；其存储的数据能在跨浏览器会话保留。
localStorage 类似 sessionStorage
但其区别在于：存储在 localStorage 的数据可以长期保留；而当页面会话结束——也就是说，当页面被关闭时，存储在 sessionStorage 的数据会被清除 。
应注意，无论数据存储在 localStorage 还是 sessionStorage ，它们都特定于页面的协议。
另外，localStorage 中的键值对总是以字符串的形式存储。 (需要注意, 和js对象相比, 键值对总是以字符串的形式存储意味着数值类型会自动转化为字符串类型).
| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |
|        | 属性 |     | |
|:----:|:----:|
| localStorage | 只读 |||
||||
