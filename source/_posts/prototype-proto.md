---
title: prototype && __proto__
date: 2019-03-19 12:20:05
tags:
- JavaScript
- Object
- 对象继承
---
[继承](https://segmentfault.com/a/1190000015727237)
### Javascript 继承
* 原型链继承
* 构造函数继承
* 组合继承
* 原型式继承

```javaScript
function base(o) {
  function F(){}
  F.prototype = o;
  return new F();
}
var baseObj = {
  name: '基类',
  arr: ['array1','array2','array3'],
  fn: function() {
    console.log(this.name, this.arr)
  }
}
var baseInstance1 = base(baseObj)
var baseInstance2 = base(baseObj)
baseInstance2.name = '修改属性值'
baseInstance1.arr.push('array4')
console.dir(baseInstance1)
console.dir(baseInstance2)
/* === baseInstance2 添加了一个实例属性 === */
```
* 寄生式继承
* 寄生组合继承
* Class extends
