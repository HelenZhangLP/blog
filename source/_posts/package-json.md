---
title: package.json
date: 2020-04-17 11:20:44
tags:
- webpack
---

###　browserslist [参考文档](https://cli.vuejs.org/zh/guide/browser-compatibility.html#browserslist)
#### browserlist 具体描述 [参考文档](https://github.com/browserslist/browserslist)
```json
"browserslist": [
  "> 1%",
  "last 2 versions"
]
```

```javascript
//package.json
{
  "name": "axios",
  "version": "1.0.0",
  "description": "",
  /* "main": "index.js", 移除，防止意外发布代码 */
  "private": true, // 确保安装包是私有的
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "webpack": "^5.43.0"
  },
  "devDependencies": {
    "webpack-cli": "^4.7.2"
  }
}
```
