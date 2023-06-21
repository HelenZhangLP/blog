---
title: Html-script-type
tags:
- html
---

## script 解释
> HTML &lt;script&gt; 元素用于嵌入或引用可执行脚本。这通常用作嵌入或者指向 JavaScript 代码。&lt;script&gt; 元素也能在其他语言中使用，比如 WebGL 的 GLSL 着色器语言。

### type
> 该属性定义 script 元素包含或src引用的脚本语言。
> 属性的值为 MIME 类型; 支持的 MIME 类型包括text/javascript, text/ecmascript, application/javascript, 和application/ecmascript。
> 如果没有定义这个属性，脚本会被视作 JavaScript。 
> 如果 MIME 类型不是 JavaScript 类型（上述支持的类型），则该元素所包含的内容会被当作数据块而不会被浏览器执行。 如果 type 属性为module，代码会被当作 JavaScript 模块 实验性。请参见ES6 in Depth: Modules 在 Firefox 中可以通过定义 type=application/javascript;version=1.8 来使用如 let 声明这类的 JS 高版本中的先进特性。但请注意这是个非标准功能，其他浏览器，特别是基于 Chrome 的浏览器可能会不支持。 关于如何引入特殊编程语言，请参见这篇文章。

### 关于 MIME 类型
> [详见-MIME-类型](/2023/02/17/MIME-类型)

##### text/javascript
据 HTML 标准，应该总是使用 MIME 类型 text/javascript 服务 JavaScript 文件。其他值不被认为有效，使用那些值可能会导致脚本不被载入或运行。
script标签的type属性值, 最熟悉的就是text/javascript
> 考虑到约定俗成和最大限度的浏览器兼容, 目前type属性的值依旧是text/javascript, 不过这个属性并不是必须的, 如果没有指定这个属性, 则其 ** `默认值仍为text/javascript` ** [<JavaScript高级程序设计(第三版)>]

```html
<script>...</script>
// script 标签里的代码，会以 javascript 方式解析，因为默认 type = text/javascript
```

##### text/babel

##### application/json
服务端通过`Content-Type`这个响应头来告诉浏览器接收到的响应体的媒体类型到底是什么, 这个媒体类型决定了服务端返回的内容(响应体)，浏览器如何处理, 比如一个常见的Content-Type
> Content-Type: application/json;

##### <font color="#a33">Uncaught SyntaxError: Expected ',' or ']' after array element in JSON at position 100</font>
> 如果 MIME 类型不是 JavaScript 类型（上述支持的类型），则该元素所包含的内容会被当作数据块而不会被浏览器执行。
```html
	// 因为 type="application/json" 浏览器把包含的内容当作数据块，不会被浏览器执行
	<script id="json-script" type="application/json">
		{
		  "data": [
			{
			  "a": 1,
			  "b": 2
			}
			{
			  "c": 3,
			  "d": 4
			}
		  ],
		  "total": 100
		}
	</script>`
	<script type="text/javascript">
		const node = document.getElementById('json-script');
		const jsonStr = node.innerText;
		const json = JSON.parse(jsonStr); // json 串中少 ,
		console.log(json);
	</script>
```