---
title: 再看 node
date: 2019-03-02 10:37:10
tags:
- Node
---
[Node.js 究竟是什么？](https://www.ibm.com/developerworks/cn/opensource/os-nodejs/)
Node 旨在提供一种简单构建可伸缩的网络程序方法
### 认识 Node 交个朋友
Node 是服务器程序，特色就是处理多并发，处理方式则是每个连接会发射一个 Node 引擎进程中的运行事件，不使用锁，所以也不会有死锁的情况，也不生成 os 进程，它的服务器能支持上万并发。
Node 本身运行 V8 Javascript。V8 Javascript 引擎是 Google 用于其 Chrome 浏览器的底层 Javascript 引擎。
### Node 事件驱动编程
没有过度设计，没有面向对象、没有接口，Javascript 是驱动编程语言。有闭包、可以使用匿名函数。它只需要监听事件，调用匿名函数回调，其它由系统处理。
### Node 的包管理工具 NPM
开发过程中通常需要依赖第三方框架，如 Jquery/element-ui 等一系列的第三方框架。这些第三方框架之间或许也是在相互依赖，直接下载、解压、处理相互间的引用似乎非常繁锁，顺应天意，包管理工具应用而生。而 Node 的包管理工具就是 NPM。
### 简单说说 Node 安装
两种方式：
1.  [Node 官网](http://nodejs.cn/download/)下载自己系统相应的包，通过图型界面安装。
2.  `brew install node` 通过命令安装，但前题是你安装过 `HomeBrew`

检查是否安装成功：`node -v` && `npm -v`
### NODE 版本切换 n
1.  n 的安装 `npm install -g n`
    查看 n 是否安装成功 `n -V`
2.  用 n 切换 node 版本时遇到的问题及解决办法
    * 问题描述
    ```
    pna.nextTick is not a function
    ```
    * 解决办法
    ```bash
    $ n lts # Install or activate the latest LTS node release
    $ npm install -g npm
    $ n stable # Install or activate the latest stable  node release
    $ npm install -g @vue/cli
    ```
