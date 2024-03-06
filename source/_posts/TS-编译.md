---
title: TS 编译
date: 2022-03-06 23:52:09
tags:
- TypeScript
---

## 命令行中编译选项
|选项|描述|
|---|---|
|-w/--watch|在监视模式下运行编译器|

## tsConfig.json 
tsConfig.json 是 ts 编译器的配置文件，ts 编译器将根据 tsConfig.json 中的配置信息对代码进行编译。
> 根目录下存在 `tsConfig.json`， terminal 中运行以下命令行，<span class='custom-box custom-box-933'>将编译当前目录及子目录中的所有 ts 文件</span>
```bash
$ tsc
```

### 配置介绍
#### include
<span class='custom-box custom-box-393'>`include` 指定需要编译的文件路径</span>

```JavaScript
// tsConfig.js
{
    "include": [
        "./src/**/*",
        "..."
    ],  // 配置需要编译的文件
}
```
* `**` 递归匹配任意子文件夹
* `*` 匹配任意文件

#### exclude
<span class='custom-box custom-box-393'>`exclude` 指定不需要编译的文件路径</span>
<span class='custom-box custom-box-933'>`exclude` 默认配置为 `["node_modules", "bower_components","jspm_packages"]`</span>
```JavaScript
// tsConfig.json
{
    "include": [
        "./src/**/*",
        "..."
    ],  // 配置需要编译的文件
    "exclude": [
        "./src/compiled/*"
    ] // 配置不需要编译的文件
}
```

#### extends
<span class='custom-box custom-box-393'>`extends` 指定需要继承的配置文件</span>
```JavaScript
// tsConfig.json
{
    "include": [
        "./src/**/*",
        "..."
    ],  // 配置需要编译的文件
    "exclude": [
        "./src/compiled/*"
    ], // 配置不需要编译的文件
    "extends": [
        "../config.json"
    ]
}
```

#### files
<span class='custom-box custom-box-393'>`files` 具体指明编译的文件列表</span><span class='custom-box custom-box-933'>只有需要编译的文件少时才会用到</span>
```JavaScript
    {
        "files": [
            "./index.ts",
            "./app.ts"
        ]
    }
```

#### compilerOptions
ts 编译器选项
> `target` 要将 ts 编译为 ES 的哪个版本
```JavaScript
{
    "files": [
        "./variable/01.ts" // 该文件下存在变量赋值为 BigInt 类型，所以 ts 编译版本不能低于 ES2020
    ],
    "compilerOptions": {
        "target": "ES2020" // 'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'es2022', 'esnext'.
    }
}
```
> `module` 指定使用的模块化规范，[【更多关于模块化】](/2019/03/26/Javascript-模块化/)
```JavaScript
    {
        "compilerOptions": {
            "target": "es6",
            "module": "es6" // 'none', 'commonjs', 'amd', 'system', 'umd', 'es6', 'es2015', 'es2020', 'es2022', 'esnext', 'node16', 'nodenext'.
        }
    }
```
> `lib` 指定项目中要使用的库
<font color='red'>Cannot find name 'console'. Do you need to change your target library? Try changing the 'lib' compiler option to include 'dom'.</font>

```JavaScript
    {
        "compilerOptions": {
            // lib 库设置选项包括：
            //  'es5', 'es6', 'es2015', 'es7', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'es2022', 'es2023', 'esnext', 'dom', 'dom.iterable', 'webworker', 'webworker.importscripts', 'webworker.iterable', 'scripthost', 'es2015.core', 'es2015.collection', 'es2015.generator', 'es2015.iterable', 'es2015.promise', 'es2015.proxy', 'es2015.reflect', 'es2015.symbol', 'es2015.symbol.wellknown', 'es2016.array.include', 'es2017.date', 'es2017.object', 'es2017.sharedmemory', 'es2017.string', 'es2017.intl', 'es2017.typedarrays', 'es2018.asyncgenerator', 'es2018.asynciterable', 'es2018.intl', 'es2018.promise', 'es2018.regexp', 'es2019.array', 'es2019.object', 'es2019.string', 'es2019.symbol', 'es2019.intl', 'es2020.bigint', 'es2020.date', 'es2020.promise', 'es2020.sharedmemory', 'es2020.string', 'es2020.symbol.wellknown', 'es2020.intl', 'es2020.number', 'es2021.promise', 'es2021.string', 'es2021.weakref', 'es2021.intl', 'es2022.array', 'es2022.error', 'es2022.intl', 'es2022.object', 'es2022.sharedmemory', 'es2022.string', 'es2022.regexp', 'es2023.array', 'es2023.collection', 'esnext.array', 'esnext.collection', 'esnext.symbol', 'esnext.asynciterable', 'esnext.intl', 'esnext.disposable', 'esnext.bigint', 'esnext.string', 'esnext.promise', 'esnext.weakref', 'esnext.decorators', 'decorators', 'decorators.legacy'.
            lib: ["es6", "dom"] // 一般不需要修改
        }
    }
```
> `outDir` 指定编译后文件的存储目录
```JavaScript
    {
        "compilerOptions": {
            "outDir": "./dist"
        }
    }
```
> `outFile` 将代码合并为一个文件
<span class='custom-box custom-box-933'>设置 `outFile` 扣，所有的全局作用域中的代码会合并到同一个文件中</span>
<font color='red'>Only 'amd' and 'system' modules are supported alongside --outFile.</font>

