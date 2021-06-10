---
title: webpack 配置
tags:
- webpack
---
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

## Error: Cannot find module 'webpack-cli/bin/config-yargs'
code: 'MODULE_NOT_FOUND'
> webpack-dev-server --open --mode development
"webpack-cli": "^4.6.0"

<font color="red">**降版本，将 webpack-cli 版本降至 3**</font>
```
$ npm i webpack-cli@3 -D --force
```

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