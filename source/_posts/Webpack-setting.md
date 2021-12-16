---
title: webpack 配置
date: 2018-05-21 10:36:49
tags:
- webpack
---

webpack 建议本地安装，可以在引入重大更新（breaking change）版本时，更容易分别升级项目。通常会通过运行一个或多个 `npm scripts` 以在本地 `node_modules` 目录中查找安装的 webpack。

<font color="#f33">npm v5.2.0 或更高版本，要运行 npx webapck 执行</font>

## 安装 wepack
```bash
npm i webpack webpack-cli webpack-dev-server --save-dev
```
* webpack webpack 的核心文件，必须
* webpack-cli 如果你使用 webpack v4+ 版本，并且想要在命令行调用 webpack 安装 webpack-cli
* webpack-dev-server dev 环境启动服务

四个核心概念
* entry(入口)
* output(输出)
* devServer
  配置本地运行环境
  ```javascript
  // 开发模式下，提供虚拟服务器，用于项目开发和测试
  devServer: {
    port: 3000,
    contentBase: "./dist" // boolean/string/array 服务器资源根目录
  }
  ```
* loader(转换器)
  通过 loader 实现 import 导入任何类型模块，如 .jsx/.css/.less 等
  ` npm i @babel/core @babel/preset-env @babel/preset-react babel-loader -D`
    * @babel/core 是调用 babel 的 API 进行转码的包
    * babel-loader 执行转义的核心包
    * babel-preset-env 一个新的 preset，根据配置目标运行环境启用需要的 babel 插件
    * babel-preset-react 用于转义 react 的 JSX 语法

  ```javascript
  // webpack.config.js
  module: {
    // 配置模块规则 —— 配置 loader
    rules: [
      // webpack 打包时识别后缀为 .js|.jsx 的文件，并用 babel-loader 去转换
      {
        test: /\.(js|jsx)$/, //告诉 webpack 需要处理的对象
        exclude: /node_modules/,
        use: { // 告诉 webpack 用什么来处理
          loader: "babel-loader"
        }
      }
    ]
  }
  ```
  配置相应内容，告诉 babel-loader 使用 ES6 和 JSX 插件
  ```javascript
  // .babelrc
  {
    "presets": ['env', 'react']
  }
  ```
  `npm i style-loader css-loader less-loader -D`
  ```javascript
  // webpack.config.js
  module: {
    rules: [
      {
        test: /\.(less|css)$/,
        exclude: /node_modules/,
        use: ['style-loader','less-loader','css-loader'] // style-loader!less-loader 同时运行两个 loader
      }
    ]
  }
  ```
  <font color="red">Module not found: Error: Can't resolve 'style-loader!css-loader'</font>
* plugins(插件)
    * clean-webpack-plugin 删除 build 或 dist 下的文件，生新的文件
      [<font color="red">使用方式不同版本各有不同，建议参考文档</font>](https://www.npmjs.com/package/clean-webpack-plugin)
  ```javascript
  // webpack.config.js
  const { CleanWebpackPlugin } = require('clean-webpack-plugin');
  module.exports = {
    plugins: [
      new CleanWebpackPlugin();
    ]
  }
  ```

<!-- more -->

### devServer.proxy
有单独的 API 后端开发服务器，且在同一域上发送 API 请求。
高级用法：`http-proxy-middleware`
```javascript
proxy: {
  '/api': {
    target: 'https://qa.amulet.com', // 代理地址
    pathRewrite: { '^api': '' } // 代理  http://qa.amulet.com/api/user 不传递 api
    // 默认不接受在 HTTPS 上运行且证书无效的后端服务器
    secure: false,  // https 代理
    changeOrigin: true // 将请求头中的 host 改为 target 中的地址 
  }
}
```
> umijs 中 changeOrigin 设置无效。需要通过 响应中的 `x-real-url` 查看

## webpack 设置热更新
<font color="red">这种的配置的问题在于，文本修改后不能及时更新，但是解决了`Cannot find module 'webpack-cli/bin/config-yargs'`的问题</font>
1. package.json 配置
```json
"dev": "webpack-dev-server --mode development --hot"
```
2. 入口文件增加
```javascript
if (module.hot) {
  module.hot.accept()
}
```
第二种方式
```javascript
var webpack = require('webpack')
module.exports = {
    devServer: {
      contentBase: './dist',
      hot: true
    },
    plugins: [
      new webpack.HotModuleReplacementPlugin()
    ]
}
```
第三种方式
<font color="red">降低 webpack-cli@3 使用 webpack-dev-server</font>
> 以上三种方式都没有办法解决，多次修改，浏览器自动刷新修改。都是需要重新启动 webpack

### Error: Cannot find module 'webpack-cli/bin/config-yargs'
code: 'MODULE_NOT_FOUND'
> webpack-dev-server --open --mode development
"webpack-cli": "^4.6.0"

<font color="red">**降版本，将 webpack-cli 版本降至 3**</font>
```
$ npm i webpack-cli@3 -D --force
```

### Error
<font color="#f33">not found: Error: Can't resolve 'webpack.dev.config.js'</font>

> 解决 webpack-dev-server --config build/webpack.dev.js
* -- config provide path to a webpack configuration file 给 webpack 提供网页的配置文件

<font color="#f33">TS2307: Cannot find module '@config/env' or its corresponding type declarations.
</font>
> 解决 **注意路径一定要正确**
```javascript
// webpack.config.js
resolve: {
  extensions: ['.ts', '.tsx', '.js'], alias: {
    "@config": path.resolve(__dirname, '../config')
  }
}
```

### Error proxy 配置后返回 public 中的 html 文件
> proxy 中配置错误 **未找到对应的接口地址**
