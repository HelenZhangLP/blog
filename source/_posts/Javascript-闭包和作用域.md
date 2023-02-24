---
title: JavaScript-闭包和作用域
date: 2023-02-04 23:09:24
tags:
- JavaScript
- Function
---

## 闭包
闭包是纯函数式编程语言的主要特征之一。
闭包允许函数访问并操作函数外的变量，只要闭包或变量存在于声明函数时所在的作用域内，闭包即可使用函数使用这些变量或函数。
### 闭包 demo
```javascript
var outerValue = 'outerValue'
var later

function outerFn() {
	function innerFn() {
		var innerValue = 'innerValue'
		console.log(outerValue, innerValue)
	}
	
	later = innerFn
}

outerFn() 
later() // outerValue innerValue
```
闭包创建了被定义时的作用域内的变量或函数的安全汽泡，函数获得了执行时的引用。因此，即使 outerFn 函数执行结束被销毁，但引用存在。
**每一个通过闭包访问变量的函数都有作用域链，作用域包含闭包的全部信息**
<font color='#a33'>使用闭包，所有信息都会存在内存中。直到 Javascript 引擎确保这些信息不再使用，即可以进行安全的垃圾回收或卸载页面才会清理。</font>
{%plantuml%}
rectangle outer as " " {
	label outerValue
	label later
	rectangle outerFn {
		rectangle innerFn {
			label innerValue
			label printOuterValue as "console.log(outerValue)"
			label printInnerValue as "console.log(innerValue)"
			
			printInnerValue --> innerValue
		}
		
		printOuterValue --> outerValue
		later --> innerFn
	}
}
{%endplantuml%}

### 闭包 —— 封装私有变量
```javascript
// 创建构造函数
function Counter() {
	// 私有变量
	var num = 0;
	this.getNum = function() {
		return num;
	}
	
	this.setNum = function () {
		num++
	}
}

var counter = new Counter()
console.log(counter.num); // undefined 通过闭包封装的私有变量外部无法访问
console.log(counter.getNum()) // 0
```
#### JavaScript 中没有真正的私有变量属性，只是通过**闭包**实现一种可接受的“私有变量”方案
```JavaScript
// 创建构造函数
function User() {
	// 私有变量
	var name = 'helen';
	this.getName = function() {
		return name;
	}
	
	this.setName = function () {
		name += ' is called'
	}
}

var userA = new User()
userA.setName()
var userB = {}
userB.getName = userA.getName

console.log(userB.getName()) // helen is called
```
#### 总结
> JavaScript 允许将一个对象的属性复制给另外一个对象，导致如 demo，name 属性并非真正私有变量，***但可成为一种隐藏信息的有用方式***

### 闭包 —— 回调函数
```html
<div id="box1"></div>
```
```javascript
function animateIt(elementId) {	
	var element = document.getElementById(elementId)
	var tick = 0
	// 匿名函数作为计时器的参数
	var timer = setInterval(function(){
		tick++;
		if (tick < 100) {
			element.style.left = element.style.top = tick + 'px'
		} else {
			clearInterval(timer)
		}
	}, 10)
}
animateIt('box1')
```
闭包内部的函数不仅可以在闭包创建的时刻访问这些变量，而且当闭包内部的函数执行时，还可以更新这些变量的值。闭包不是在创建的那一时刻的状态的快照，而是一个真实的状态封装，只要闭包存在，就可以对变量进行修改。

#### 结合词法环境，理解闭包
{%plantuml%}
<style>
card {
	backgroundColor yellowgreen
	fontColor #fff
	fontSize 12
	lineColor transparent
}
class {
	backgroundColor lightgreen
	lineStyle dotted
}
</style>
allowmixing
stack 执行栈 {
	card globalContext <<Global>> as "全局执行上下文" {
	}
	card animateIt1Context <<Function>> as "函数执行上下文"{
	}
	
	animateIt1Context --> globalContext
}

class GlobalEnviroment as "全局词法环境" << (E, #337700) Enviroment >> {
	animateIt: function
}
class animateIt1Enviroment as "函数词法环境"  << (E, #337700) Enviroment >> {
	element: <div id="box1">...</div>
	tick: 0
	timer
}

storage animateItFn as "function(){}" {
}

globalContext --> GlobalEnviroment
GlobalEnviroment::animateIt --> animateItFn
animateIt1Context --> animateIt1Enviroment
animateItFn --> animateIt1Enviroment::[[enviroment]]
{%endplantuml%}

## 作用域
在程序的特定部分中，标识符的可见性。作用域是程序的一部分，特定名字绑定特定的变量。
### 通过执行上下文来跟踪代码 —— Javascript 引擎如何跟踪函数的执行并回到函数的位置
Javascript 中，代码执行的基础单元是函数。
Javascript 中每条语句的执行，都处于特定的执行上下文。Javasacript 分为两种代码，全局代码和函数。同样，执行上下文也分为函数执行上下文和全局执行上下文。全局执行上下文只有一个，当 Javascript 代码执行时即创建。函数执行上下文可有多个，每调用一个函数，创建一个新的函数执行上下文。
### 函数执行上下文
```javascript
function user() {
	getOrderList()
}

function getOrderList() {
	assets('assets', true, `this is user's order list`)
}

