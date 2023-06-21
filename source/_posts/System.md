---
title: System
date: 2022-10-19 15:06:58
tags:
- macos
---

## mysql.dmg 安装 ** cannot be opened because the developer cannot be verified
> macOS 上的可执行程序都需要经过苹果授予的证书签名后才能正常执行，**自行编译或下载的可执行文件没有签名**
### 解决
1.	系统偏好（System Preferences）-> 安全与隐私（Security & Privacy）-> 打开右下脚锁
2.	运行被拦截的软件，出现 cannot be opened because the developer cannot be verified, 点击 cancel
3.	点击 系统偏好 里 Allow Anyway
4.	再次运行需要安装的软件

## brew install mysql
```bash
$ brew install mysql // 安装 mysql
$ brew services start mysql // 启动 mysql 服务
$ mysql -u root // 连接 mysql 服务器
```

## ERROR 2002(HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock'
> It's probably because MySQL is installed but not yet running mysql 安装后没有启动服务

### 解决
```bash
$ brew services start mysql
```
