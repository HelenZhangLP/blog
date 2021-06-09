---
title: macos 常用命令
date: 2020-10-22 11:59:39
tags:
- grocery
---

### macos 解压文件 .rar
commands 参数
> `e`             Extract files without archived paths # 解压缩文件到当前目录
  `l[t[a],b]`     List archive contents [technical[all], bare]  # 列出压缩文件
  `p`             Print file to stdout #打印文件标准输出设备
  `t`             Test archive files # 测试压缩文件
  `v[t[a],b]`     Verbosely list archive contents [technical[all],bare] # 详细列出压缩文件信息
  `x`             Extract files with full path # 用绝对路径解压缩文件

  ```bash
  $ unrar x x.rar
  ```
  <!--more-->

  ### macos 文件重命名 [更多相关](https://www.cnblogs.com/liujiacai/p/8313548.html)

  ```bash
  $ mv 切图图片资源/ static
  ```

  ### macos 复制文件
  ```bash
  $ cp webpack.config.js webpack.dev.config.js
  ```
