---
title: JavaScript-Docment Object Model
tags:
- JavaScript
---

## Browser Object Model
```
window
├── navigator
├── location
├── frames
├── screen
├── document
│   ├── forms
│   ├── links
│   ├── anchors
│   ├── images
│   ├── all
│   ├── cookie
└── history
```

## document
> window.document 是 Document 的实例。代表整个 HTML 文件。<u>该对象上提供一组 HTMLCollection 实例</u>

|name|description|
|----|----|
|document.forms|获取所有窗体|
|document.images|获取所有的图片元素|
|document.links|a 标签定义，超链接用 href 属性|
|document.anchors|a 标签定义，锚点使用 name 属性|
|document.cookie|设定或读取 cookie，使用不便，容量只有 4kb|

### 具体如下 DEMO
```html
<!Doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
  </head>
  <body>
    <form action="" name="login">
      姓名：<input type="text" name="user" value="admin" />
      密码：<input type="password" name="password" value="123456" />
      <button type="submit">登录</button>
    </form>
  </body>
</html>
```
#### 获取 forms
1.  用[]加索引获取 `document.forms[0]`
2.  用[]加 id 或 name 取 `document.forms['login]`
3.  点运算符加名称取 `document.forms.login`
> 以上三种方式适用于 document.forms/document.links/document.anchors/document.images
**继续用`elements`获取子元素**
1.  用[]加索引获取 `document.forms[0].elements[0]`
2.  用[]加 id 或 name 取 `document.forms['login].elements['user']`
3.  点运算符加名称取 `document.forms.login.elements.user`
> 在父子元素中都有 name 属性时，可以直接采用 `document.login.user.value`

## 文件对象模型（Document Object Model, DOM)
浏览器厂商实现的DOM——目的是解决浏览器间的对象模型不一致的问题

卷标以及文字都有相应的对象，这些就形成了树状结构
```
document                                    (Document)
├── html                                    (HTMLHtmlElement)
│     ├── head                              (HTMLHeadElement)
│     │     ├── meta
│     ├── body
│     │     ├── form
│     │     │     ├── input[name=user]
│     │     │     ├── input[name=password]
│     │     │     ├── button                 
│     │     │     │     ├── 登录              (Text)
```
> document 是 Document 的实例，代表整份文件，使用 document.childNodes[0] 或者 document.documentElement 取 html 卷标的 DOM 元素。

DOM API 分两部分
核心 API 可以用任何语言实现操作接口，可操作的对象基于 XML 文件
HTML DOM API 是核心 DOM API 的延伸，专门用于操作 HTML

## 使用 JavaScript 取 HTML 中的信息
### 文件结构
核心 DOM API 取出 HTML 中某个节点，再取其以下节点
*   parentNode: 取得父节点
*   previousSibling: 前邻节点
*   nextSibling: 后邻节点
*   firstChild: 首个子节点
*   lastChild: 最后一个子节点
*   childNodes: 所有子节点
`doucment.body.parentNode` 得到 text
    
可以通过以下特性获取元素
*   parentElement 取父元素
*   previousElementSibling 取前邻接元素
*   nextElementSibling 取后邻接元素
*   firstElementChild 第一个子元素
*   lastElementChild 最后一个子元素
*   children 所有直接子元素
### id 属性、卷标名称、class 属性
取 Element 类型的节点
|name|description|return|
|----|-----------|------|
|getElementsByTagName|顺序是子树中的顺序，使用索引取对应节点|HTMLCollection|
|getElementsById|卷标对应 id 属性，独一无二，重复时取子树中第一个符合的元素|/|
|getElementsByName|HTML 卷标定义 name 属性，name 属性值可以重复|HTMLCollection|
|getElementsByClassName|HTML 卷标上定义有 class 属性|HTMLCollection|
> innerHTML 取卷标内的 HTML，**HTML5 正式将 innerHTML 纳入标准**

### 选择器语法
通过 `querySelector()`、`querySelectorAll()` + CSS 选择器取元素

|id|选择器|
|---|---|
|document.getElementById('test')|document.querySelector('#test')|
|document.getElementsByTagName('div')|document.querySelectorAll('div')|

## 卷标属性与DOM特性
**这里稍做个理解上的区分，方便理解**
> 通过 JavaScript 获取 DOM，继续取 DOM 具有的特性（Property）
> HTML 卷标上设置的为属性（attribute）
<font color="#f33">通常情况下，JavaScript 特性与 HTML 卷标上的属性是对应的，<u>也有因保留或关键字原因造成的不一致</u>如下表</font>

|html 属性|DOM特性|
|----|----|
|&lt;input name="user" value="admin"&gt;|let input=document.querySelector('input')[0];<br /> let {name, value} = input|
|&lt;label class="label" for="radio"&gt;demo&lt;/&gt;<br/> **class 为保留字**<br/>**for 为关键字**|let label = document.getElementsByTagName('label')[0]; <br/> let className=label.className;<br/> let labelFor = label.htmlFor|

<font color="#f33">特别说明：文本也是一个 DOM 节点</font>
用 `document.body.lastChild.data` body 中最后一个子节点的内容

<font color="#f33">innerHTML 设定 HTML 片段给 innerHTML，标签会被解析、建立对应的 DOM 对象，**script 卷标被忽略，避免 XSS 的问题**</font>

> 如果卷标间只有文字，想要获取文件，使用 `textContent`

### attributes 特性
卷标设置的属性，在 DOM 对象的 attributes 特性中记录。attributes 类型为 NamedNodeMap，为类数组。
> getAttribute() 取 attribute 记录的属性值，取 attributes 中不存在指定属性时，返回 null,指的是 DOM 上没有对应的特性，该值为默认值。
> setAttribute() 设定属性
> removeAttribute() 来移除 attributes 属性。操作后**只是回到默认值**，不是直接将特性移除。<u>没有任何操作可以将 DOM 对应于属性的特性移除</u>

```JavaScript
var dom = document.getElementsByName('input')
dom.getAttribute('readonly')

var imgDom = document.getElementById('img')
img.removeAttribute('src') // src = ''

dom.setAttribute('value', 'helenZhang') // 修改 input 的 defaultValue
```

<font color="#f33">基于安全考虑，input type 为 file 时，defaultValue、value 属性的设置会被忽略，无法通过程序代码取得 DOM 的 defaultValue、value 
特性，使用程序代码设置 DOM 的 defaultValue、value 或通过 setAttribute() 设值会影响 DOM 相对应的特性，但对浏览器窗体或文件上传行为没有影响，只能由使用者亲自选取文件。</font>

## 修改 DOM 树
浏览器解析 HTML，生成 DOM 树。根据 DOM 树绘制浏览器中的画面，改变 DOM 树，浏览器重绘画面。如此构成修改文件的基本原理。
### 修改 DOM 的几个 API
|API name|API description|
|----|----|
|createElement|建立卷标对应的元素|
|createTextNode|建立文本节点|
|appendChild|添加子节点|
|removeChild|删除子节点|

<font color="#f99">**操作多DOM的几种方式**</font>
1.  **createDocumentFragment，建立 DocumentFragment 实例，利用它作为容器在背景建立 DOM 结构，最后将 DocumentFragment 实例通过 appendChild() 附加至 DOM 树**
2.  组织 html 片段字符串，最后再设定给 innerHTML 
```javascript
let frag = ''
for (let i=0; i<10; i++) {
    frag += '<img src={imgs[i]} />'
}
document.body.innerHTML = frag
```
