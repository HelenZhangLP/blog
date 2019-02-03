---
title: hexo 坑
date: 2019-01-31 16:44:18
tags: 博客 hexo
---


#### ERROR - No layout: index.html
``` bash
$ hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
WARN  No layout: index.html
```

> *## 解决 ##*
``` bash
$ git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean
```
> Modify theme setting in _config.yml to ocean
`theme: ocean`

#### ERROR - 发布到 git 服务器访问出现空白页或无主题样式
> 原因是发布到 github 上的项目名称要与个人github的用户名一致，且加后缀 .github.io，如 'helenzhanglp.github.io'
