---
title: 搭建 http 环境
tags:
  - http
date: 2019-05-03 11:01:59
---


## OpenResty
OpenResty 基于 Nginx 的 “强化包”，不仅**支持 http/https**，还特别<u>集成了<font color="red">脚本语言 Lua 简化 Nginx 二次开发</font>。**方便搭建动态网关**，能够当成应用容器来编写业务逻辑</u>

## wireshark
wireshark 抓包工具，能够截获在 TCP/IP 协议栈中传输的所有流量，并按协议类型、地址、端口等任意过滤。

## 项目运行
```
$ cd http_study/www/
$ openresty -p `pwd` -c config/nginx.conf

# 停止项目
$ openresty -s quit -p `pwd` -c config/nginx.conf
```

<font color="red">mac 使用 run.sh 替换 windows 的 run.bat</font>
<font color="darkgreen">127.0.0.1 为 loopback 环回地址 wireshark(windows) 中选择 Npcap loopback Adapter; wireshark(mac) 选择 loopback: lo0</font>

<!--more-->


## 踩坑记
### Error: Failed to connect to raw.githubusercontent.com port 443:Failed to download resource "openresty-openssl111--patch"
> 域名被 DNS 污染了，需要重新设置下本地的 hosts 域名解析顺序：<font color="red">浏览器缓存 -> 本地 hosts -> 外部 DNS</font>

```
$ vim /etc/hosts
# 添加
199.232.4.133 raw.githubusercontent.com
```

### Error: An unexpected error occurred during the `brew link` step 解决办法
```
# 创建 Frameworks 文件夹
$ sudo mkdir /usr/local/Frameworks
# 更改目录权限
$ sudo chown -R $(whoami) /usr/local/Frameworks
```

### The capture session could not be initiated on interface 'lo0' (You don't have ...
> 打开终端输入命令sudo chmod 777 /dev/bpf*