```JavaScript
    {
        "compilerOptions": {
            "module": "amd", // Asynchronous Module Definition，Require.js 规范
            "outFile": "./dist/outFile.js"
        }
    }
```

> `allowJs` 是否编译 js 文件，默认 false 不编译；`checkJs` 是否对 js 文件进行 ts 语法检查，默认不检查
```JavaScript
// check.js
let a = 1
a = 'hello' //不能将类型“string”分配给类型“number”。ts(2322)
```
---
```JavaScript
    {
        "compilerOptions": {
            "module": "amd", // Asynchronous Module Definition，Require.js 规范
            "outFile": "./dist/outFile.js",
            "allowJs": true, // 默认 false
            "checkJs": true, // 默认 false
        }
    }
```
>    * `removeComments` 编译时移除注释
>    * `noEmit` 不生成编译后的文件 <span class='custom-box custom-box-993'>使用频率低，语法检查时使用</span>
>    * `noEmitOnError` 编译错误时不生成编译后文件

```JavaScript
{
    "compilerOptions": {
        "module": "amd", // Asynchronous Module Definition，Require.js 规范
        "outFile": "./dist/outFile.js",
        "removeComments": true,
        "noEmitOnError": true,
        "noEmit": true
    }
}
```
> `noImplicitAny` 不允许使用隐匿 `any` 类型
```JavaScript
// app.ts
// 使用隐式 any
// 参数 "a" 隐式具有 "any" 类型，但可以从用法中推断出更好的类型。ts(7044)
// 参数 "b" 隐式具有 "any" 类型，但可以从用法中推断出更好的类型。ts(7044)
let fn = function (a, b) {
    return a + b
}
```
----
> `noImplicitAny` 开启
```JavaScript
// tsConfig.json
{
    "compilerOptions": {
        "module": "amd", // Asynchronous Module Definition，Require.js 规范
        "outFile": "./dist/outFile.js",
        "removeComments": true,
        "noEmitOnError": true,
        "noEmit": true,
        "noImplicitAny": true
    }
}
```
<font color='red'>Error:</font>
```JavaScript
// 使用隐式 any
// 参数“a”隐式具有“any”类型。ts(7006)
// 参数“b”隐式具有“any”类型。ts(7006)
let fn = function (a, b) {
    return a + b
}
```

> `noImplicitAny` 不允许不明确类型的this {noImplicitAny:true}
```JavaScript
    // app.ts
    // this 的值取决于它的调用方式，浏览器直接调用 this，this 则指向 window
    // 对象方法调用，this 指向当前对象
    let fn = function() {
        console.log(this)
    }

    // parse/app.ts:16:25 - error TS2683: 'this' implicitly has type 'any' because it does not have a type annotation.

    let fn = function(this: any) {
        console.log(this)
    }
```

> `strictNullChecks` 检查语法是否存在空值
```JavaScript
// tsConfig.json
{
    "compilerOptions": {
        "module": "amd", // Asynchronous Module Definition，Require.js 规范
        "outFile": "./dist/outFile.js",
        "removeComments": true,
        "noEmitOnError": true,
        "noEmit": true,
        "noImplicitAny": true,
        "strictNullChecks": true
    }
}
```
---
```JavaScript
    let box = document.getElementById('box')
    box.innerHtml = '<div>null</div>'
```
<font color='red'>TS2531: Object is possibly 'null'</font>
```JavaScript
    let box = document.getElementById('box')
    box?.innerHtml = '<div>null</div>'
```

> `strict` 所有严格检查的总开关