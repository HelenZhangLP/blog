---
title: 'Javascript-正则'
date: 2020-01-13 15:35:23
category: 技术
tags:
- Javascript
- RegExp
---

## RegExp
### RegExp.prototype.exec() 在一个指定字符串中执行一个搜索匹配。返回匹配数组或 null。
*** notice ***
- [ ] 设置了 global 或 sticky 标志位后 RegExp 对象是`有状态`的；
- [ ] 会将上次成功匹配的位置记录在 lastIndex 属性中。
- [ ] 如此 exec() 可用来对单个字符串中的多次匹配结果进行逐条遍历。
> regexObj.exec(str)
<!--more-->

```javascript
const reg = /(\w+)/g
const str = 'abc abc abc'
reg.exec(str)
/**
[ 'abc', 'abc', index: 0, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
[ 'abc', 'abc', index: 4, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
[ 'abc', 'abc', index: 8, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
null
**/
```
加全局匹配标识符，会记录上次匹配的位置，可以循环遍历。`注意不要在遍历条件中写入正则表达式，如果一直匹配，会造成死循环`。
```javascript
const reg = /(\w+)(\s)/g
const str = 'abc abc abc'
reg.exec(str)

/**
  [ 'abc ', 'abc', ' ', index: 0, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
[ 'abc ', 'abc', ' ', index: 4, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
null
 */
```
#### 返回值 []
* [0] 匹配的字符串（['abc ']）
* [1],...[n] 分组捕获（['abc', ' ']）
* index 匹配到的字符位于原始字符串的基于0的索引值 （index: 0）
* input 原始字符串
---


- [ ] `\w` 匹配任意来自基本拉丁字母表中的字母数字字符，还包括下划线。等价于 [A-Za-z0-9_]
- [ ] `\w+` 匹配数字、字母、下划线 x 1 或多次。等价于 {1,}
- [ ] `(\w+)` 匹配数字、字母、下划线至少一次，并且捕获匹配项
- [ ] `\s` 匹配空白符，包括空格、制表符、换页符、其它 Unicode 空格

