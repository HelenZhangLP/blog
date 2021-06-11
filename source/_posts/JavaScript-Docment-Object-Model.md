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

[comment]: <> (### attributes 特性)
