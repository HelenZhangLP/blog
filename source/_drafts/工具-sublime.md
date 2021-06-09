---
title: 工具-sublime
date: 2021-03-01 09:35:29
categories:
- 工具
tags:
- develop-tools
---

sublime 可以配置编译系统
自带代码高亮
没有自动补全
代码格式化
<!--more-->

## sublime Text 插件管理器 packageControl
安装方式：
Control+\`打开控制台 写入 `import urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace('','%20')).read())`

## packageControl 中安装 JavaScript 代码提示插件 SublimeCodeIntel
command+shift+p 打开 packageControl -> install package -> SublimeCodeIntel

## Node.js JavaScript 运行时编译环境
$ which node
Tools->Build System->New Build System
{
  'cmd': [which node, '$file'],
  'selector': 'source.js'
}
