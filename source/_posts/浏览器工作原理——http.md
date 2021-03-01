---
title: 浏览器工作原理——http
date: 2020-11-12 16:19:06
tags:
- browser
- 浏览器工作原理
---

XHR -> Request Headers -> view source
```
GET /shakespeare/jsd/exchange_rates/current HTTP/1.1
Host: www.jianshu.com
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.jianshu.com/p/6dbcc3aff98b
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: __yadk_uid=b5C1W8FTbvn2CMErMOWLRvRBfqtcfZyR; _ga=GA1.2.1361763253.1581860875; __gads=ID=e22e0402239bcdd8-22e9450047c200f4:T=1593494469:RT=1593494469:S=ALNI_MbgAdC1pDH1wBUKwaWC6teLb00cyg; read_mode=day; default_font=font2; locale=zh-CN; UM_distinctid=175537082be1a1-08e93312a69e19-4d112a29-38400-175537082bf1b5; CNZZDATA1278917561=2017572215-1603418142-https%253A%252F%252Fwww.jianshu.com%252F%7C1603447023; sensorsdata2015jssdkcross=%7B%22distinct_id%22%3A%2216e7305e1e716c-046300c902b66b-123b6a5d-1024000-16e7305e1e881a%22%2C%22%24device_id%22%3A%2216e7305e1e716c-046300c902b66b-123b6a5d-1024000-16e7305e1e881a%22%2C%22props%22%3A%7B%22%24latest_referrer%22%3A%22https%3A%2F%2Fwww.baidu.com%2Flink%22%2C%22%24latest_traffic_source_type%22%3A%22%E8%87%AA%E7%84%B6%E6%90%9C%E7%B4%A2%E6%B5%81%E9%87%8F%22%2C%22%24latest_search_keyword%22%3A%22%E6%9C%AA%E5%8F%96%E5%88%B0%E5%80%BC%22%7D%7D; _gid=GA1.2.1053313438.1605150552; _m7e_session_core=559108e9b8dcd6adff1770cd4662ce96; signin_redirect=https://www.jianshu.com/p/6dbcc3aff98b; Hm_lvt_0c0e9d9b1e7d617b3e6842e85b9fb068=1604707994,1604975982,1605150555,1605156601; _gat=1; Hm_lpvt_0c0e9d9b1e7d617b3e6842e85b9fb068=1605170209
```

<!--more-->

HTTP 是一种允许浏览器向服务器发起资源的请求的协议，是 Web 的基础。

## 浏览器发起 HTTP 请求流程
1.  构建请求
> GET /shakespeare/jsd/exchange_rates/current HTTP/1.1

2.  查找缓存
浏览器缓存是在本地保存资源副本，以供下次请求时直接使用的技术。如果浏览器资源的副本存在，请求被拦截，返回副本，结束请求。从而缓解服务器压力，提升性能。对于网站来说，缓存是实现资源加载最重要的部分。

3.  准备 IP 和 端口 并等待 TCP 队列
如果浏览器请求缓存资源失败，就会进入网络请求。
怎么通过 url 获取 ip 地址和端口号呢？ip 地址是数字标识，从请求里面可获取，如下请求 ip 地址为 47.92.108.93：
```
Request URL: https://www.jianshu.com/shakespeare/jsd/exchange_rates/current
Request Method: GET
Status Code: 200 OK
Remote Address: 47.92.108.93:443
Referrer Policy: no-referrer-when-downgrade
```
ip 数字难记。我们一般记的是域名，如上请求 `jianshu.com`，需要使用 **域名系统** （Domain Name System，DNS）将域名和ip进行映射。浏览器有`DNS`数据缓存服务。**`总结下来：第一步浏览器从 DNS 缓存数据里面查找 ip，如果没有再请求 DNS 返回域名对应的 ip`**
端口号在没有指定的情况下是 80 端口，像上面的案例中，端口号为 443。

> chrome 机制，同一个域名最多只建立 6 个 tcp 链接，多于 6 个请求会进入排队等待状态，走到进行中的请求完成。

4.  建立 TCP 连接
三次握手，传输数据，断开链接

5.  发送 HTTP 请求
建立 TCP 链接后浏览器和服务器便可以通信了。
* 发送请求行，包括请求方法、请求的 URI(Uniform Resource Identifier)统一资源标识、HTTP 版本协议
> GET /shakespeare/jsd/exchange_rates/current HTTP/1.1

上面的案例是 get 请求，除此之外还有 post 请求，即发送一些数据给服务器，浏览器会把需要发送给服务器的数据通过`请求体`发送。
发送请求行之后，浏览器还要以`请求头`形式发送一些其他信息，把浏览器的基本信息告诉给服务器。
> User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36
Host: www.jianshu.com
Cookie: \__yadk_uid=b5C1W8FTbvn2CMErMOWLRvRBfqtcfZyR; \_ga=GA1.2.1361763253.1581860875;

## 服务器处理 HTTP 请求
1.  返回请求
可以通过命令 `curl -i https://www.jianshu.com/p/6dbcc3aff98b` 查看返回的请求数据，如响应行，响应头，响应体等数据
`curl -I jianshu.com` -I 只获取响应头和响应行数据。

Response Headers
```
HTTP/1.1 200 OK
Server: Tengine
Date: Thu, 12 Nov 2020 08:36:49 GMT
Content-Type: application/json; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
ETag: W/"4a327be29535622a46f3b610df2a02c1"
Cache-Control: max-age=0, private, must-revalidate
Set-Cookie: locale=zh-CN; path=/
Set-Cookie: _m7e_session_core=559108e9b8dcd6adff1770cd4662ce96; domain=.jianshu.com; path=/; expires=Thu, 12 Nov 2020 14:36:49 -0000; secure; HttpOnly
X-Request-Id: 503c9b5a-3190-4e2a-9060-ead15564caa1
X-Runtime: 0.007053
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Content-Encoding: gzip
```
**响应行** `HTTP/1.1 200 OK` 协议版本与状态码。我们这里的状态码是 200 表示处理成功，遇到其它无法处理或处理出错的会有其它的状态码，如：没有找到返回页面 404。301 状态码表示重定向，需要重定向的网址包含在 **响应头** Location 字段中。
服务器随响应向浏览器发送 **响应头** 信息。包括了：
`Date: Thu, 12 Nov 2020 08:36:49 GMT` 返回数据的时间；
`Content-Type: application/json; charset=utf-8` 返回数据的类型；
`Set-Cookie: _m7e_session_core=559108e9b8dcd6adff1770cd4662ce96; domain=.jianshu.com; path=/; expires=Thu, 12 Nov` 服务器在客户端保存的 cookie。

2.  断开链接
通常情况下，服务向浏览器返回数据后，会断开链接。`Connection: keep-alive` 表示 TCP 连接在发送后保持打开状态，浏览器可以通过同一个 TCP 发送请求。**保持 TCP 连接可以省去下次请求建立连接的时间，提升资源加载速度**
