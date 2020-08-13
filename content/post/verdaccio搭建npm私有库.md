---
layout: verdaccio
title: verdaccio搭建npm私有库
date: 2020-08-13 14:37:01
tags: verdaccio
---

# verdaccio搭建npm私有库
搭间私有库的原因很简单，目前正在开发一个组件库，提供给公司内部使用，我不想去注册npm，也不想等待npm的审核，只想要有个仓库快速测试发布自己的npm包
<!--more-->

```js
# 全局安装
npm install verdaccio - g

verdaccio
```
## pm2守护进程
- 安装：`npm install pm2 -g`
- 启动：`pm2 start verdaccio`
- 停止：`pm2 stop verdaccio`
- 重启：`pm2 restart verdaccio`
- 删除应用：`pm2 delete verdaccio`
- 查看日志：`pm2 logs verdaccio`

## 使用
- 全局设置
    
    `npm set registry http://localhost:4873/`
- 局部设置 

    `NPM_CONFIG_REGISTRY=http://localhost:4873 npm i`

## 发布
1. 添加一个用户并且登录 
    
    `npm adduser --registry http://localhost:4873`
2. 发布你的包
    
    `npm publish --registry http://localhost:4873`

## Docker
```js
docker pull verdaccio/verdaccio

docker pull verdaccio/verdaccio:4

docker run -it --rm --name verdaccio -p 4873:4873 verdaccio/verdaccio
```
