---
title: .bash_profile/.bashrc/.zshrc
date: 2021-07-06 13:41:14
tags:
- macos
- bash
---

![img.png](/images/macos/terminal.png)
灾难性事件，电脑硬盘挂了。那么近几天我所做的是恢复之前的配置<br/>
<font color="#f99">
    需求如上图：
    terminal 中显示 git 分支，当前路径，主机及用户名
</font>

## 几个 bash 配置文件的说明
*   `/etc/profile` 为系统的每个用户设置环境信息，当用户第一次登录时，该文件被执行，并从 `/etc/profile.d` 目录的配置文件中搜集 shell 的设置
*   `/etc/bashrc` 为每个运行 bash shell 的用户执行此文件，当 bash shell 被打开时，该文件被读取
*   `~/.bash_profile` 每个用户可以使用该文件输入专用于自己使用的 shell 信息，**当用户登录时** 该文件仅仅执行一次。默认情况下，他设置一些环境变量，执行用户的 .bashrc 文件
*   `~/.bashrc` 该文件包含专用于你的 bash shell 的 bash 信息，当登录时以及每次打开新的 shell 时，该文件读取

另外,/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承/etc/profile中的变量,他们是"父子"关系.

## login shell
通过终端输入用户名和密码，进入 terminal 的 shell 环境，`ssh 进入远程主机`。
## no-login shell 环境
无需用户名和密码进入的 shell 环境，`从桌面终端进入` 的 shell 环境就是 no-login shell。

> 想通过 login shell 运行的命令放在 `.bash_profile`中，想通过 no login shell 运行的命令则放入 .bashrc 中

> Mac OS 中，运行 termianl 时系统默认运行 login shell。所以 $home 中没 .bashrc

## .bashrc 配置图片需求
<u>terminal 中显示 git 分支，当前路径，主机及用户名</u>
```shell
find_git_branch() {
        git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/[\1]/p'
}

PRCMPT_COMMAND="find_git_branch; $PROCMPT_COMMAND"

## zsh 下 ps1 配置
export PS1="[%9F%n@%10F%M %13F%~ $(find_git_branch)]%15F"


export LS_OPTIONS=’–color=auto’ # 如果没有指定，则自动选择颜色
export CLICOLOR='Yes' #是否输出颜色
export LSCOLORS='CxfxcxdxbxegedabagGxGx' #指定颜色
~                                                   
```

## .bash_profile
```
#.bash_profile 文件中添加如下代码
if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi

#如此 terminal 读取 .bash_profile 文件后 会 load .bashrc 文件内容
```

> 运行 `source .bash_profil` 使配置生效。<font color="#f33">目前这种方式仅一次有效</font>

## .zshrc 
zsh mac 下最好用的终端，新建 `~/.zshrc`，添加 `source ~/.bash_profile` 使 `.bashrc` 下的配置永久有效
```shell
# zshrc 下配置
source ~/.bash_profile 
```

## <font color="#f33">ERROR 切换分支后，分支号没有实时更新</font>
```shell
## ~/.zshrc
source ~/.bashrc

find_git_branch() {
        git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/[\1]/p'
}

setopt PROMPT_SUBST
export PROMPT='%9F%n@%10F%M%f %13F%~%f %F{green}$(find_git_branch)%f %F{normal}$%f '
```
