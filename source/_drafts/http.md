---
title: http
tags:
- http
---

## 认识几个概念
### HTTP(HyperText Transfer Protocol)
> HTTP(HyperText Transfer Protocol) 超文本传输**协议**
  H - Hypertext 超文本
  T - Transfer 传输
  P - Protocol 协议
  用在计算机世界里的协议，用于<font color="orange">两点(请求方和响应方)</font>之间<font color="skyblue">**传输**文字、图片、音频、视频等超文本数据</font>的**约定和规范(protocol)**

<!--more-->

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
