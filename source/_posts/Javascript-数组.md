---
title: 'JavaScript 数组'
date: 2020-11-06 11:22:46
categories:
- 技术
tags:
- JavaScript
---

对于需要收集的 **有序、具有索引的数据** 的需求，可以使用数组

## 数组使用过程中要注意的细节
数组的索引其实是数字为名称的特性，空项目则是连数字特性都不存在。
```javascript
var array1 = [undefined, undefined]
var array2 = []
array2.length = 2
// array2 // [empty x 2]

'0' in array1 //true
'0' in array2 // false
array2[0] // undefined ，属性不存在，为 undefined
```

> <font color="#f33s">建议不要修改数组的 length, 也不要让数组产生空项目，<u> JavaScript 中的数组 API 对数组空项处理方式不同</u></font>

map/filter/forEach 都会跳过空项，回调函数不会执行。但返回结果不一样，filter 不保留空项，map 会保留空项

### 过滤除 undefined ，除 0，false,null,'' 等
undefined 不是保留字，避免 undefined 被拿来当变量名称设定了其他的值，所以使用 typeof 确认值的类型名称
```javascript
function ifUndefined(obj, name) {
  return typeof obj[name] === 'undefined'
}
```


## 数组的静态方法
是以 Array 为命名空间的方法，包括以下几个：
### Array.isArray()
> Array.isArray() 的判断依据是<u>数组内部实现特性[[Class]]的值'Array'</u>

```JavaScript
class SubArray extends Array {}
let subArray = new SubArray()
Array.isArray(subArray) // true
```
用类语法继承，子类实例会被 Array.isArray() 判断为 true
<font color="#f33">类语法的继承能继承标准 API，而且内部实现特性以及特殊行为也会继承</font>

```JavaScript
let obj = {}
obj.prototype = Array.prototype
obj instanceof Array // false
Object.setPrototypeOf(obj, Array.prototype)
obj instanceof Array // true
Array.isArray(obj) // false
```
对象原型被修改的类数组，可以骗过 instanceof 但 Array.isArray() 判断为 false

### Array.of()
```JavaScript
var arr = new Array(10) 
console.log(arr) // (10) [empty × 10]
var arr2 = new Array(1,2,3)
console.log(arr2) //(3)[1, 2, 3]
```
以上 demo 通过 Array 构造函数创建了数据，参数两层函数，一是数组的长度。二是数组的元素。较混乱<br/>
常用字面量形式定义数组，方便，容易理解
> 可使用 Array.of() 建立指定元素数组
```JavaScript
Array.of(10) // 建立一个元素为 10 的数组 [10]
Array.of(1,2,undefined,4) // [1, 2, undefined, 4]
[1, 2, , 4]
Array.of(1,2,,4) // Uncaught SyntaxError: Unexpected token ','
```
<font color="#f33">Uncaught SyntaxError: Unexpected token ','</font> 异常说明 与字面定义数组语法不同[1, 2, , 4]，Array.of() 不支持空项目
```javascript
class SubArray extends Array{}
var subArray = SubArray.of(1,2,3,4,5)
subArray instanceof SubArray
subArray // SubArray(5) [1, 2, 3, 4, 5]
```

### Array.from()
Array.from() 可以接受类数组或可迭代对象，传回的新数组包含类数组或可迭代对象元素
Array.form() 从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例
```javascript
Array.from('helen') // (5) ["h", "e", "l", "e", "n"]
Array.from('helen', item => item.toUpperCase()) // ["H", "E", "L", "E", "N"]
Array.from('helen', (item, idx) => !idx ? item.toUpperCase() : item) // ["H", "e", "l", "e", "n"]

class SubArray extends Array{}
let sub = SubArray.from('helen') // SubArray(5) ["h", "e", "l", "e", "n"]
sub instanceof SubArray // true
```

## 改变数组

