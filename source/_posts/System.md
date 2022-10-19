---
title: System
date: 2022-10-19 15:06:58
tags:
- macos
---

## ** cannot be opened because the developer cannot be verified
> macOS 上的可执行程序都需要经过苹果授予的证书签名后才能正常执行，**自行编译或下载的可执行文件没有签名**
### 解决
1.	系统偏好（System Preferences）-> 安全与隐私（Security & Privacy）-> 打开右下脚锁
2.	运行被拦截的软件，出现 cannot be opened because the developer cannot be verified, 点击 cancel
3.	点击 系统偏好 里 Allow Anyway
4.	再次运行需要安装的软件

