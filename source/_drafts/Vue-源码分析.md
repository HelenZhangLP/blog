---
title: Vue 源码分析
tags:
- vue
---

使用 monorepos(monolithic repository 整体式存储库) 打包
monorepos(monolithic repository 整体式存储库) 是一种项目架构，即一个仓库内包含多个项目（模块，包）。
vue3 中使用 yarn workspace lerna 管理项目

## rollup
Rollup is a module bundler for JavaScript which compiles small pieces of code into something large and more complex, such as a library or application.
<span class='custom-box custom-box-933'>Rollup 是一个 JavaScript 模块打包器，</span> 将小块代码编译为更大、更复杂的代码。例如库或应用程序

### @rollup/plugin-json
@rollup/plugin-json allows Rollup to import data from JSON file
@rollup/plugin-json 允许 Rollup 从 JSON 文件中导入数据

### @rollup/plugin-node-resolve
@rollup/plugin-node-resolve locates modules using the Node resolution algorithm, for using third party modules in node_modules.
一个使用节点解析算法定位模块的汇总插件，用于在Node_modules中使用第三方模块

### Configuration Options
#### output.format
<table>
<thead>
<tr align='center'>
<td colspan="3">指定打包输出格式</td>
</tr>
</thead>
<tbody>
<tr><td>amd</td><td>Asynchronous Module Definition, used with module loaders like RequireJS</td><td>异步模块定义，与RequireJS 等模块加载程序一起使用</td></tr>
<tr><td>cjs</td><td>CommonJS, suitable for Node and other bundlers(alias: Common.js)</td><td>适用于 Node 和 其它 bundlers</td></tr>
<tr><td>es</td><td>CommonJS, suitable for Node and other bundlers(alias: Common.js)</td><td>适用于 Node 和 其它 bundlers</td></tr>
<tr><td>iife</td><td>CommonJS, suitable for Node and other bundlers(alias: Common.js)</td><td>适用于 Node 和 其它 bundlers</td></tr>
<tr><td>umd</td><td>Universal Module Definition, works as amd, cjs, iife all of one</td><td>通用模板定义，集 adm, cjs, iife 于一体</td></tr>
<tr><td>system</td><td>Native format of the SystemJS loader(alias: system.js)</td><td>SystemJS 加载本机格式，别名 system.js</td></tr>
</tbody>
</table>


```bash
$ npm i rollup rollup-plugin-typescript2 @rollup/plugin-node-resolve @rollup/plugin-json execa -D
# @rollup/plugin-json
# @rollup/plugin-node-resolve
# execa // node 插件，用于处理子进程的
# rollup
# typescript
# rollup-plugin-typescript

$ npx tsc --init #自动生成 ts.config 配置文件（node_modules/typescript/bin/tsc）
```

```json
// package.json
{
    // If true, the package is considered private and Yarn will refuse to publish it regardless(不管) of the circumstances（条件）. Setting this flag also unlocks some features that wouldn't make sense in published packages, such as workspaces.

    // 如果为真，该包被认为私有，yarn 不管什么情况都会拒绝发表。设置这个标识，将会解锁这个软件包中一些没意义的功能
    "private": true,
    // Workspaces are an optional feature used by monorepos to split a large project into semi-independent subprojects.each one listing their own set of dependencies，The workspaces field is a list of glob patterns that match all directories that should become workspaces of your application.

    // Workspaces 是 monorepos 用来拆分大型项目为独立子项目的一个可选功能，每个子项目列出自己的依赖项，工作空间字段是一个匹配所有应该成为你应用程序的工作空间的目录的全局模式列表
    "workspaces": ["packages/*"],
    // A field used to list shell scripts that will be executed when running `yarn run`. Script are by default a miniature shell，so some advanced features might not be avaliable（if you have more complex needs, we recommand you to just execute a JavaScript file）Note that scripts containing:(the colon character) are globals to your project and can be called regardless of your current workspace.Finally,be aware that scripts are always executed relative to the closest workspace(never the cwd)

    // 用于列出执行 “yarn run” 时要执行的 shell 脚本的字段。默认情况下，脚本通过微型 shell 执行，所以一些高级的功能可能不可用（如果你有更高的需求，我们推荐你只执行一个 JavaScript 文件）注意脚本包含冒号对于你的项目是全局变量，而且不管你现在哪个工作空间，都可以被调用。最后，请注意，脚本总是相对于最近的工作空间执行，不是 cwd
    "scripts": {
        "dev":"",
        "build": "node scripts/build.js"
    },

    // The set of dependencies that must be made avaliable to the current package in order for it to work properly. Consult the list of supported ranges for more information.
    // 为了使当前包正常运行，依赖项必须对当前包可用的。请参考支持的范围列表，了解更多信息。
    "dependencies": {

    },

    "devDependencies": {
        "@rollup/plugin-json": "^4.1.0", // 允许 Rollup 从 JSON 文件中导入数据
        "@rollup/plugin-node-resolve": "^13.0.4", // 使用节点解析算法定位模块的汇总插件
        "rollup": "^3.21.0", // JavaScript 打包器
        "rollup-plugin-typescript2": "^0.34.1",
        "execa": "",
        "typescript": ""
    }
}
```
---
```json
// packages/activity/package.json
{
    // The name of the package，Used to identity it across the application，especilly amongst multiple workspaces.the first part of name(here @scope/) is optional and is used as a namespace.
    // 包名，用于标识整个应用程序，尤其是多个工作空间。名称的第一部分“@/scope”是可选的，用作命名空间
    "name": "@/scope/name"
}
```

