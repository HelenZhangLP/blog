---
title: HtmlElement
tags:
- html
---

## HTMLElement.offsetParent
只读属性, 返回一个指向最近的（指包含层级上的最近）包含该元素的<font color='#f33'>定位元素（position:fixed,relative,absolute,sticky）</font>
```html
<body>
	<div id='outer'>
		<div id='inner'></div>
	</div>
</body>
```for
---
```css
 #outer {
	 position: relative; // fixed,absolute,sticky(粘性的)
 }
```
---
```javascript
 var inner = document.getElementById('inner')
 inner.offsetParent // <div id='outer'>...</div>
```
---
或返回最近的 `table`，`td`,`th`,`body` 元素。
---
```css
 #outer {}
```
---
```javascript
 var inner = document.getElementById('inner')
 inner.offsetParent // <body>...</body>
```
---
**当前元素的 `style.display` 设置为 'none' 时， offsetParent 返回 'null'**
**当前元素的 `position: fixed` 时，offsetParent 返回 'null'**
---
```css
	#div {
		display: none;
		/* position: fixed; */
	}
```
---
```javascript
 var div = document.getElementById('div')
 div.offsetParent // null
```

