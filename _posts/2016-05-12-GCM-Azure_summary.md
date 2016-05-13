---
layout: post
title: google cloud message（GCM）和Azure实现Notification总结
category: accumulation
tags: 积累
keywords: gcm, azure, notification
description: google cloud message（GCM）和Azure实现Notification总结
---

* 目录
{:toc}

### 1.相关资料
我也是一知半解，基本上就是根据官方教程来实现，但microsoft的azure文档我找了好久，英文不好，汗。。。

gcm start: https://developers.google.com/cloud-messaging/android/start

azure start: https://azure.microsoft.com/zh-cn/documentation/articles/notification-hubs-android-get-started/

gcm official demo: https://github.com/google/gcm

gcm personal demo: https://github.com/iammert/FastGCM

### 2.遇到的问题
## 1.手机运行官方demo时，发送消息，消息收不到必须切换一下网络才可以

stackoverflow上有人问过这个问题：http://stackoverflow.com/questions/13835676/google-cloud-messaging-messages-sometimes-not-received-until-network-state-cha


## 2.onActivityResult()和onResume()调用顺序
API中这样描述：当你一个Activity是以请求码开始，结束时返回给前页面结果码，页面根据结果码进行相应的信息处理。我们会在返回的页面先接受结果码，然后才调用onResume()。

通常我们还会遇到这样一个问题：
在处理返回页面的数据问题
1.需要从服务器上刷新数据时我们会在onResume()方法里处理
2.而刷新从结束界面返回的数据我们会在onAcitviyResult()方法里面处理

为了避免二者在同一块控件上对数据处理，我们只需加个标识符，在两个方法里进行判断，要用哪个方法进行刷新

