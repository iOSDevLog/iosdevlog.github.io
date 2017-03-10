---
layout: post
title: "Fragments and UI Modularization"
description: "AndroidDevLog"
category: 
tags: [AndroidDevLog]
---


# Creating Dynamic UIs with Android Fragments - Second Edition

[Jim Wilson](http://blog.jwhh.com)

March 2016

<https://www.packtpub.com/application-development/creating-dynamic-uis-android-fragments-second-edition>

---

![1](/assets/images/Android/push/fragment/1/1.png)

### The old thinking – activity-oriented 旧思维 - 活动导向
---

This layout resource is reasonably simple and is explained as follows:

这种布局资源相当简单，解释如下：

• The overall layout is defined within a vertically-oriented LinearLayout element containing two ScrollView elements
• 整体布局在包含两个ScrollView元素的垂直方向的LinearLayout元素中定义
• Both of the ScrollView elements have a layout_weight value of 1 that causes the top-level LinearLayout element to divide the screen equally between the two ScrollView elements
• 两个ScrollView元素的layout_weight值均为1，导致顶层LinearLayout元素在两个ScrollView元素之间平均划分屏幕
• The top ScrollView element with the id value of scrollTitles wraps a RadioGroup element containing a series of the RadioButton elements, one for each book
• 具有scrollTitles的id值的顶部ScrollView元素包装一个RadioGroup元素，该元素包含一系列RadioButton元素，每个书籍一个
• The bottom ScrollView element with the id value of scrollDescription contains a TextView element that displays the selected book's description
• 底部ScrollView元素的id值为scrollDescription，它包含一个TextView元素，用于显示所选书的描述

Displaying the activity UI

The application's activity class is MainActivity. To display the activity's user interface, we will override the onCreate method and call the setContentView method, passing the R.layout.activity_main layout resource ID via the following code:

```java
protected void onCreate(Bundle savedInstanceState) {
     super.onCreate(savedInstanceState);
     // load the activity_main layout resource
     setContentView(R.layout.activity_main);
}
```

### The new thinking: fragment-oriented 新思维 - 碎片导向
---

#### Creating the fragment layout resources 创建片段布局资源

#### Defining the layout as a reusable list 将布局定义为可重用的列表

To create a fragment for the book list, we will define a new layout resource file called *fragment_book_list.xml*.

要为图书列表创建一个片段，我们将定义一个名为*fragment_book_list.xml*的新布局资源文件。

We will copy the top ScrollView element and its contents from the *activity_main.xml* resource file to the *fragment_book_list.xml* resource  file.

我们将顶层的ScrollView元素及其内容从*activity_main.xml*资源文件复制到*fragment_book_list.xml*资源文件。

The resulting *fragment_book_list.xml* resource file is as follows:

生成的*fragment_book_list.xml*资源文件如下：

```xml
 <!-- List of Book Titles -->
 <ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/scrollTitles">

    <RadioGroup
        android:id="@+id/bookSelectGroup"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        >

        <RadioButton
            android:id="@+id/dynamicUiBook"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="@string/dynamicUITitle"
            android:checked="true"
            />

        <RadioButton
            android:id="@+id/android4NewBook"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="@string/android4NewTitle"
            />

        <RadioButton
            android:id="@+id/androidSysDevBook"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="@string/androidSysDevTitle"
            />

        <RadioButton
            android:id="@+id/androidEngineBook"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="@string/androidEngineTitle"
            />

        <RadioButton
            android:id="@+id/androidDbProgBook"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="@string/androidDbProgTitle"
            />
    </RadioGroup>
</ScrollView>
```

## Summary 总结
---

The shift from the old thinking of being activity-oriented to the new thinking of being fragment-oriented opens up our applications to rich possibilities. 

从以活动为导向的旧思维向以碎片为导向的新思维的转变打开了我们的应用到丰富的可能性。

Fragments allow us to better organize both the appearance of the user interface and the code we use to manage it.

片段允许我们更好地组织用户界面的外观和我们用来管理它的代码。

 With fragments, our application user interface has a more modular approach that frees us from being tied to the specic capabilities of a small set of devices and prepares us to work with the rich devices of today and the wide variety of new devices to come tomorrow.

有了碎片，我们的应用程序用户界面采用了更加模块化的方法，使我们不必受限于一小组设备的特定功能，并准备我们与当今丰富的设备和各种新设备一起工作。

In the next chapter, we'll build on the modularized user interface we created with fragments to enable our application to automatically adapt to the differences in various device form factors with only minimal changes to our application.

在下一章中，我们将基于我们使用片段创建的模块化用户界面，使我们的应用程序能够自动适应各种设备形式因素的差异，只需对应用程序进行最小的更改。