---
title: Node 工具包
date: 2020-02-17 14:34:52
tags:
- node
---

### tools
#### nrm
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

#### i5ting_toc 把 md 文档转换为 html 页面的工具
```bash
zhangliping@zhangliingdembp /  $ npm install -g iString_toc # 将 iString_toc 安装为全局包
zhangliping@zhangliingdembp /  $ iString_toc -f ./name.md -o # 调用 iString_toc 将 md 转换为 html 
```

#### nvm node 版本管理工具
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