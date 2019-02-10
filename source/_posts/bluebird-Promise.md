---
title: bluebird && Promise
date: 2019-02-08 09:50:15
categories:
- 技术
tags:
- Javascript
---

本想看看 markdown 怎么通过 hexo 生成 html。结果被第一代码吸引了，发现呐，经常用的 Promise 竟然引入 bluebird，好吧。承认怎么跑偏了。不管了，捡到篮子的都是菜。

```javascript
const Promise = require('bluebird');
```

### javascript - Promise

```javascript
{ [Function: Promise]
  [length]: 1,
  [name]: 'Promise',
  [prototype]:
   Promise {
     [constructor]: [Circular],
     [then]: { [Function: then] [length]: 2, [name]: 'then' },
     [catch]: { [Function: catch] [length]: 1, [name]: 'catch' },
     [chain]: { [Function: chain] [length]: 2, [name]: 'chain' },

     [Symbol(Symbol.toStringTag)]: 'Promise' },
  [reject]: { [Function: reject] [length]: 1, [name]: 'reject' },
  [all]: { [Function: all] [length]: 1, [name]: 'all' },
  [race]: { [Function: race] [length]: 1, [name]: 'race' },
  [resolve]: { [Function: resolve] [length]: 1, [name]: 'resolve' },
  [defer]: { [Function: defer] [length]: 0, [name]: 'defer' },
  [accept]: { [Function: accept] [length]: 1, [name]: 'accept' },
  [Symbol(Symbol.species)]: [Getter] }
  ```
