---
title: macos 常用命令
date: 2020-10-22 11:59:39
tags:
- grocery
- macos
---

空难发生于最近，电脑硬盘坏了，换了新硬盘。新电脑，一切重新开始。
## homebrew 安装
### 安装
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
### <font color="#f33">curl: (7) Failed to connect to raw.githubusercontent.com port 443: 拒绝连接 问题</font>
> DNS 污染造成的
1.  使用 `https://www.ipaddress.com/` 查询 `raw.githubbusercontent.com` 的正确 IP
![img.png](/images/macos/ipaddress.png)    
2.  
```shell
$ vim /etc/hosts
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
```
> 一个打脸的事实，安装好之后不要乱骚情，测试删除。使用的方法版本不对，结果又整出下面的妖

### <font color="#f33">fatal: unable to access 'https://github.com/Homebrew/brew/': LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443</font>
步骤如上，`ipaddress` 中查找 `github` ip。hosts 中配置代理。我的配置如下
```shell
140.82.113.4 github.com
```

## brew 安装 node
```shell
brew install node
```

## macos 解压文件 .rar
commands 参数
```shell
`e`             Extract files without archived paths # 解压缩文件到当前目录
  `l[t[a],b]`     List archive contents [technical[all], bare]  # 列出压缩文件
  `p`             Print file to stdout #打印文件标准输出设备
  `t`             Test archive files # 测试压缩文件
  `v[t[a],b]`     Verbosely list archive contents [technical[all],bare] # 详细列出压缩文件信息
  `x`             Extract files with full path # 用绝对路径解压缩文件

  ```bash
$ unrar x x.rar
```

## macos 文件重命名 [更多相关](https://www.cnblogs.com/liujiacai/p/8313548.html)

```bash
$ mv 切图图片资源/ static
```

## macos 复制文件
```bash
$ cp webpack.config.js webpack.dev.config.js
```

## macos 安装 tree 命令
### 方法一
* `cd $home`
* `vim .bashrc`
* `alias tree = "find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"`
* `source .bashrc`

### 方法二
也可以使用 homebrew 安装 tree 命令行：
`1 $ brew install tree`

