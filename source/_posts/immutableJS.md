---
title: immutableJS
date: 2019-03-11 11:21:52
categories:
- 技术
tags:
- Javascript
- immutable
---
Facebook 工程师 Lee Byron 花费 3 年时间打造，与 React 同期出现，但没有被默认放到 React 工具集里（React 提供了简化的 Helper）
### Record
Record() 可以创建一个新的 Record 类，类似于Javascript的Object，但是只接收特定字符串为key，具有默认值:
### setIn()
setIn 设置深层结构中的某些属性
> setIn(keyPath: Iterable<any>, value: any): this
