---
title: HTML-为什么
tags:
  - Html
date: 2023-02-22 14:51:03
---


## 1. p 标签里为什么不能包含块级元素
> [DTD(Document Type Definition)](w3.org/TR/html4/sgml/dtd.html)
```dtd
<!ELEMENT P - O (%inline;)*            -- paragraph -->
```
> 表示 p 元素只能包含内联元素，[The P element represents a paragraph. It cannot contain block-level elements (including P itself).](https://www.w3.org/TR/html401/struct/text.html#h-9.3.1)