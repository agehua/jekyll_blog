---
layout: post
title: Android手机定位服务
category: technology
tags: 积累
keywords: LocationListener
description: Android使用手机定位服务使用，GPS服务不可用则跳转到手机位置服务设置页面
---

* 目录
{:toc}

### 1.Android Webview的坑

- 1.webview再次加载页面空白

  1.可以关闭掉硬件加速


  2.不关闭硬件加速的情况下：在关闭Acivity之前手动调用下面方法，
  {%highlight java %}
   public void goFinish(){
        isLoadWithError =false;
        if (null!=jsCallBack)
            mWebView.removeJavascriptInterface("XXX");
        mWebView.setFocusable(true);
        mWebView.removeAllViews();
        try {
        	mWebView.clearHistory(); //webview没有历史记录，这里会抛出异常
        }catch(Exception e){
            e.printStackTrace();
        }
        mWebView.destroy();

        try {
        	//mWebView为WebView所在类的全局变量名，不可以混淆
            Field fieldWebView = this.getClass().getDeclaredField("mWebView");
            fieldWebView.setAccessible(true);
            WebView webView = (WebView) fieldWebView.get(this);
            webView.removeAllViews();
            webView.destroy();

        }catch (NoSuchFieldException e) {
            e.printStackTrace();

        }catch (IllegalArgumentException e) {
            e.printStackTrace();

        }catch (IllegalAccessException e) {
            e.printStackTrace();

        }catch(Exception e){
            e.printStackTrace();
        }
        this.finish();
    }
   {%endhighlight %} 

   注意：因为用到了反射去清理webview，所以混淆时，这个方法所在的类不能混淆 


- 2.部分手机h5 game屏幕闪烁

	在小米的某些手机上，会出现这种情况。抱歉，还没有好的解决办法。谁有好的解决办法可以邮件告知，多谢 :)


### 2.WebView防止远程代码攻击 

- 1.使用Android4.2以上的系统，通过在Java的远程方法上面声明一个@JavascriptInterface，可以预防改安全漏洞  

- 2.低于Android4.2的系统，如果系统自己添加了一个叫searchBoxJavaBridge_的Js接口，则需要把这个接口删除

详情参见这篇文章：http://blog.csdn.net/leehong2005/article/details/11808557/

这里贴一个我自己写的webview类：https://gist.github.com/agehua/99233b40e05db29ee0ed4f50fb2c7530

