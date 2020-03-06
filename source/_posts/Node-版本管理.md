---
title: Node 版管理
date: 2020-02-17 14:34:52
tags:
- Node
---

### nvm node 版本管理工具
#### 安装 use the following cURL command 脚本 [具体参考](https://github.com/nvm-sh/nvm）

```bash
# install script use the following cURL command
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash

# attempts to add the source lines from the snippet below to the correct profile file (~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc).
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
<!--more-->
> 与 n 相同，nvm 同样是管理 node 版本的。
