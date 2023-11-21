---
title: Node 工具包
date: 2020-02-17 14:34:52
tags:
- Node
---

## tools
### nodemon
<span class='custom-box custom-box-933'>问题：</span>在编写 `node.js` 项目时，修改项目代码后，需要 `ctrl+c` 停止项目，再重启。

> [nodemon](https://www.npmjs.com/package/nodemon)工具，可以监听项目变化，项目代码修改后，nodemon 自动重启项目，方便开发调试。
nodemon is a tool that helps develop node.js based applications by automatically restarting the node application when file changes in the directory are detected.
nodemon 是一个帮助开发 node 的工具，当文件夹中的文件被检测到变化时，自动重启 node 应用。
nodemon does not require any additional changes to your code or method of development. nodemon is a replacement wrapper for node. to use nodemon,replace the word node on the command line when executing your script
nodemon 不需要对你的代码或开发方法做额外的改变，
nodemon 是 `node` 的替换包。要使用 nodemon，需要在执行脚本时替换命令行中的 `node`

```bash
$ npm install -g nodemon
```

### pm2
服务持久化管理的 <span class='custom-box custom-box-939'>即使终端关闭，服务器也在，电脑重启后，服务器关闭。</span>
```bash
$ npm i pm2 -g #安装 pm2
$ pm2 start server.js --name TASK #启动
$ pm2 restart TASK #重启
$ pm2 stop TASK #暂停
$ pm2 delete TASK #删除
```

### nrm
由于 npm 官方镜像源在国外，下包时会存在下载慢的问题。所以我们一般在将镜像源切换为淘宝镜像源
```bash
zhangliping@zhangliingdembp ~/Documents/vite  $ npm config get reigstry # 查看当前下包的镜像源
zhangliping@zhangliingdembp ~/Documents/vite  $ npm set registry=https://registry.npm.taobao.org/ # 将下包的镜像源切换为淘宝的镜像源
zhangliping@zhangliingdembp ~/Documents/vite  $ npm config get registry # 查看镜像源是否下载成功
```
<div class="custome-box custom-box-933">这里需要注意，淘宝镜像源地址一定要写正确。</div>
为了避免镜像源写错的问题，这里介绍一个工具<span class="custome-box custom-box-393">nrm 利用 nrm 提供的终端命令，可快速切换</span>

```bash
zhangliping@zhangliingdembp ~/Desktop/node/src/demo5/中文/leave a blank space  $ npm i nrm -g  # 全局安装 nrm 工具
zhangliping@zhangliingdembp ~/Desktop/node/src/demo5/中文/leave a blank space  $ nrm ls # 查看可用的镜像源
zhangliping@zhangliingdembp ~/Desktop/node/src/demo5/中文/leave a blank space  $ nrm use taobao # 使用淘宝镜像
```

### i5ting_toc 把 md 文档转换为 html 页面的工具
```bash
zhangliping@zhangliingdembp /  $ npm install -g iString_toc # 将 iString_toc 安装为全局包
zhangliping@zhangliingdembp /  $ iString_toc -f ./name.md -o # 调用 iString_toc 将 md 转换为 html 
```

### nvm node 版本管理工具
> 安装 use the following cURL command 脚本 [具体参考](https://github.com/nvm-sh/nvm）

```bash
# install script use the following cURL command
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash

# attempts to add the source lines from the snippet below to the correct profile file (~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc).
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
> 与 n 相同，nvm 同样是管理 node 版本的。

## spm Sea.js 安装工具

## npx
This command allows you to run an arbitrary command from an npm package(either one installed locally, or fetched remotely) in a similar context as running it via `npm run`.
该命令允许你运行 npm 包中的任一命令（本地安装或远程获取）如同运行通过 `npm run` 在相同环境中运行它。

Whatever packages are specified by the `--package` option will provied in the PATH of the executed command，along with any locally installed package executables. The `--package` option may be specified multiple times, to execute the supplied command in an environment where all specified packages are availiable
不管 "--package" 指定的任何包都将提供给执行命令的 PATH，以及任何本地安装的可执行包。“--package” 选项可以被指定多次。在所有指定包的环境中执行提供的命令均可用。

If any requested packages are not present in the local project dependencies，then they are installed to a folder in the npm cache,which is added to the `PATH` environment variable in the executed process. A prompt is printed(which can be suppressed by providing either -- yes or  --no)
如果任何请求包在本地项目依赖中不存在，那么它们会被安装到 npm 缓存的文件夹中，添加到执行过程中的 PATH 环境变量。提示被打印（通过提供 --yes 或 --no 控制）

Package names provided without a specifier will be matched with Whatever version exists in the local project，Package names with a specifier will only be considered a match if they have the exact same name and version as the local dependency.
提供不带说明符的包名将被与本地项目中任何存在的版本相匹配，带有说明符的包名在本地依赖项中有着完全相同的名称和版本将被认为匹配。