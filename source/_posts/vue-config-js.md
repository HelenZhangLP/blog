---
title: vue.config.js
date: 2019-07-22T11:12:36.000Z
tags: 
- Vue
---

# 全面解读 `vue.config.js`

> [参考](https://cli.vuejs.org/zh/config/)

> ```javascript
> const path = require('path')
> module.exports = {
>   // 基本路径，默认被部署在根路径，如果需要部署在子路径，publicPath 设置为 /example/
>   publicPath: process.env.NODE_ENV === "development" ? "/" : "/mallActivity/",
>   outputDir: 'dist', // 运行 vue-cli-service build 时生成生产环境构建文件的目录
>   assetsDir: 'assets', //生成文件的静态资源目录
>   // productionSourceMap：{ type:Bollean,default:true } 生产源映射
>   // 如果您不需要生产时的源映射，那么将此设置为false可以加速生产构建
>   productionSourceMap: false,
>   css: {
>     loaderOptions: { // CSS 加载器
>       stylus: {
>         javascriptEnabled: true
>       }
>     }
>   },
>   chainWebpack: config => {
>     const types = ['vue-modules', 'vue', 'normal-modules', 'normal']
>     types.forEach(type => addStyleResource(config.module.rule('stylus').oneOf(type)))
>   },
>   devServer: {
>     proxy: { // 在开发环境下将 API 请求代理到 API 服务器
>       '/mock': {
>         target: 'http://10.8.5.180:80',
>         pathRewrite: {
>           '^/mock': '/mock'
>         }
>       }
>     }
>   }
> }
> function addStyleResource (rule) {
>   rule.use('style-resource')
>     .loader('style-resources-loader')
>     .options({
>       patterns: [
>         path.resolve(__dirname, './src/assets/stylus/golbal.styl')
>       ]
>     })
> }
> ```
