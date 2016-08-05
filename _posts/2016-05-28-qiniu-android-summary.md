---
layout: post
title: 七牛android使用总结
category: accumulation
tags: 积累
keywords: qiniu, android
description: 七牛android使用总结，包括list，delete，upload，download
---

* 目录
{:toc}


### 1.在android上实现对七牛空间操作
在android上实现对七牛空间的各种操作，包括list，delete，upload，download。支持私有空间

注意：官方不建议开发者把AccessKey和SecretKey放在前端的java文件里，最好还是有一台应用服务器

如果只是想尝试一下，好吧:)  代码中都有说明，直接上代码

### 2.代码

一共有三个类：

  #### 一个工具类、一个config类，一个HMAC-SHA1签名加密类，使用这个类生成对应七牛资源管理里用到的[管理凭证](http://developer.qiniu.com/article/developer/security/access-token.html)

  {% gist f73bcb98af2c0c44a4c49c7c7b7e6bda %}


最后，如果有问题欢迎讨论，我的邮箱简介里有 :)  
