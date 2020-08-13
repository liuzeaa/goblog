---
layout: minio
title: minIO使用最佳实践
date: 2020-08-13 07:56:13
tags: minio
keywords: minio 分布式存储
categories: minio
---
# minIO使用最佳实践
<!--more-->

## 约定

| 项 | 值 | 说明 | 
| ---- |----|-----| 
| endPoint | 192.168.2.250 | minio服务器的ip，或者是代理服务器的ip | 
| port | 9000 | 约定不变 | 
| useSSL | false | 约定不变 | 
| accessKey | admin | 研发测试环境使用此值，生产环境要改变 | 
| secretKey | password | 研发测试环境使用此值，生产环境要改变 | 

## 路径规范

| 项 | 路径 | 场景 | 
| ---- | ---- |  ---- | 
| bucket | 每个项目使用唯一的bucket名称，如ZStack_dev、ZStack_test...... | bucket的名称在go的配置文件配置 | 
| 工具库 | bucket/gongju/双日期/filename（随机） | 上传工具 | 
| 漏洞库 | bucket/loudong/双日期/filename（随机） | 上传漏洞 | 
| 武器库 | bucket/wuqi/双日期/filename(随机) | 上传武器 | 
| 文档 | bucket/document/双日期/filename(随机) | 上传文档 | 
| 书籍 | bucket/book/双日期/filename(随机) | 上传书籍 | 
| 文章 | bucket/article/双日期/filename（随机） | 文章中上传图片 | 
| 文件 | bucket/file/双日期/filename（随机） | 用户上传文件 | 
| 截图 | bucket/screenshot/双日期/filename（随机） | 截图后生成的图片 | 
| 未分类 | bucket/other/双日期/filename（随机） | 新业务场景产生，还未来得及分类的文件统一放在未分类目录 | 

## 接口

| 接口 | 接口人 | minIO接口 | 文档地址 | 说明 | 
| ---- |----|----| ----| ----| 
| 创建存储桶 | 后端 | 非接口 | [https://docs.minio.io/cn/golang-client-api-reference.html#MakeBucket](https://docs.minio.io/cn/golang-client-api-reference.html#MakeBucket) | 在Golang启动时进行创建，只创建一次 | 
| 文件上传步骤1 | 后端 | PresignedPutObject(bucketName, objectName string, expiry time.Duration) (*url.URL, error) | [https://docs.minio.io/cn/golang-client-api-reference.html#PresignedPutObject](https://docs.minio.io/cn/golang-client-api-reference.html#PresignedPutObject) | 通过浏览器直接上传一个文件到S3服务，而不需要暴露S3服务的认证信息给这个用户。<br>该接口会返回上传文件的url地址 | 
| 文件上传步骤2 | 前端 |  |  | 从一个stream/Buffer中上传一个对象 | 
| 文件下载 | 后端 | PresignedGetObject(bucketName, objectName string, expiry time.Duration, reqParams url.Values) (*url.URL, error) | [https://docs.min.io/cn/golang-client-api-reference.html](https://docs.min.io/cn/golang-client-api-reference.html) | 生成一个给HTTP GET请求用的presigned URL。浏览器/移动端的客户端可以用这个URL进行下载，即使其所在的存储桶是私有的。这个presigned URL可以设置一个失效时间，默认值是7天。 | 

## 技术细节

- 文件的上传大小，通过前端进行限制，nginx也要进行限制
