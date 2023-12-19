---
title: vue-cli
tags:
---

<span class='custom-box custom-box-933'>`Vue CLI` is based on webpack</span>

## overview[概述]
Vue CLI is a full system for rapid[快速的] Vue.js development, providing[提供]:
* interactive project scaffolding[脚手架] via `@vue/cli`
* A runtime dependency[依赖项](@vue/cli-server)，依赖项是：
    * Upgradeable[可升级];
    * Built <u>on top of[基于] webpack</u>, with sensible[合理的] defaults[默认值];
    * Configurable via in-project config file;
    * Extensible via plugins
* A rich collection of official plugins integrating[集成] the best tools in the frontend ecosystem[生态系统]
* A full graphical user interface[图形用户界面] to create and manage Vue.js project.

Vue CLI aims to be the standard tooling baseline for the Vue ecosystem
Vue CLI 致力于将 Vue 生态中的工具基础标准化。
It ensures the various build tools work smoothly together with sensible defaults so you can focus on writing your app instead of spending days wrangling[争论，纠结] with configurations.
它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。
At the same time, it still offers the flexibility to tweak[轻微调整] the config of each without the need for ejecting.
与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。

## Components of the System
There are several moving parts(独立的部分) of Vue CLI —— if you look at the source code. you will find that it is a monorepo containing a number of separately（单独） published packages.

### CLI
The CLI(@vue/cli) is a globally installed npm package
CLI (@vue/cli) 是一个全局安装的 npm 包，
and provides the `vue` command in your terminal
提供了终端里的 vue 命令。
It provides the ability to quickly scaffold a new project via `vue create`.
它可以通过 vue create 快速搭建一个新项目，
You can also manage your project using a graphical user interface via `vue ui`.
你也可以通过 vue ui 构建一套图形化界面管理你的所有项目。
We will walk through what it can do in the next few sections of the guide.
我们会在接下来的指南中逐章节深入介绍。

### CLI Service


## Deployment【部署】
### General Guidelines【能用指南】
If you are using Vue CLI along with a backend framework that handles static assets as part of its deployment.
如果使用 `Vue CLI` 与处理静态资源后端框架一起作为部署的一部分，
all you need to do is make sure Vue CLI generates the built files in the correct location,
那么你需要做的是确保`Vue CLI`生成的构建文件在正确的位置，
and then follow the development instruction of your backend framework.
并遵循后端框架的发布方式。

### Previewing Locally【本地预览】
The `dist` directory is meant to be served by an HTTP server(unless you've configured `publicPath` to be a relative value).
`dist` 目录需要启动 HTTP 服务访问（除非你已经将 `publicPath` 配置了一个相对的值），
so it will not work if you open `dist/index.html` directly over `file://` protocol.
所以以 file:// 协议直接打开 dist/index.html 是不会工作的。
The easiest way to preview your production build locally is using a Node.js static file server, for example serve
在本地预览生产环境构建最简单的方式就是使用一个 <span class='custom-box custom-box-339'>Node.js 静态文件服务器</span>，例如 serve：

```bash
 npm install -g serve
# -s flag means serve it in Single-Page Application mode
# which deals with the routing problem below
serve -s dist
```

### Routing with `history.pushState`
If you are using Vue Router in `history` mode, a simple static file server will fail.
如果你在历史模式下使用 Vue 路由器，简单的静态文件服务器将失败。
For example, if you used Vue Router with route for `todos/42`, 
举例，如果你使用 `Vue Router` 为 `todos/42` 定义了一个路由，
the dev server has been configured to respond to `localhost:3000/todos/42` properly,
开发服务器已经配置了相应的 localhost:3000/todos/42 响应，
but a simple static server serving a production build will respond with a 404 instead.
但是一个为生产环境构建架设的简单的静态服务器会返回 404。
To fix that, you will need to configue your production server to fallback(退回) to `index.html` for any request that do not match a static file.
为了解决这个问题，你需要配置生产环境服务器，将任何没有匹配到静态文件的任何请求回退到 index.html。
The Vue Router docs provide configuration instructions for common server setups.
Vue Router 的文档提供了常用服务器配置指引。

