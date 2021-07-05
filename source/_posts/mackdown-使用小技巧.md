---
title: markdown 使用小技巧
date: 2021-06-11 11:58:36
tags:
- Markdown
---

## 个人 markdown 编写规则
* \#f33 特别注意的问题
* &gt; 知识点
* \#f99 弱提醒

## <font color="#f33">markdown mermaid graph 不能用 `end` 标签</font>

## markdown 显示部分，省略其余
> <!-- more -->

## markdown 中使用 mermaid 编制流程图
> graph TD <font color="#f33">td 要大写</font>

## markdown 中编辑一些代码动图
[keynote 制作代码编辑动图](https://juejin.cn/post/6909481718156099597#heading-0 )

## markdown 中插入图片，需要重启服务

## table 中 换行
> 使用 `<br />` 标签

## markdown 中怎么定义变量并累加其实字符串，如下：
```md
// 定义变量
[url]: https://baidu.com
[url测试][url]+/a/b/index.html
```
