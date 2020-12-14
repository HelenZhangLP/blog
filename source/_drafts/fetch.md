---
title: fetch
date: 2020-12-07 11:28:59
tags:
- web api
- fetch api
---

fetch 与 jQuery.ajax() 的不同
1.  当接收一个代表错误的 HTTP 状态码（即使 HTTP 状态码是 404 或 500）时，从 fetch() 返回的 Promise **不会标记为 reject**。它会返回 Promise 状态 resolve ( resolve 返回值属性 ok 值为 false)。当且仅当网络故障或请求被阻止时返回 reject。
2.  fetch 可以接受跨域 cookies；可以使用 fetch 建立跨域会话
3.  fetch 不会发送 cookies
