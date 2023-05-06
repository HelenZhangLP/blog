---
title: Python-MacOS下 Python 配置环境变量
date: 2023-03-08 16:11:55
tags:
-Python
---

拢共分三步：
第一步，查看 Python 安装位置，`which python3`；
```bashrc
$ which python3
/Library/Frameworks/Python.framework/Versions/3.11/bin/python3
```
第二步，认识冰箱门打开，`open -e ~/.bash_profile`；
第三步，把 `alias python="/Library/Frameworks/Python.framework/Versions/3.11/bin/python3"` 放进去；
第四步，把冰箱门关上，让冰箱运行，`source ~/.bash_profile`
最后验证一下哦，看到如下执行结果，就说明配置成功了哦
```bashrc
$ python
Python 3.11.2 (v3.11.2:878ead1ac1, Feb  7 2023, 10:02:41) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 2+3
5
>>> ^D
```