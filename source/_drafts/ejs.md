---
title: ejs
date: 2020-05-29 10:13:25
tags:
---

EJS 一套简单高效的模板语言，当前博客的主题便是用它开发的。它可以利用 Javascript 代码生成 html 页面。

### EJS 笔记
> <%- 输出非转义的数据到模板
```
<%- partial('_partial/head') %>
```

> <% '脚本' 标签，用于流程控制，无输出。
```
<%
  var title = page.title;

  if (is_archive()) {
    title = __('archive_a');
    if (is_month()) {
      title += ': ' + page.year + '/' + page.month;
    } else if (is_year()) {
      title += ': ' + page.year;
    }
  } else if (is_category()) {
    title = __('category') + ': ' + page.category;
  } else if (is_tag()) {
    title = __('tag') + ': ' + page.tag;
  }
%>
```
