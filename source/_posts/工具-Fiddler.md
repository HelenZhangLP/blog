---
title: 开发工具-Fiddler
date: 2020-03-06 16:53:53
categories:
- 工具
tags:
- develop-tools
---

### [安装教程推荐](https://www.cnblogs.com/yyhh/p/5140852.html)

### 坑一，配置后手机移动端不能正确连接抓包
> 1、fiddler 配置完成后要重启后才生效
> 2、重启打开后，File-Capture Traffic

### 坑二，Inspector - syntaxView 乱码
> 工具栏选中 `Decode`
<!--more-->

### 坑三，!SecureClientPipeDirect failed: System.Security.Authentication.Authenticatio
> 以 ios 为例，`设置 - 通用 - 关于本机 - 证书信任设置 - DO_NOT_TRUST_FiddlerRoot`
