---
title: .idea gitignore 忽略问题
date: 2019-06-03 16:21:53
tags:
- git
---
> .gitignore 中添加 .idea/ 没用
[原因参照 - 1](https://help.github.com/en/articles/ignoring-files)
[原因参照 - 2](https://git-scm.com/docs/gitignore)
忽略所有目录下的 .idea -> \*\*/.idea/
```bash
$ git rm --cached FILENAME
```
