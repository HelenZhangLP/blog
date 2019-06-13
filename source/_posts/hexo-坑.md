---
title: hexo 坑
date: 2019-01-31 16:44:18
categories:
- hexo
tags:
- hexo
---

#### *&lt;!-- more --&gt;* 断行，在首页展示缩略信息，该方法能被 hexo 更好识别
```
auto_excerpt:
  enable: true
  length: 150
```

#### ERROR - No layout: index.html
```
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
WARN  No layout: index.html
```
<!-- more -->

> *## 解决 ##*
```
$ git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean
```
> Modify theme setting in \_config.yml to ocean
`theme: ocean`

#### ERROR - 发布到 git 服务器访问出现空白页或无主题样式
> 原因是发布到 github 上的项目名称要与个人github的用户名一致，且加后缀 .github.io，如 'helenzhanglp.github.io'

#### ERROR - 博客中添加 gittalk
> *## Error Error: Not Found. ##*
*## 解决 ##*
```
gitalk:
  enable: true
  clientID: 8e7e6dda81936172806e # GitHub Application Client ID
  clientSecret: 97f71b6bbdf731bc650ec39212061882b8f36e71 # Client Secret
  repo: 'blog' # Repository name 仓库不能为私有仓库 Private Repository
  owner: HelenZhangLP  # GitHub ID
  admin: HelenZhangLP # GitHub ID
```
