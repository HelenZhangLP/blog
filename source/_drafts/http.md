---
title: http
tags:
- Http
---

## Idempotent Method
A request method is considered "idempotent"
一个请求方法被认为**幂等**
as the effect for a single such request
作为单个此类请求的效果
幂等（idempotent）：同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的。幂等方法不应该有副作用。
GET, HEAD, PUT, DELETE 等方法都是幂等的，所有 safe 方法都是幂等的。

## 认识几个概念
### HTTP(HyperText Transfer Protocol)
> HTTP(HyperText Transfer Protocol) 超文本传输**协议**
  H - Hypertext 超文本
  T - Transfer 传输
  P - Protocol 协议
  用在计算机世界里的协议，用于<font color="orange">两点(请求方和响应方)</font>之间<font color="skyblue">**传输**文字、图片、音频、视频等超文本数据</font>的**约定和规范(protocol)**

### DNS(Domain Name System)
> DNS(Domain Name System) 域名系统
  用有意义的名字来作为 IP 地址的等价替代

### URI(Uniform Resource Identifier)
URI(Uniform Resource Identifier) 统一资源标识符，可以唯一地标记互联网上的资源
URI 三个基本构成：
1.  协议名 访问该资源应当使用的协议 http
2.  主机名 即互联网上主机的标记，可以是域名(Domain Name)或IP地址
3.  路径 资源在主机上的位置 “/” 分隔多目录

### HTML（HyperText Markup Language）
HTML 运行在浏览器，由浏览器解析
HTML 超文本标语言，描述超文本文档
HTML 两个主要的标准 HTML4 和 HTML5
HTML 广义上是指 HTML/JavaScript/CSS等前端技术的组合，能够实现比传统静态更丰富的动态页面

## 关于 HTTP（HyperText Transfer Protocol)
HTTP 超文本传输协议
HTTP 是一个应用层协议，基于 TCP/IP，在网络的任意两点（请求方和应答方）间传输超文本（图片、文字、视频、音频等）
HTTP 协议中的两个端点**请求方**和**应答方**
请求方 —— web 浏览器 user agent
应答方 —— web 服务器 存储着网络的静态与动态资源

HTTP 需要 TCP/IP 实现寻址、路由和可靠的数据传输
HTTP 需要 DNS 协议实现对互联网上主机的定位、查找

HTTP over TCP/IP
HTTP over SSL/TLS

HTTP 有多个版本，目前应用最广的是 HTTP/1.1，但 HTTP/1.1 性能难以满足高流量网站，于是推出了 HTTP/2 HTTP/3

## 请求方法
###  HTTP PUT  
PUT 创建一个新的资源或用请求的有效载荷替换目标资源的表示。
调用一次与连续调用多次效果是相同的。
`响应` 如果目标资源没有当前的表示，并且 PUT 方法成功创建了资源，那么源服务器必须返回 201 Created 通知用户代理，资源已创建
如果目标资源已存在，并且依照请求中封装的表现形式成功进行了更新，那么，源服务器必须返回 200 OK 或 204 No Content 表示请求成功完成

### HTTP DELETE
HTTP DELETE 方法用于删除指定资源
删除成功的状态码：
* 状态码 202 Accept 请求操作可能会成功执行，但尚未开始执行；
* 状态码 204 No Content 操作已执行，但没有进一步相关信息；
* 状态码 200 OK 操作已执行，并且响应中提供了相关状态的描述信息；