---
title: JavaScript——循环
tags:
- JavaScript
---

## for...in
for...in 不仅可枚举遍历对象的实例属性(公有属性)，也包括可枚举原型属性(私有属性)。Symbols 在 for...in 循环中不可枚举，~不同的浏览器遍历的顺序不同~。
<span class='custom-box custom-box-933'>性能差（既可跌代私有属性，也可跌代公有属性）</span>只能迭代可枚举，且非 Symbol 类型的属性

```javascript
let arr = [1,2]
arr[Symbol('react.Fragment')] = 'react.Fragment'
console.log(arr) // [1, 2, Symbol(react.Fragment): 'react.Fragment']
Array.prototype.test = 'test'
for (const key in arr) {
    console.log(arr[key], typeof arr[key])
}
/**
 * 1
 * 2
 * test
 */
```

## Array.prototype.forEach
如何获取对象的所有私有属性，不论类型与是否可枚举
1.  Object.getOwnPropertyNames() 获取对象的所有私有属性
2.  Object.getOwnPropertySymbols() 返回给定对象自身的所有 Symbol 属性数组

```javascript
let arr = [1,2]
Array.prototype.test = 'test'
arr[Symbol('react.Fragment')] = 'react.Fragment'
console.log(arr) // [1, 2, Symbol(react.Fragment): 'react.Fragment']
console.log(Object.getOwnPropertyNames(arr)) // ['0', '1', 'length']
console.log(Object.getOwnPropertySymbols(arr)) // Symbol(react.Fragment)]
let keys = [...Object.getOwnPropertyNames(arr), ...Object.getOwnPropertySymbols(arr)] //  ['0', '1', 'length', Symbol(react.Fragment)]
keys.forEach(key => {console.log(arr[key])}) // 1, 2, 2, react.Fragment
```


### `tip：` ECMAScript5 标准之后允许开发者通过属性描述器接口 PropertyDescriptor 来修改属性并且可以定义新属性。可设置 value, writable, enumerable, configurable, set, get 等。若 enumerable 设为 false，则不可枚举，for in 时该属性不会显示。

### `tip：` ES6 规范中，for...of 兼容性有些问题 IE 不支持。使用时多加考虑。

### `tip：` forEach 不能使用 break 指令中断循环。
