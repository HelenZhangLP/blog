---
title: git 坑
date: 2019-06-03 16:21:53
categories:
- 工具
tags:
- git
---

### orgin/Head -> origin/master 分支
> orgin/HEAD 像一个指针，表示默认分支。origin/Head -> origin/master 代表 origin/master 是默认分支
> `git remote set-haed origin -d` 可以删除这个分支

### .idea gitignore 忽略问题
> .gitignore 中添加 .idea/ 没用

[原因参照 - 1](https://help.github.com/en/articles/ignoring-files)
[原因参照 - 2](https://git-scm.com/docs/gitignore)
忽略所有目录下的 .idea -> \*\*/.idea/

```bash
$ git rm --cached FILENAME
```

### macos 升级后 git 编译错误
```
$> git
dyld: Symbol not found: _OBJC_IVAR_$_NSFont._fFlags
  Referenced from: /Applications/Xcode.app/Contents/Frameworks/IDEFoundation.framework/Versions/A/../../../../SharedFrameworks/DVTKit.framework/Versions/A/DVTKit
  Expected in: /System/Library/Frameworks/AppKit.framework/Versions/C/AppKit
 in /Applications/Xcode.app/Contents/Frameworks/IDEFoundation.framework/Versions/A/../../../../SharedFrameworks/DVTKit.framework/Versions/A/DVTKit
gcc: error: unable to locate xcodebuild, please make sure the path to the Xcode folder is set correctly!
gcc: error: You can set the path to the Xcode folder using /usr/bin/xcode-select -switch
```
> git: error: unable to locate xcodebuild, please make sure the path to the Xcode folder is set correctly!

**解决办法**
> xcode-select --install
> sudo /usr/bin/xcode-select --switch /Library/Developer/CommandLineTools/
