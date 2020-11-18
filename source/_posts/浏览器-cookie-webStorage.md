---
title: cookie 与 Web Storage
tags:
- window
- web storage
- browser
date: 2019-06-14 11:41:10
---

先了解下浏览器缓存存储过程的序列图：

```sequence
title: 浏览器缓存序列图
Note left of 浏览器:发起 http 请求
浏览器->Cache: GET /index HTTP/1.1
Note right of Cache: 缓存是空的，\n继续请求 web 服务器
Cache->Web 服务器:GET /index HTTP/1.1
Note left of Web 服务器: 缓存本地
Web 服务器->Cache: HTTP/1.1 200 OK \n Cache-Control: \n Max-age=2000
Cache->浏览器: HTTP/1.1 200 OK \n Cache-Control: \n Max-age=2000 \n Age:0
Note left of 浏览器: 1000 秒后再次发起请求
浏览器->Cache: GET /index HTTP/1.1
Cache->Cache: /index.html \n Max-age=2000 \n Age=1000
Note right of Cache: 缓存生命周期内的
Cache->浏览器: HTTP/1.1 200 OK \n Cache-Control: \n Max-age=2000 \n Age:1000
Note left of 浏览器: 2100 秒后再次发起请求
浏览器->Cache: GET /index HTTP/1.1
Note right of Cache: /index.html \n Max-age=2000 \n Age=2100 \n 缓存过期，向服务器请求
Cache->Web 服务器: GET /index HTTP/1.1 \n If-None-Match
Web 服务器->Cache: HTTP/1.1 304 Not Modified
Note left of Web 服务器:服务器 304，\n 缓存内容没有改动
Cache->浏览器: HTTP/1.1 304 \n Not Modified \n Cache-Control: \n Max-age=2000 \n Age:0
```
`If-None-Match:"4f80f-13c-3a1xb12a"` 服务器用来判断请求资源是否有更新，如果没有更新就返回 304 状态码，浏览器继续使用缓存数据。如有更新，服务器直接返回资源给浏览器。

Web Storage 将少量数据存储于客户端磁盘的技术。实际业务中将不希望从数据库的少量数据通过 web storage 存储，如：用户登录态、用户当前的部分操作如购物车数量增加等。

## Web Storage 使用前验证兼容性
```javascript
if ((typeof Storage) == 'undefined') return;
// localStorage || sessionStorage 程序代码
```

`注意：`
- 由于部分 IE 和 Firefox 浏览器在测试的时候需要上传到服务器或 localhost 上执行，建议本地安装 http-server 测试。

<!-- more -->


## [window.localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)
只读的 localStorage 属性`允许你访问一个 Document 源（origin）的对象 Storage`；其存储的数据能在跨浏览器会话保留，不会随浏览器关闭而消失，数据可分页、跨窗口访问。

localStorage 类似 sessionStorage
但其区别在于：存储在 localStorage 的数据可以长期保留；而当页面会话结束——也就是说，当页面被关闭时，存储在 sessionStorage 的数据会被清除 。

`注意:`
- 无论数据存储在 localStorage 还是 sessionStorage ，它们都特定于页面的协议。
- localStorage 中的键值对总是以字符串的形式存储。 (需要注意, 和 javascript 对象相比, 键值对总是以字符串的形式存储意味着数值类型会自动转化为字符串类型。
- localStorage API 与 javascript 一样基于同源策略（Same-Origin Policy），网页之间的相互调用仅限于相同的网站，同源才能获取同一个 localStorage。（同源策略：相同协议，相同域名IP，相同端口）

### window.localStorage 存取的三种方式
1.  window.localStorage 的对象方法 setItem 与 getItem
> window.localStorage.setItem(key, value)
  var value = window.localStorage.getItem(key)

2.  数组索引
> window.localStorage['key'] = value
> var value = window.localStorage['key']

3.  属性
> window.localStorage.key = value
> var value = window.localStorage.key

### window.localStorage 清除的几种方式
1. 浏览器开发者工具 - application - localStorage - 选中删除项，点击管理工具上的删除
2. `window.localStorage.removeItem(key)`
3. `delete window.localStorage.key`
4. `delete window.localStorage[key]`
5. 清除全部数据 `window.localStorage.clear()`

## [window.sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)
用于保存临时数据，防止用户不小心刷新数据丢失，如大的表单提交。
sessionStorage 属性`允许你访问一个 session Storage 对象`。它与 localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。在新标签或窗口打开一个页面时会在顶级浏览上下文中初始化一个新的会话，这点和 session cookies 的运行方式不同。

> 存储在 sessionStorage 或 localStorage 中的数据特定于该页面的协议。

### window.sessionStorage 存取的三种方式
1.  window.sessionStorage 的对象方法 setItem 与 getItem
> window.sessionStorage.setItem(key, value)
  var value = window.sessionStorage.getItem(key)

2.  数组索引
> window.sessionStorage['key'] = value
> var value = window.sessionStorage['key']

3.  属性
> window.sessionStorage.key = value
> var value = window.sessionStorage.key

### window.sessionStorage 清除的几种方式()
1. 浏览器开发者工具 - application - sessionStorage - 选中删除项，点击管理工具上的删除
2. `window.sessionStorage.removeItem(key)`
3. `delete window.sessionStorage.key`
4. `delete window.sessionStorage[key]`
5. 清除全部数据 `window.sessionStorage.clear()`
6. 关闭浏览器或当前窗口

## [Cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie)
Cookie 是一个请求首部，其中含有先前由服务器通过 Set-Cookie 首部投放并存储到客户端的 HTTP cookies。
这个首部可能会被完全移除，例如在浏览器的隐私设置里面设置为禁用cookie。

> *  Cookie 数据始终在同源的http请求中携带（即使不需要），就会在浏览器和服务器间来回传递，安全性低。而 Web Storage 不会把数据发给服务器，仅存在本地
> *  以键值对应组合保存数据


## Web Storage 与 Cookie 对比表
{% table table-striped %}
|         | 属性 | 访问对象 | 过期时间 | 存储大小 |
| ------------- |-------------| -----| ------| ---- |
| window.localStorage | / | Document 源（origin）的对象 Storage(local storage) | 永久存储数据，浏览器关闭后数据不丢失，除非主动执行删除指令删除 |HTML5 规范中，容量由客户端程序（浏览器）决定，通常 1mb~5mb |
| window.sessionStorage |   /  | session storage 对象 | 页面关闭，会话结束，session 清除 |HTML5 规范中，容量由客户端程序（浏览器）决定，通常 1mb~5mb |
| Cookie | 标识用户用户身份而存储在用户本地终端上的数据（通常经过加密） | / | Expires/Max-Age 之前有效 |4kb|
{% endtable %}
