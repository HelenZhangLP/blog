---
title: web console
tags:
---

## 将console.log分配给另一个对象（Webkit问题）
```javascript
var _ = {}
if (typeof window.console !== 'undefined' && typeof window.console.debug == "function") {
  _.log = function () {
    window.console.debug.apply(window.console, arguments)
  }
} else {
  _.log = function () {}
}
```

First, if `console` is indeed undefined(as it is in browsers such as IE), you'll get an error. You should check it instead as a property of the global object. which is `window` in browsers. It's also a good idea in general to test a feature before using, so I've added a test for the `debug` method.
Possibly the implementation of `console.debug` in Safari relies on its value of `this` being a reference to `console`, which will not be the case if you call it using `_.log`(`this` will instead be a reference to _). having done a quick test, this does seem to be the case and the following fixed the problem.
