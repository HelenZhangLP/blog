---
title: http 请求状态码
date: 2019-06-12 10:38:02
tags:
- 网络请求
---
## [http 请求状态码 200](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200)
> 请求成功，默认状态下 200 响应可以被缓存
* GET:  已经取得资源，并将资源添加到响应的消息体中。
* HEAD: 响应的消息体为头部信息。
* POST: 响应的消息体中包含此次请求的结果。
* TRACE: 响应的消息体中包含服务器接收到的请求信息。
* PUT 和 DELETE 的请求成功通常并不是响应 200 OK 的状态码，而是 204 No Content 表示无内容(或者  201  Created表示一个资源首次被创建成功)。

<!--more-->

## [http 请求状态码 206](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/206)
> HTTP 206 Partial Content 成功状态响应代码表示请求已成功，并且主体包含所请求的数据区间，该数据区间是在请求的 Range 首部指定的。
```
Response Headers
Accept: */*
Accept-Encoding: identity;q=1, *;q=0
Accept-Language: zh-CN,zh;q=0.9
Connection: keep-alive
Host: cktvideo-1252817906.cos.ap-guangzhou.myqcloud.com
If-Range: "640f0886f386c9c253a95c5d1c01ca16-163"
**`Range: bytes=9895936-170855393`**
Referer: https://cktvideo-1252817906.cos.ap-guangzhou.myqcloud.com/gaokeyan1.mp4
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1
```
> 如果只包含一个数据区间，那么整个响应的 Content-Type 首部的值为所请求的文件的类型，同时包含  Content-Range 首部。
```
Response Headers(10)
Accept-Ranges: bytes
Connection: keep-alive
Content-Length: 170855394
** Content-Range: bytes 0-170855393/170855394
** Content-Type: video/mp4
Date: Wed, 12 Jun 2019 02:34:48 GMT
ETag: "640f0886f386c9c253a95c5d1c01ca16-163"
Last-Modified: Sun, 30 Dec 2018 17:44:16 GMT
Server: tencent-cos
x-cos-request-id: NWQwMDY0YzhfMWNiMjk0MGFfNThlZl83YTg5NWY=
```

## [http 请求状态响应码 304](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/304)
> 无需再次传输请求的内容，也就是说可以使用缓存的内容。这通常是在一些安全的方法（safe），例如 GET 或 HEAD 或 在请求中附带了头部信息： If-None-Match 或 If-Modified-Since。
```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Connection: keep-alive
Host: cktvideo-1252817906.cos.ap-guangzhou.myqcloud.com
** If-Modified-Since: Sun, 30 Dec 2018 17:44:16 GMT
** If-None-Match: "640f0886f386c9c253a95c5d1c01ca16-163"
Range: bytes=0-1048575
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1
```