user()
```
> <font color="#a33">Javascript 是单线程模型，同一时刻只能执行特定的代码</font>
> Javascript 引擎使用执行上下文跟踪函数的执行。使用执行上下文栈（调用栈）跟踪执行上下文。具体如下：
* Javascript 程序开始，全局执行上下文已创建，开始执行**全局执行上下文**

{%plantuml%}
	stack ExecutionContext [
		全局执行上下文
	]
{%endplantuml%}

* `user()` 函数调用，`user`执行上下文入栈，全局执行上下文暂停

{%plantuml%}
	stack ExecutionContext [
		<b>user 函数执行上下文
		---
		全局执行上下文(suspend)
	]
{%endplantuml%}

* `getOrderList()` 函数调用后， `user()`函数执行上下文暂停， `getOrderList()` 函数执行上下文入栈

{%plantuml%}
stack ExecutionContext [
	<b>getOrderList 函数执行上下文
	---
	user 函数执行上下文（suspend）
	---
	全局执行上下文(suspend)
]
{%endplantuml%}

* `getOrderList()` 函数执行完成后，`getOrderList` 函数上下文出栈，`user` 函数恢复执行

{%plantuml%}
	stack ExecutionContext [
		<b>user 函数执行上下文
		---
		全局执行上下文(suspend)
	]
{%endplantuml%}

* `user` 函数执行完后，`user` 函数上下文出栈，全局执行上下文恢复执行

{%plantuml%}
	stack ExecutionContext [
		全局执行上下文
	]
{%endplantuml%}

### 使用词法环境跟踪变量的作用域
词法环境（lexical enviroment）是 JavaScript 引擎内部用来跟踪标识符与特定变量之间的映射关系。
词法环境，即作用域（scopes）是 JavaScript 作用域的内部实现机制。
函数、代码片断、try-catch 语句可以具有独立的标识映射表。
#### 代码嵌套与词法环境
```javascript
var userId = 0
function getUserInfo() {
	var url='/getUserInfo/'
	function async() {
		var host='localhost:8080'
		request() {
			var method = 'get'
			console.log(userId)
		}
	}
}
user()
```
{%plantuml%}
<style>
card {
  BackGroundColor YellowGreen
  fontColor white
}
</style>
 stack 执行栈 {
	 card fn2 <<request>> as "函数执行上下文"
	 card fn1 <<async>> as "函数执行上下文"
	 card fn <<getUserInfo>> as "函数执行上下文"
	 card globalContext <<Global>> as "全局执行上下文"
	 
	 fn2 .[hidden].> fn1
	 fn1 -[hidden]-> fn
	 fn -[hidden]-> globalContext
 }
 
 storage getUIFn as "function getUserInfo(){}"
 note top: JavaScript 引擎调用函数内置<b>[[Enviroment]]</b>属性，与创建时的环境相关联
 storage asyncFn as "function async(){}"
 note top: JavaScript 引擎调用函数内置<b>[[Enviroment]]</b>属性，与创建时的环境相关联
 storage requestFn as "function request(){}"
 note left: JavaScript 引擎调用函数内置<b>[[Enviroment]]</b>属性，与创建时的环境相关联
 
 map globalEnviroment #aliceblue;line:blue {
 	 userId=>0
 	 getUserInffo *--> getUIFn
 }
 
 map getUserInfoEnviroment #pink;line.dotted {
	 url=>/getUserInfo/
 	 async *--> asyncFn
 }
 
 map asyncEnviroment #lightGreen {
	 host=>localhost:8080
	 request *---> requestFn
 }
 
 map requestEnviroment #lightBlue {
	 method=>get
 }
 
 globalContext --> globalEnviroment
 fn --> getUserInfoEnviroment
 fn1 --> asyncEnviroment
 fn2 --> requestEnviroment
 
 requestFn --> requestEnviroment:[[[[Enviroment]]]]
 asyncFn --> asyncEnviroment:[[[[Enviroment]]]]
 getUIFn --> getUserInfoEnviroment:[[[[Enviroment]]]]

 requestEnviroment ..> asyncEnviroment:外层
 asyncEnviroment ..> getUserInfoEnviroment:外层
 getUserInfoEnviroment ..> globalEnviroment:外层
 
{%endplantuml%}
词法环境主要基于代码嵌套，通过代码嵌套可以实现代码结构包含另一代码结构。
无论何时创建函数都会创建一个与之关联的词法环境，并存在函数的[[Eviroment]]内部属性上。也会创建内部环境环境的引用。
JavaScript 引擎调用函数内置的[[Eviroment]]属性与创建时的环境相关联。

### 理解 JavaScript 变量
> [JavaScript-变量](/2018/05/29/JavaScript-变量/)

### 词法环境中注册标识符 —— 在函数声明之前调用函数
```JavaScript
console.log(userId, getUserInfo, arrowFn, statementFn) // undefined ƒ getUserInfo() {} undefined undefined
var userId = 1
getUserInfo()
// arrowFn() // Uncaught TypeError: arrowFn is not a function
// statementFn() // Uncaught TypeError: statementFn is not a function
function getUserInfo() {}
var arrowFn = () => {}
var statementFn = function() {}
```

#### <font color="color:#a33">Uncaught TypeError: statementFn is not a function</font>
JavaSript 代码的执行分两个阶段进行
> 第一阶段 —— 注册当前词法环境中声明的变量和函数并赋初值
{%plantuml%}
class Global{
	- userId: undefined
	- arrowFn: undefined
	- statementFn: undefined
	---
	+function getUserInfo
}
{%endplantuml%}
> 第二阶段 —— 赋值，执行
{%plantuml%}
class Global{
	- userId: 1
	- arrowFn: function(){}
	- statementFn: function(){}
	---
	+function getUserInfo
}
{%endplantuml%}

#### 总结
{%plantuml%}
(*) --> if "函数环境？" then
  -right->[是] "创建参数对象和函数参数" as a1
else
  -->[否]if "函数环境或全局环境？" as a2 then
    ->[是] "注册其它函数外面的函数声明" as a3
  else
    -->[否]if "当前环境是作用域块？" then
      -left->[是] "注册当前作用域块中的 let/const 变量"
    else
      -right->[否] "注册函数作用域外面用 var 声明的变量\n\r注册块级作用域外面用 let/const 声明的变量"
    endif
  endif
endif
{%endplantuml%}

### 词法环境注册标识符 —— 函数重载
```JavaScript
console.log('fn', fn) // fn ƒ fn() {}
var fn = 3
function fn() {}
console.log('fn', fn) // fn 3
```
> 第一阶段，JavaScript 引擎进行代码扫描，合建词法环境，注册变量、声明函数
{%plantuml%}
storage function as "function() {}"
map Global #lightGreen {
	 fn *--> function
}
{%endplantuml%}
> 第二阶段，赋值并执行
{%plantuml%}
map Global #lightGreen {
	 fn => 3
}
{%endplantuml%}

## 通过词法环境理解闭包
```JavaScript
function Counter() {
	var num = 0
	
	this.setNum = function() {
		num++;
	}
	
	this.getNum = function() {
		return num
	}
}
```
{%plantuml%}
<style>
class {
	backgroundColor lightGreen
	lineStyle dotted
}
</style>
allowmixing
class Counter << (E,#337700) Enviroment >> {
	num: 0
}
note left: <b>2.</b> Counter 函数被调用\r\n创建新词法环境 CounterEnviroment\n\r它会跟踪该作用域中所有局部变量

class couter << (I,#aa3333) Instance >> #lightblue;line.dotted {
	setNum
	---
	getNum
}
note left: <b>1.</b> new 操作符调用 Counter 构造函数\n\r返回新实例对象counter，this 指向 counter

storage setNum as "function setNum() {}" {
}
storage getNum as "function getNumb() {}" {
}

couter::setNum --> setNum
couter::getNum --> getNum

setNum --> Counter::num:[[[[Enviroment]]]]
getNum --> Counter::num:[[[[Enviroment]]]]

{%endplantuml%}
---
> 词法环境用于保持跟踪函数中定义的变量
{%plantuml%}
<style>
card {
  BackGroundColor YellowGreen
  fontColor white
  lineColor transparent
  fontSize 12
}
</style>
 allowmixing
 class GlobalEnviroment as "全局环境"  << (E,#337700) Enviroment >>  #lightGreen;line.dotted {
	 Counter: function
	 counter: object
 }
 class CounterEnviroment as "Counter 函数执行环境"  << (E,#337700) Enviroment >>  #lightGreen;line.dotted {
	 num: 0
 }
 class getNumEnviroment as "getNum 函数执行环境"  << (E,#337700) Enviroment >>  #lightGreen;line.dotted {
 	 
 }
 class counterInstance as "counter 实例" << (O,#aa3333) Instance >> #lightblue;line.dotted {
	setNum
	....
	getNum
 }
 stack stack as "执行栈" #white {
	 card GlobalContext <<Global>> as "全局执行上下文" {
	 }
	 card getNumContext <<getNum>> as "函数执行上下文" {
	 }
	 
	 getNumContext -[hidden]-> GlobalContext
 }
 storage CounterFn as "function(){}" {
 }
 storage setNum as "function setNum() {}" {
 }
 storage getNum as "function getNumb() {}" {
 }
 
 GlobalContext --> GlobalEnviroment
 getNumContext --> getNumEnviroment
 counterInstance::setNum --> setNum
 counterInstance::getNum --> getNum
 GlobalEnviroment::counter --> counterInstance
 GlobalEnviroment::Counter --> CounterFn
 CounterFn-->GlobalEnviroment:[[[[Enviroment]]]]
 setNum --> CounterEnviroment::num:[[[[Enviroment]]]]
 getNum --> CounterEnviroment::num:[[[[Enviroment]]]]
 getNumEnviroment --> CounterEnviroment:outer
{%endplantuml%}

