---
layout: post
title: "Android 6 for Programmers 读书笔记  Welcome"
author: iosdevlog
date: 2017-11-10 11:43:27 +0800
description: ""
category: android
tags: [android6]
---

Android Studio 3.0
Build #AI-171.4408382, built on October 21, 2017
JRE: 1.8.0_152-release-915-b08 x86_64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
Mac OS X 10.12.

![1](/assets/images/Android/Android6/1/1.png)

![2](/assets/images/Android/Android6/1/2.png)

![3](/assets/images/Android/Android6/1/3.png)

![4](/assets/images/Android/Android6/1/4.png)

# 创建新工程

把 gradle-wrapper.properties (Gradle Version) 第7行`https`改为`http`:

```
#Fri Nov 10 14:04:14 CST 2017
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
# https -> http
distributionUrl=http\://services.gradle.org/distributions/gradle-4.1-all.zip
```

activity_main.xml

{% highlight xml linenos %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/welcomeLinearLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.iosdevlog.welcome.MainActivity">

    <TextView
        android:id="@+id/welcomeTextView"
        android:layout_width="wrap_content"
        android:layout_height="0dp"
        android:layout_gravity="center_horizontal"
        android:layout_weight="1"
        android:gravity="center"
        android:text="@string/welcomTextView"
        android:textColor="@color/welcome_text_color"
        android:textSize="@dimen/welcome_textsize" />

    <ImageView
        android:id="@+id/bugImageView"
        android:layout_width="wrap_content"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:contentDescription="@string/iosdevlog_logo"
        android:src="@drawable/bug" />

</LinearLayout>
{% endhighlight %}

* 1 改为 `LineLayout`
* 7  垂直布局
* 15 添加 id
* 18 TextView水平居中
* 17,19,28,29 各占 50%
* 20 多行居中
* 21 文字内容
* 22 文字颜色
* 23 文字大小
* 30 辅助功能 TalkBack

# 国际化

`strings.xml` 右上角 `Open Editor`。

点击`地球仪`，选择`Chinese`。

在`Chinese`输入翻译的语言。
