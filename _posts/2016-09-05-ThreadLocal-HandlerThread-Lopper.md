---
layout: post
title:  ThreadLocal、HandlerThread、Lopper区别
category: accumulation
tags: 积累
keywords: ThreadLocal, HandlerThread, Lopper
---

* 目录
{:toc}

### 前言

    Android中非UI线程（WorkThread）不能操作UI线程（MainThread）

handler 发送Message 给MessageQueue，Looper 来轮询消息，如果有Message，然后再发送给Handler，Handler 拿到消息就可以所在的线程执行了。

#### ThreadLocal<T>

Thread这个类有一个变量：ThreadLocal.ThreadLocalMap threadLocals ，这是一个map的数据结构，里面的元素的key就是ThreadLocal，value就是我们自定义的一些目标类。我们可以在自己的多线程类中定义好几个ThreadLocal，然后每一个ThreadLocal  put一个特定的目标类，然后以后可以用ThreadLocal get到目标类（用自己作为Thread里map的key），因为每个Thread有自己独自的map，所以这样可以实现每个线程有自己的LocalThread，并且一个Thread里可以有多个LocalThread。

简单理解就是每个线程维护一个map，然后可以用一定的关键字取出这个map里的目标类（比如一个bean），这个“一定的关键字”说的就是这个ThreadLocal 。

ThreadLocal隔离了各个线程，让各线程之间没有什么共享的问题。

参考：[Android 中 Handler，Looper，HandlerThread 的使用](http://www.jianshu.com/p/08cb3665972f)


#### Looper
Looper是Android handler机制的重要组成部分，Looper这个名字起的很形象，翻译过来是：打环的人，就是维护一个循环的人。
Looper里有一个静态变量：private static final ThreadLocal sThreadLocal = new ThreadLocal(); 这是典型的Android里用到ThreadLocal的一个情况，调用Looper.prepare的时候，唯一做的事情就是把sThreadLocal作为key，把一个new出来的looper对象作为value  put到相应线程的map里。然后以后用到Looper.loop的时候，就从这个sThreadLocal里取出这个Looper，然后死循环（阻塞循环）MessageQueue，取出Message并执行message指向的Handler。

#### HandlerThread
HandlerThread就是在普通的Thread基础上加上了Looper的支持，让用户不必自己去创建Looper了。

http://stormzhang.com/designpattern/2016/03/27/android-design-pattern-singleton/
