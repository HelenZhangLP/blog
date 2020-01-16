---
title: Javascript-字符串
date: 2020-01-09 10:14:03
categories: 技术
tags: 
- Javascript
- String
---

## Javascript 字符串方法

- [ ] `需求删除最后一个为 & 的字符`
### replace() 返回一个部分或全部匹配由替代模式所取代的新的字符串。
> str.replace(regexp|substr, newSubStr|function)

```javascript
let str = 'zhangliping&'
// regexp = /&$/
str.replace(/&$/, "") // zhangliping
/*
 * --- 引申 ---
 * 删除字符串全部的 &
 */
str.replace(/&$/g, "") // 全局替换
str.replace(/^&/,"") // 替换开头
str.replace(/[?&]$/, "") //替换结尾的 ? 或 &
```
<!-- more -->
### slice() 方法取字符串的一部分，返回新字符串，原字符串不动
> str.slice(beginIndex[, endIndex])

```javascript
let str = 'zhangliping&'
str.slice(0, str.length-1); // zhangliping
```

### substring() 返回从开始位置到结束位置的一个子集。
> str.substring(indexStart[, indexEnd])
```javascript
let str = 'zhangliping&'
str.substring(0, str.length-1);// zhangliping
```

### match() 检索返回一个匹配正则表达式的结果
> str.match(regexp)
> 参数：一个正则表达式对象。
> 若参数为非正则表达式对象，会隐式地使用 `new RegExp(obj)` 转换为 RegExp
> 若没给参数，参数默认为包含空字符串数组，如 `[""]`
```javascript
const str = 'abc abc'
let reg = /(\w+)/g
str.match(reg)
/** 
  [ 'abc', 'abc' ]
 */
reg = /(\w+)/
str.match(reg)
/** 
 [ 'abc', 'abc', index: 0, input: 'abc abc', groups: undefined ]
*/
```
> 返回值： 
> 使用 g 标志，返回表达式匹配的所有结果，不返回匹配捕获组(`[ 'abc', 'abc' ]`)
> 不使用 g 标志，返回第一个完整匹配及其相关的捕获组（input 搜索字符串；groups 捕获数组或 `undefined`; index 匹配结果开始位置）。