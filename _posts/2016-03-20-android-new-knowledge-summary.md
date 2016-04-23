---
layout: post
title:  android新特性新知识点总结
category: read
tags: 读书
keywords: 新特性, 新知识点,总结
---


* 目录
{:toc}

### mipmap 目录和drawable 目录有什么区别
Nexus 6 有 493 ppi，它刚好在 xxhdpi和xxxhdpi之间，所以显示的时候需要对xxxhdpi的资源进行缩小，如果你用了mipmap-xxxhdpi,那么这里会对sclae有一个优化，性能更好，占用内存更少。所以现在官方推荐使用mipmap：

### setTranslucentStatus()方法
在Android4.4之后使用沉浸式状态栏，需要用到这个方法
{%highlight java%}
public class MainActivity extends Activity
{
    @Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //首先检测当前的版本是否是api>=19的
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT)
        {
            setTranslucentStatus(true);
        }

        SystemBarTintManager tintManager = new SystemBarTintManager(this);
        tintManager.setStatusBarTintEnabled(true);
        tintManager.setStatusBarTintColor(Color.parseColor("#FFC1E0"));
    }

    @TargetApi(19)
    private void setTranslucentStatus(boolean on)
    {
        Window win = getWindow();
        WindowManager.LayoutParams winParams = win.getAttributes();
        final int bits = WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS;
        if (on)
        {
            winParams.flags |= bits;
        }
        else
        {
            winParams.flags &= ~bits;
        }
        win.setAttributes(winParams);
    }
}
{%endhighlight %}

布局设置
{%highlight javascript%}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              <!--这两行是必须设置的-->
              android:fitsSystemWindows="true"
              android:clipToPadding="true"

              android:orientation="vertical"
              android:background="#FFD9EC"
        >

    <TextView
            android:text="沉浸式状态栏"
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:textSize="23dp"
            android:layout_gravity="center_horizontal"
            android:gravity="center"
            android:background="#FFD9EC"
            />
    <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="@android:color/darker_gray"/>

</LinearLayout>
{%endhighlight %}