|API|description|
|---|-----------|
|sort|默认排序方式是按照 Unicode 码点，若元素不是字符串，会先转换为字符串再排序。<br />若要按指定方式排序，必须传入带有两个参数的回调函数，传回的值决定了排序方式。**结果改变原数组**，如下 demo —— sort 排序|
|reverse|反转数组后传回原数组 **结果改变原数组**, 如下 demo —— reverse 排序|
|fill|用某个值进行填充。三个参数：一是要填充的值，二是 start 填充值的起始索引；三是 end 填充值的结束索引。**可以直接使用 call() 或 apply() 指定 fill() 调用时的 this 对象。如下 demo —— fill 的使用|

```javascript
// sort 排序
let arr = [1,100,22,3,11]
console.log(arr.sort())  // [1, 100, 11, 22, 3]
console.log(arr) // [1, 100, 11, 22, 3]
console.log(arr.sort((n1,n2)=>n1-n2)) // [1, 3, 11, 22, 100]

// reverse 排序
console.log(arr.reverse()) // [100, 22, 11, 3, 1]
console.log(arr) // [100, 22, 11, 3, 1]

// fill 方法的使用
Array.prototype.fill.call({length: 2}, 10) // {0: 10, 1: 10, length: 2}
```

### 实现堆栈
|API|描述|
|---|---|
|push|可接受一个或多个参数，接收的参数追加至数组结尾。返回数组长度。**改变原数组**|
|pop|移除并返回数组的最后一个元素|

```javascript
// push
let arr = [1,100,22,3,11]
arr.push(3,5,9) // 8
console.log(arr) // (8)[1, 100, 22, 3, 11, 3, 5, 9]

// pop 
arr.pop() // 9
console.log(arr) // (7)[1, 100, 22, 3, 11, 3, 5]
```
> push + pop 可视为先进后出的堆栈结构使用

### 实现队列
|API|描述|
|---|---|
|unshift|在数组的前端插入一个或多个元素，返回数组长度|
|shift|移除数组前端的第一个元素|

```javascript
let queue = []
queue.unshift(1) // 1
console.log(queue) // [1]
queue.shift()
console.log(queue) // []
```
> unshift + ship 可视为先进先出的队列

