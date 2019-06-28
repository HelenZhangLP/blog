---
title: cookie、sessionStorage、localStorage
tags:
- window
date: 2019-06-14 11:41:10
---

## [window.localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)
只读的 localStorage 属性`允许你访问一个 Document 源（origin）的对象 Storage`；其存储的数据能在跨浏览器会话保留。
localStorage 类似 sessionStorage
但其区别在于：存储在 localStorage 的数据可以长期保留；而当页面会话结束——也就是说，当页面被关闭时，存储在 sessionStorage 的数据会被清除 。
应注意，无论数据存储在 localStorage 还是 sessionStorage ，它们都特定于页面的协议。
另外，localStorage 中的键值对总是以字符串的形式存储。 (需要注意, 和js对象相比, 键值对总是以字符串的形式存储意味着数值类型会自动转化为字符串类型
<!-- more -->
## [window.sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)
sessionStorage 属性`允许你访问一个 session Storage 对象`。它与 localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。在新标签或窗口打开一个页面时会在顶级浏览上下文中初始化一个新的会话，这点和 session cookies 的运行方式不同。

> 存储在 sessionStorage 或 localStorage 中的数据特定于该页面的协议。

## [Cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie)
Cookie 是一个请求首部，其中含有先前由服务器通过 Set-Cookie 首部投放并存储到客户端的 HTTP cookies。
这个首部可能会被完全移除，例如在浏览器的隐私设置里面设置为禁用cookie。

> Cookie 数据始终在同源的http请求中携带（即使不需要），就会在浏览器和服务器间来回传递，localStorage && sessionStorage 不会把数据发给服务器，仅存在本地

{% table table-striped %}
|         | 属性 | 访问对象 | 过期时间 |
| ------------- |-------------| -----| ------|
| window.localStorage | / | Document 源（origin）的对象 Storage(local storage) | 永久存储数据，浏览器关闭后数据不丢失，除非主动删除 |
| window.sessionStorage |   /  | session storage 对象 | 页面关闭，会话结束，session 清除 |
| Cookie | 标识用户用户身份而存储在用户本地终端上的数据（通常经过加密） | / | Expires/Max-Age 之前有效 |
{% endtable %}
