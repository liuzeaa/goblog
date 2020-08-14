---
title: "MacOS常用TerminalProxy方法"
date: 2020-08-14T14:41:34+08:00
draft: true
---

首先配置好自己本地的sock监听地址
<!--more-->

## 打开.zshrc
```bash
vi ~/.zshrc 
```
## 增加 proxy | unproxy
```bash
alias proxy="
    export HTTP_PROXY=socks://127.0.0.1:1080;
    export HTTPS_PROXY=socks://127.0.0.1:1080;
    export ALL_PROXY=socks://127.0.0.1:1080;
    export NO_PROXY=socks://127.0.0.1:1080;"
alias unproxy="
    unset HTTP_PROXY;
    unset HTTPS_PROXY;
    unset ALL_PROXY;
    unset NO_PROXY;"
```
## 使用 .zshrc
```bash
source ~/.zshrc
```
## 测试 proxy 效果
```bash
curl ip.sb
>> xxx.xxx.xxx.xxx
proxy
curl ip.sb
>> xxx.xxx.xxx.xxx
```
