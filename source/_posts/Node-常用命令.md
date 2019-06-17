---
title: Node 常用命令
date: 2019-02-08 10:16:39
categories:
- 技术
tags:
- node
---

#### npm
```
Usage: npm <command>

where <command> is one of:
    access, adduser, audit, bin, bugs, c, cache, ci, cit,
    clean-install, clean-install-test, completion, config,
    create, ddp, dedupe, deprecate, dist-tag, docs, doctor,
    edit, explore, get, help, help-search, hook, i, init,
    install, install-ci-test, install-test, it, link, list, ln,
    login, logout, ls, org, outdated, owner, pack, ping, prefix,
    profile, prune, publish, rb, rebuild, repo, restart, root,
    run, run-script, s, se, search, set, shrinkwrap, star,
    stars, start, stop, t, team, test, token, tst, un,
    uninstall, unpublish, unstar, up, update, v, version, view,

    whoami
    npm <command> -h  quick help on <command>
    npm -l            display full usage info
    npm help <term>   search for help on <term>
    npm help npm      involved overview


Specify configs in the ini-formatted file:
    /Users/lipingzhang/.npmrc
or on the command line via: npm <command> --key value
Config info can be viewed via: npm help config

npm@6.8.0 /usr/local/lib/node_modules/npm
```

> 退出 node 环境
1. 两次ctrl+C
2. 一次ctrl+D
3. `process.exit()`
4. .exit
