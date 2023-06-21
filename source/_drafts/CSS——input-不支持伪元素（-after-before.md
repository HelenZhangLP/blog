---
title: CSS——input 不支持伪元素（:after,:before)
tags:
- CSS
---

> Authors specify the style and location of generated content with :before and :after pseudo-elements
  作者用伪元素 :after 和 :before 定义生成内容的样式和位置
  as their names indicate, the :before and :after pseudo-elements specify the location of content before and after an element's document tree content
  如他们的名字所示，:before 和 :after 伪类定义了一个元素文档树内容前后的内容的位置和样式
  `content` 定义插入的内容
  一个元素文档树之前或之后的内容，元素可以插入内容，那么元素必须是一个容器
  一个元素文档树内容之前和之后的内容就是指这个元素是要可以插入内容的，也就是说这个元素要是一个容器。
  使用之后将给指定元素添加为子元素,而“content”属性与这些伪元素一起为指定插入的内容。
  input 元素为单标签，没有独立的闭合标签，无法容纳别的标签作为子元素，所以在其中插入元素无法实现

>
According to the pseudo-elements spec(http://dev.w3.org/csswg/css-pseudo-4/#generated-content) pseudo-elements like ::before and ::after generate boxes as if they were immediate children of their originating element. And the HTML5 spec(http://www.w3.org/TR/html5/forms.html#the-input-element) says that the content model of <input> elements is empty, so they don't have any children. In which case ::before and ::after pseudo-elements don't apply to them.We have already UseCounted the feature and now intend to show deprecation warning until M53 at least and decide on further action regarding removal for M54.

input，img，iframe等元素都不能包含其他元素，所以不能通过伪元素插入内容。至于Chrome 中checkbox和radio可以插入，那应该是Bug
