---
title: Npm Error
date: 2021-03-30 14:04:13
tags:
- npm
---

## npm ERR! code ECONNREFUSED
> npm ERR! FetchError: request to https://registry.npmjs.org/npm failed, reason: connect ECONNREFUSED 127.0.0.1:1034

<font color='red'>**代理引起的**</font>
```
$ npm config get proxy
http://127.0.0.1:1034/
$ npm config set proxy null
```
<!-- more -->