[有关数组 API push/pop/fill 的几个扩展 demo](https://github.com/HelenZhangLP/demo/blob/draft/js/Array/index.html)
<font color="#f33">Array.prototype 定义的方法，都具有通用性，可以对普通的对象进行操作，操作结果会令指定的对象成为类数组，必要时，也可以直接将对象的原型设为 Array.prototype</font>

```javascript
let o = {}
Object.setPrototypeOf(o, Array.prototype)
o.push(1,3,9,2)
o.sort((n1,n2)=>n1-n2)
```

## 函数式风格 API
### 有关函数式程序设计

|API|描述|
|---|---|
|indexOf|查找元素所在的第一个索引，返回 -1 代表没有该元素|
|lastIndexOf|查找元素所在的最后一个索引，返回 -1 代表没有该元素|
|includes|查找数组中是否有指定元素 **ES7**，返回值为 true/false|
|find|查找符合条件的元素，参数可以是一个回调函数。**ES6**|
|findIndex|查找符合条件的元素的索引，参数同样可以是一个回调函数。**ES6**|
|filter|过滤出符合条件的元素，返回的是一个数组|
|slice|数组切割，参数一，切割起始索引；参数二切割终止索引，若无，默认 array.length。不改变原数组，返回新的数组|
|concat|数组拼接，不改变原数组。返回拼接后的数组|
|every|判断数组是否都符合某个条件|
|some|判断数组中是否有符合条件的元素|
|join|使用字符串将数组串接为字符串|
|flat|二维数组摊平成一维数组（ES10）|
|flatMap|推平数组的同时进行数组元素的转换，接受回调函数，执行后返回一维数组|
|reduce|处理结果为单一的值，逐一削减数组，最终拿到单一值|
|reduceRight|从右向左迭代元素|

> 以上三个 api 都是使用 === 来判断是否存在该元素
> NaN !== NaN NaN 不等于任何值

#### <font color="#f99">查找数组中的 NaN</font>
```javascript
function indexOf(arrays, value, idx = 0) {
  if (idx === arrays) return -1
  let ele = arrays[idx]
  if (Number.isNaN(value) && Number.isNaN(ele)) return idx
  if (value === ele) return idx
  
  return indexOf(arrays, value, idx + 1)
}

let arrays = [1,2,3,9,NaN,3]
console.log(indexOf(arrays,NaN)) //4

// 使用 findIndex 回调函数中自己定义判断条件
let idx = arrays.findIndex(ele=>Number.isNaN(ele)) 
console.log(idx) // 4
```
<font color="#f33">
*   支持函数式范式的语言，特性之一是 <b>具备一级函数</b><br />
*   纯函数式编程，**不提供循环语法**，使用递归解决重复性任务。<br />
*   纯函数式编程中，没有变量的概念，也不能改变对象或数据状态。
</font>

```javascript
// 递归1——思考边界条件
// 递归2——一次只做一件事
// 递归3——别管上层递归或下层递归的状态
/**
 * 
 * @param arrays 要遍历的数组
 * @param mapper 要执行的回调函数
 * @returns {*}
 */
function map(arrays, mapper) {
  if(arrays.length === 0) return arrays // arrays 是一个空数组
  let head = arrays[0]
  let mapped = mapper(head)
  
  console.warn(map(arrays.slice(1), mapper))
  return [mapped].concat(map(arrays.slice(1), mapper))
}

console.log(map([1,2,5],ele=>ele*10))
```

## 群集
### Set 与 WeakSet
使用 Set 收集不重复的值，Set 构造函数接收可迭代对象

|API|描述|
|---|---|
|String.prototype.split|将字符串按照指定字符进行切割，返回数组|
|Set.prototype.size|得到 Set 集合中元素的数量|
|Set.prototype.values|返回一个新的迭代对象，该对象包含 Set 对象中的按插入顺序排列的所有元素的值|
|Set.prototype.add|在 Set 集合尾部添加一个元素，返回该 Set 对象|
|Set.prototype.has|返回一个布尔值，表示该值在 Set 中是否存在|
|Set.prototype.delete|移除 Set 中与参数相等的元素|
|Set.prototype.clear|移除 Set 对象中的所有元素|

```javascript
// 收集以下字符串中不重复的值
let strArr = "I'm mommy shark, she is baby shark, he is daddy shark".split(' ')
let words = new Set(strArr)
console.log(words.size)
console.log(words.add('Confirming over the phone'))
console.log(words)
/**
 * 
 * {
 *    0: "I'm"
      1: "mommy"
      2: "shark,"
      3: "she"
      4: "is"
      5: "baby"
      6: "he"
      7: "daddy"
      8: "shark"
      9: "Confirming over the phone"
 * }
 */
console.log(words.has('Confirming over the phone')) // true
words.delete('Confirming over the phone')
words.has('Confirming over the phone') // false
words.clear()
words.size // 0

// 获取 value
console.log(words.values()) // SetIterator {"I'm", "mommy", "shark,", "she", "is",…}
console.log(Array.from(words)) // ["I'm", "mommy", "shark,", "she", "is", "baby", "he", "daddy", "shark"]
```
> set 本身无序，不具备索引。没有直接可取的 get 之类的方法，有关 Set 方法，见上表

<font color="#f33">ECMAScript 对 Set 采用 SameValueZero 演算 相等采用 ===，0 等于 -0, <u>Set 中若有 NaN，试图再加入 NaN，Set 中还是只有一个 NaN</u></font>
```javascript
new Set([NaN,NaN,10,10,1,undefined,undefined]) // Set(4){NaN, 10, 1, undefined}

// 即使相同属性，相同值的两个对象在 set 中也是不同的
let o1 = {x: 1}
let o2 = {x: 1}
let o3 = o2

new Set([o1,o2,o3])
/**
 * Set(2) {{…}, {…}}
   [[Entries]]
   0:
   value: {x: 1}
   1:
   value: {x: 1}
   size: 2
 */
```
<font color="#f99">ES6 中也提供了 WeaKSet，只能加入对象，不能加入除对象以外的，如 null,undefined,NaN和一些基本类型；垃圾回收时不会考虑是否被 WeakSet 
管理，只要对象没有其它名称参考着，就会回收。避免必须使用 Set 管理对象，却忘了从 Set 中清除对象而发生内存泄漏。由于对象没有引用就会被回收，所以WeakSet 没有 size，只有 add(),delete(),has() 
方法</font>

### Map 与 WeakMap
历史：JavaScript 对象是键值对，键只能是字符串，不可以使用字符串以外的类型
现在：ES6 提供的 Map 类型，键可以使用基本类型、undefined、NaN、null 与对象，**Map 对键的唯一性采用的是 SameValueZero 演算**

Map 的实例是可迭代对象，使用 for...of 迭代出的元素会是个包含键与值的数组，使用解构语法可以分解成

|API|描述|
|---|---|
|Map.prototype.size|返回 Map 对象的键/值|
|Map.prototype.has|返回布尔值，表示 Map 实例是否包含键对应的值|
|Map.prototype.clear|清除 Map|
|Map.prototype.keys|返回一个新的 Iterator 对象，插入顺序包含了Map对象中每个元素的键|

```javascript
// 过去
var o = {x:1}
let map = {
    [o]: 'veriable'
}
console.log(map) // {[object Object]: "veriable"}
for (o in map) {
    console.log(typeof o) // string
}

// ES6 提供 Map 类型
let mapES6 = new Map()
mapES6.set(o, 'variable')
console.log(mapES6) // Map(1){"[object Object]" => "variable"}[[Entries]]0: {"[object Object]" => "variable"}size: (...)__proto__: Map
for (let o of mapES6) {
  console.log(typeof o)  // object
}
console.log(mapES6.get(o)) // variable
```
> ES10 新增了 Object.fromEntries() 函数，建构对象时使用，接受可迭代物（如，数组、Map）作自变量，该对象迭代出来的每个元素都必须是[键，值]形式

```javascript
Object.fromEntries([['k1','v1'],['k2','v2']]) // {k1: "v1", k2: "v2"}
Object.fromEntries(new Map([['k1','v1'],['k2','v2']])) // {k1: "v1", k2: "v2"}
```

<font color="#f99">WeakMap 只能使用除 null 以外的对象作为，垃圾回收时不会考虑对象是否被 WeakMap，只要对象没有被引用就会被回收。WeakMap 
不存在对象作为键，值也会被清除。可以用来避免从Map中清除对象而发生内存泄漏</font>

[comment]: <> (### ArrayBuffer)

## 数组重组方法
### 1.  数组转换成字符串
* Array.prototype.toString() 将数组转换成 **以逗号分隔的字符串**
```javascript
var arr = ["cookie", "local storage", "session storage", "web storage"];
arr.toString();
// "cookie,local storage,session storage,web storage"
```
* Array.prototype.join([separator]) 将数组转换成 **指定分隔符的字符串**
```javascript
arr.join('/')
// "cookie/local storage/session storage/web storage"
```
  - `注意：` 如果数组只有一个元素，则该连接字符不显示
  ```javascript
  var arr1 = ['category']
  arr1.join('/')
  // "category"
  ```
### 2.  创建新数组
* Array.prototype.concat() 合并两个以上数组，最终拿到生成后的新数组，生成的新数组是需要合并的数组的浅拷贝。
  - `注意：`如果将引用类型复制到数组中,引用类型改变，新的数组数组发生改变，反之一样。因为他们指向同一个引用对象。
  ```javascript
  var arr_reference_type = [['concat'],'array prototype function'];
  var arr_concat = arr.concat(arr_reference_type);
  console.log(arr_concat);
  // ["cookie", "local storage", "session storage", "web storage", ["concat"], "array prototype function"]
  // 修改 arr_reference_type
  arr_reference_type[0].push('slice');
  console.log(arr_concat);
  // ["cookie", "local storage", "session storage", "web storage", ["concat", "slice"], "array prototype function"]
```
