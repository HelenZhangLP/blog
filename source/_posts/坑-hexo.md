---
title: Hexo 坑
date: 2019-01-31 16:44:18
categories:
- hexo
tags:
- hexo
---

## 浏览器样式缓存问题
这个其实也不只是 hexo 页面会遇到的问题，所有页面修改样式后都会有样式缓存的问题，解决办法如下：
> 浏览器端 ctrl(command) + shift + r
> 手机浏览器同样要清除缓存，如 safari history -> clear all 

## master 分支中修改样式，然后发布，不是必须提交远程

## hexo db.json
> db.json 为缓存文件，如果 theme 有修改，请删除 db.json。然后再执行 `hexo g`

## hexo 启动草稿服务
```
$ hexo g && hexo s --drafts
```

## hexo deploy 没有反应
`检查` \_config.yml 中 deploy 配置

## 访问页面空白
`检查` themes

## 断行，在首页展示缩略信息，该方法能被 hexo 更好识别
```html
<!--more-->
```
```
auto_excerpt:
  enable: true
  length: 150
```

## ERROR - No layout: index.html
```
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
WARN  No layout: index.html
```
<!-- more -->

`解决办法：`
```
$ git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean
```
> Modify theme setting in \_config.yml to ocean
`theme: ocean`

## ERROR - 发布到 git 服务器访问出现空白页或无主题样式
> 原因是发布到 github 上的项目名称要与个人github的用户名一致，且加后缀 .github.io，如 'helenzhanglp.github.io'

## ERROR - 博客中添加 gitalk
> *## Error Error: Not Found. ##*
`解决办法：`
```
gitalk:
  enable: true
  clientID: 8e7e6dda81936172806e # GitHub Application Client ID
  clientSecret: 97f71b6bbdf731bc650ec39212061882b8f36e71 # Client Secret
  repo: 'blog' # Repository name 仓库不能为私有仓库 Private Repository
  owner: HelenZhangLP  # GitHub ID
  admin: HelenZhangLP # GitHub ID
```

## ERROR - theme 样式修改，先需要先提交至 github，再 deploy

## Error: Spawn failed
> fatal: unable to access 'https://github.com/HelenZhangLP/helenzhanglp.github.io.git/': Received HTTP code 502 from
 proxy after CONNECT
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html

`解决办法：`
\_config.yml 中 修改 deploy，https 链接换成 ssh
deploy:
  type: git
  repo: git@github.com:*/*.github.io.git

## hexo 中加入 sequence diagram，安装 hexo-filter-sequence 后不显示
```
sequence-diagram.js:792 Uncaught ReferenceError: Raphael is not defined
    at sequence-diagram.js:792
    at sequence-diagram.js:1468
(anonymous) @ sequence-diagram.js:792
(anonymous) @ sequence-diagram.js:1468
(index):93 Uncaught ReferenceError: Diagram is not defined
    at (index):93
(anonymous) @ (index):93
```
`解决办法：`
临时在头部加入
```
<% if(config.sequence) {%>
<script src="https://cdn.bootcdn.net/ajax/libs/lodash.js/4.17.20/lodash.core.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/raphael/2.3.0/raphael.min.js"></script>
<% } %>
```
