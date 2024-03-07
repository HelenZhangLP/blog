---
title: macos 常用命令
date: 2020-10-22 11:59:39
tags:
- grocery
- macos
---

空难发生于最近，电脑硬盘坏了，换了新硬盘。新电脑，一切重新开始。
## homebrew 安装
### 安装
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
### <font color="#f33">curl: (7) Failed to connect to raw.githubusercontent.com port 443: 拒绝连接 问题</font>
> DNS 污染造成的
1.  使用 `https://www.ipaddress.com/` 查询 `raw.githubbusercontent.com` 的正确 IP
![img.png](/images/macos/ipaddress.png)    
2.  
```shell
$ vim /etc/hosts
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
```
> 一个打脸的事实，安装好之后不要乱骚情，测试删除。使用的方法版本不对，结果又整出下面的妖

### <font color="#f33">fatal: unable to access 'https://github.com/Homebrew/brew/': LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443</font>
步骤如上，`ipaddress` 中查找 `github` ip。hosts 中配置代理。我的配置如下
```shell
140.82.113.4 github.com
```

## brew 安装 node
```shell
brew install node
```

## macos 解压文件 .rar
commands 参数
```shell
`e`             Extract files without archived paths # 解压缩文件到当前目录
  `l[t[a],b]`     List archive contents [technical[all], bare]  # 列出压缩文件
  `p`             Print file to stdout #打印文件标准输出设备
  `t`             Test archive files # 测试压缩文件
  `v[t[a],b]`     Verbosely list archive contents [technical[all],bare] # 详细列出压缩文件信息
  `x`             Extract files with full path # 用绝对路径解压缩文件

  ```bash
$ unrar x x.rar
```

## macos 文件重命名 [更多相关](https://www.cnblogs.com/liujiacai/p/8313548.html)

```bash
$ mv 切图图片资源/ static
```

## macos 复制文件
```bash
$ cp webpack.config.js webpack.dev.config.js
```

## macos 安装 tree 命令
### 方法一
* `cd $home`
* `vim .bashrc`
* `alias tree = "find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"`
* `source .bashrc`

### 方法二
也可以使用 homebrew 安装 tree 命令行：
`1 $ brew install tree`

## tree 命令解释
```bash
usage: tree [-acdfghilnpqrstuvxACDFJQNSUX] [-H baseHREF] [-T title ]
        [-L level [-R]] [-P pattern] [-I pattern] [-o filename] [--version]
        [--help] [--inodes] [--device] [--noreport] [--nolinks] [--dirsfirst]
        [--charset charset] [--filelimit[=]#] [--si] [--timefmt[=]<f>]
        [--sort[=]<name>] [--matchdirs] [--ignore-case] [--fromfile] [--]
        [<directory list>]
  ------- Listing options -------
  -a            All files are listed. 列出所有的文件夹和目录
  -d            List directories only. 仅列表目录
  -l            Follow symbolic links like directories. 如果遇到符号链接目录，直接列表连接所指的原始目录
  -f            Print the full path prefix for each file. 为每个文件加前缀打印完整的地址
  -x            Stay on current filesystem only. 留在当前文件系统，若指向目录下的某些子文件，其存放在另一个文件系统上，则将该目录排除在寻找范围外
  -L level      Descend only level directories deep. 限制打印目录层级
  -R            Rerun tree when max dir level reached. 
  -P pattern    List only those files that match the pattern given. 只列出给定匹配模式的文件及目录
  -I pattern    Do not list files that match the given pattern. 列出除给定模式以外的文件
  --ignore-case Ignore case when pattern matching.
  --matchdirs   Include directory names in -P pattern matching.
  --noreport    Turn off file/directory count at end of tree listing.
  --charset X   Use charset X for terminal/HTML and indentation line output.
  --filelimit # Do not descend dirs with more than # files in them.
  --timefmt <f> Print and format time according to the format <f>.
  -o filename   Output to file instead of stdout.
  ------- File options -------
  -q            Print non-printable characters as '?'.
  -N            Print non-printable characters as is.
  -Q            Quote filenames with double quotes.
  -p            Print the protections for each file.
  -u            Displays file owner or UID number.
  -g            Displays file group owner or GID number.
  -s            Print the size in bytes of each file.
  -h            Print the size in a more human readable way.
  --si          Like -h, but use in SI units (powers of 1000).
  -D            Print the date of last modification or (-c) status change.
  -F            Appends '/', '=', '*', '@', '|' or '>' as per ls -F.
  --inodes      Print inode number of each file.
  --device      Print device ID number to which each file belongs.
  ------- Sorting options -------
  -v            Sort files alphanumerically by version.
  -t            Sort files by last modification time.
  -c            Sort files by last status change time.
  -U            Leave files unsorted.
  -r            Reverse the order of the sort.
  --dirsfirst   List directories before files (-U disables).
  --sort X      Select sort: name,version,size,mtime,ctime.
  ------- Graphics options -------
  -i            Don't print indentation lines.
  -A            Print ANSI lines graphic indentation lines.
  -S            Print with CP437 (console) graphics indentation lines.
  -n            Turn colorization off always (-C overrides).
  -C            Turn colorization on always.
  ------- XML/HTML/JSON options -------
  -X            Prints out an XML representation of the tree.
  -J            Prints out an JSON representation of the tree.
  -H baseHREF   Prints out HTML format with baseHREF as top directory.
  -T string     Replace the default HTML title and H1 header with string.
  --nolinks     Turn off hyperlinks in HTML output.
  ------- Input options -------
  --fromfile    Reads paths from files (.=stdin)
  ------- Miscellaneous options -------
  --version     Print version and exit.
  --help        Print usage and this help message and exit.
  --            Options processing terminator.
```
> 列出除 `node_modules` 以外的文件
```bash
$ tree -I node_modules
```
> 当前目录下列 2 层子级目录
```bash
$ tree -L 2
```