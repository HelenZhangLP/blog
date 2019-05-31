---
title: 分析 hexo 脚手架
date: 2019-05-21 10:36:49
tags:
- node
---

```bash
$: hexo

Usage: hexo <command>

Commands:
  help     Get help on a command.
  init     Create a new Hexo folder.
  version  Display version information.

Global Options:
  --config  Specify config file instead of using _config.yml
  --cwd     Specify the CWD
  --debug   Display all verbose messages in the terminal
  --draft   Display draft posts
  --safe    Disable all plugins and scripts
  --silent  Hide output on console

For more help, you can use 'hexo help [command]' for the detailed information
or you can check the docs: http://hexo.io/docs/
```
<!-- more -->
1.  help
2.  init
3.  version

### 引入依赖包
```javascript
var chalk = require('chalk'); // chalk 粉笔  "chalk": "^1.1.3",
var tildify = require('tildify'); // tildify 使变薄 "tildify": "^1.2.0" convert an absolute path to a tilde path 将绝对路径转换为 ~/路径
var Promise = require('bluebird'); // bluebird 知更鸟 "bluebird": "^3.4.0",
var Context = require('./context');
var findPkg = require('./find_pkg');
var goodbye = require('./goodbye');
var minimist = require('minimist'); //minimist 参数解析
var resolve = require('resolve'); // 同步或异步解决方案
var camelCaseKeys = require('hexo-util/lib/camel_case_keys'); // 参数对象转换为驼峰命名 (?)
```

### ./lib/hexo.js
```javascript
function entry(cwd, args) {
  cwd = cwd || process.cwd();
  /**
   * 返回 node 进程工作的当前目录    
   * '/Users/lipingzhang/Desktop/hexo-cli的副本'
   */

  /**
   * process.argv
   * [ '/usr/local/bin/node', '/Users/lipingzhang/Desktop/hexo-cli的副本/bin/hexo', 'init' ]
   * 返回一个包含 node process 命令行启动时传入的参数数组
   * 第一个参数 process.execPath 启动 node 进程的可执行文件绝对路径
   * 第二个参数是将要执行的文件路径
   * 其它为所有其余命令行参数
   * process.argv.slice(2) [ 'init' ]
   */
   /**
    * minimist minimist(process.argv.slice(2))
    * { _: Array(1) } { _: [ 'init' ]}
    * camelCaseKeys 参数对象转换为驼峰命名
    */
  args = camelCaseKeys(args || minimist(process.argv.slice(2)));

  var hexo = new Context(cwd, args);
  var log = hexo.log;

  // Change the title in console
  process.title = 'hexo';
}
```

```javascript
var pathFn = require('path');

function checkPkg(path) {
  var pkgPath = pathFn.join(path, 'package.json');

  return fs.readFile(pkgPath).then(function(content) {
    var json = JSON.parse(content);
    if (typeof json.hexo === 'object') return path;
  }).catch(function(err) {
    if (err && err.cause.code === 'ENOENT') {
      var parent = pathFn.dirname(path);

      if (parent === path) return;
      return checkPkg(parent);
    }

    throw err;
  });
}
```

#### ./context.js
```javascript
'use strict';

var logger = require('hexo-log');
var chalk = require('chalk');
var EventEmitter = require('events').EventEmitter;
var Promise = require('bluebird');
var ConsoleExtend = require('./extend/console');

function Context(base, args) {
  base = base || process.cwd();
  args = args || {};

  EventEmitter.call(this);

  this.base_dir = base;
  this.log = logger(args);

  this.extend = {
    console: new ConsoleExtend()
  };
}
```

#### ./extend/console.js
```javascript
// constructor Console  --> Initialization parameters store && alias
function Console() {
  this.store = {};
  this.alias = {};
}

Console.prototype.get = function(name) {
  name = name.toLowerCase(); // 入参转换为小写
  return this.store[this.alias[name]];
};

Console.prototype.list = function() {
  return this.store;
};

Console.prototype.register = function(name, desc, options, fn) {
  if (!name) throw new TypeError('name is required');

  if (!fn) {
    if (options) {
      if (typeof options === 'function') {
        fn = options;

        if (typeof desc === 'object') { // name, options, fn
          options = desc;
          desc = '';
        } else { // name, desc, fn
          options = {};
        }
      } else {
        throw new TypeError('fn must be a function');
      }
    } else {
      // name, fn
      if (typeof desc === 'function') {
        fn = desc;
        options = {};
        desc = '';
      } else {
        throw new TypeError('fn must be a function');
      }
    }

    if (fn.length > 1) {
      fn = Promise.promisify(fn);
    } else {
      fn = Promise.method(fn);
    }
  }
```
