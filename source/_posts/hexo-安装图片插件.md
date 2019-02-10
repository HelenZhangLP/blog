---
title: hexo 安装图片插件 ~ hexo + 七牛云
date: 2019-02-03 11:49:08
categories:
- hexo
tags:
- 图片
- 七牛云
---

想要一块地盘，想要保存宝宝的照片，点点滴
想要宝宝成长开辟地盘
于是。。。
七牛云
hexo

{% qnimg longmao.jpeg title:龙猫 at 七牛云 logo alt:七牛云 'class:' extend:?imageView2/2/w/225 %}

```bash
## 语法
{% qnimg longmao.jpeg title:龙猫 at 七牛云 logo alt:七牛云 'class:' extend:?imageView2/2/w/550 %}
```

<!-- more -->

## hexo 中安装七年云插件

``` bash
$ npm install hexo-qiniu-sync --save
```
> hexo 七牛同步插件 hexo-qiniu-sync
git 地址（https://github.com/gyk001/hexo-qiniu-sync.git）
赞挖井人

## hexo 中配置七牛云
在 `_config.yml` 中配置 qiniu 插件，具体每个参数怎么配置、代表什么在（https://github.com/gyk001/hexo-qiniu-sync.git）中有详细说明，以下是个人配置的 demo

``` bash
qiniu:
  offline: false
  sync: true
  bucket: audreyzhang
  secret_file: a/qn.json # 该配置与 access_key && secret_key 取其一即可
  # access_key: 3***i
  # secret_key: s**d
  dirPrefix: static
  # 外链前缀
  urlPrefix: http://***.bkt.clouddn.com/static
  # 使用默认配置即可
  up_host: http://upload.qiniu.com
  # 本地目录
  local_dir: static
  # 是否更新已经上传过的文件(仅文件大小不同或在上次上传后进行更新的才会重新上传)
  update_exist: true
  image:
    folder: images
    extend:
  js:
    folder: js
  css:
    folder: css
```
### hexo 中配置七牛插件 - ERROR
#### ERROR - 1 - port has been used
> 每次新增图片，不会自动上传到七牛云服务器，需要重新执行 `hexo s`，于是遇见以下错误
{% qnimg portHasBeenUsed.png title: error extend: ?imageView2/2/w/500 %}
**这个很明显，就是端口被占用了。找出启服务的 terminal `ctrl + c` 停止当前服务，`hexo s` 重新启动**

#### ERROR - 2 - get file stat err
> 添加图片，执行 `hexo g` 报错： *get file stat err* 如下图
> {% qnimg getFileStatErr.png title: error extend: ?imageView2/2/w/500 %}
> 逐一排查，发现是 secret_key && access_key 未设置导致的，排查结果如下图
> {% qnimg getFileStatErr-badToken401.png title: error extend: ?imageView2/2/w/500 %}
**`_config.yml` 中添加 access_key && secret_key，有两种方式： **

1. 直接添加
   access_key: 3***i
   secret_key: s**d
2. 新增加 `sec.json`，将 `access_key: 3***i && secret_key: s**d` 写入 json 文件，然后 `_config.yml，qiniu` 插件中，添加 `secret_file: a/qn.json`
**(具体如上 demo，选其一即可。AccessKey/SecretKey 可至 七牛云 个人中心 -> 密钥管理 中查看)**

#### ERROR - 3 - token not specified 图片不展示
> 可能是因为当前空间是私有空间造成的，具体原因可看文档
  https://developer.qiniu.com/fusion/kb/3885/through-the-http-status-code-download-failure-reason
  https://developer.qiniu.com/kodo/kb/4054/matters-needing-attention-of-private-space
