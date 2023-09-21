---
title: hapi
date: 2023-08-16 11:48:05
tags:
---

导入 joi
```JavaScript
const Joi = require('joi')
```
## Validation

This tutorial is compatible with hapi v17 and newer; and joi v16 and newer.
该教程兼容 hapi 17+ 和 joi 16+

### Overview
validating data can be very helpful in making sure that your application is stable and secure


## Joi
### any
#### any.required()
Marks a key as required which not allow `undefined` as value. All keys are optional by default.
标记密钥必填不能使用 `undefined`，所有 key 默认可选。
```JavaScript
joi.string().required()
```
### string
Generates a schema object that matches a string data type.
Joi.string() 生成一个匹配字符串数据类型的架构对象。
> Note that the empty string is not allowed and must be enabled with `allow('')`. empty string is not a valid string by default.
> 注意：空白字符串不允许使用，默认情况下不是有效字符串

`Joi.string().allow('')` 允许为空字符串
`Joi.string().empty('').default('default value')` 为空字符串指定默认值

#### string.alphanum()
Requires the string value to only contain a-z, A-Z, and 0-9
请求值只能包含 a-z, A-Z, 0-9
> Joi.string().alphanum() 当前字段验证规则为字符串只包含 a-z, A-Z, 0-9

#### string.min(limit, [encoding])
Specifies the minimum number string characters where:
指定字符串最少包含几个字符，其中：
* `limit` - the minimum number of string characters required or a reference.(请求或引用的字符串最少包含几个字符)
* `encoding` - if specified, the string length is calclulated in bytes using the provided encoding.(如果指定，使用指定的编码用字节计算字符长度)
> Joi.string().min(2) 验证规则为最少包含两个字符

#### string.max(limit, [encoding])
Specifies the maximum munber of string characters where:
指定字符字符的最大长度，其中：
* `limit` - the maximum mumber of string characters allowed or reference.(允许或引用的字符串的最大字符数)
* `encoding` - if specified, the string length is calclulated in bytes using provided encoding.(如果指定，使用指定的编码方式以字节的方式计算字符串长度)
> Joi.string().max(10) 字符串类型，最多包含 10 个字符

#### string.pattern(regex,[name | options])
Defines a pattern rules where:
定义模式规则，其中：
* `regex` - a regular expression object the string value must match against.(字符串值必须与正则表达式值相匹配)Note that if the pattern is a regular expression(注意：如果模式是一个正则表达式),for it to match the entire key name, it must begin with `^` and end with `$`.(为了能够匹配整个键名，必须以 ^ 开头，以 $ 结尾)
* `name` - optional name for patterns(useful with multiple patterns).(模式的可选名称，适用于多个模式)
* `options` - an optional configuration object with the following supported properties:(一个带有以下支持属性的可选配置对象)
    * `name` - optional pattern name (可选模式名称)
    * `invert` - optional boolean flag. Defaults to `false` behavior. if specified `true`,the provided pattern will be disallowed instead of required.(可选的布尔标识，默认为 false，如果设置为 true. 则提供的模式将被禁用，不是必须的)

```JavaScript
const invertedNamedSchema = Joi.string().pattern(/^[a-z]+$/, { name: 'alpha', invert: true });
invertedNamedSchema.validate('lowercase'); // ValidationError: "value" with value "lowercase" matches the inverted alpha pattern
```