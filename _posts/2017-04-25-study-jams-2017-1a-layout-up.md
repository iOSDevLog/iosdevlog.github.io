---
layout: post
title: "Study Jams 2017 1A. 布局-上"
description: ""
category: 
tags: [StudyJams]
---

## XML
---

### 什么是 XML?

<http://www.w3school.com.cn/xml/xml_intro.asp>

* XML 指可扩展标记语言 *（EXtensible Markup Language）*
* XML 是一种标记语言，很类似 HTML
* XML 的设计宗旨是传输数据，而非显示数据
* XML 标签没有被预定义。您需要自行定义标签。
* XML 被设计为具有自我描述性。
* XML 是 W3C 的推荐标准

### Android 开发中使用 XML 

测试网站: <https://v.studyjams.cn>

```xml
<TextView
    android:text="Hi there!"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="36sp"
    android:fontFamily="sans-serif-light"
    android:textColor="@android:color/black"
    android:background="#ccddff"
    android:padding="20dp" />
```

* 标准写法

1. 开头： `<View 名称 属性列表...>`
2. 中间： 子 View
3. 结尾： `</ View>`

如果标准写法 2 没有子 View， 可使用

* 自我封闭式标签

`<View 名称 属性列表... />`

## View
---

* android:layout_width
* android:layout_height
* android:background

> View 的 width 和 height 单位 （dp）
> 颜色使用 android 色号 或者 16进制色号 #fff

### TextView

* android:text
* android:textSize
* android:fontFamil
* android:textColor

> 字体单位 textSize 单位 （sp）

### ImageView

* android:src 
    1. `@` 表示位置引用
    1. drawable/name
    1. 图片放到资源文件中
* android:scaleType

公共方法

```java
static ImageView.ScaleType valueOf(String name)

final static ScaleType[] values()

枚举值

public static final ImageView.ScaleType CENTER

图片位于视图中间，但不执行缩放比例。在XML中，使用语法：android:scaleType="center"

public static final ImageView.ScaleType CENTER_CROP

按比例统一缩放图片（保持图片的尺寸比例）便于图片的两维（宽度和高度）等于或大于相应的视图维度。然后图片居中于视图。在XML中，使用语法：android:scaleType="centerCrop"

public static final ImageView.ScaleType CENTER_INSIDE

按比例统一缩放图片（保持图片的尺寸比例）便于图片的两维（宽度和高度）等于或小于相应的视图维度。然后图片居中于视图。在XML中，使用语法：android:scaleType="centerInside"

public static final ImageView.ScaleType FIT_CENTER

缩放图片使用CENTER。在XML中，使用语法：android:scaleType="fitCenter"

public static final ImageView.ScaleType FIT_END

缩放图片使用END。在XML中，使用语法：android:scaleType="fitEnd"

public static final ImageView.ScaleType FIT_START

缩放图片使用START。在XML中，使用语法：android:scaleType="fitStart"

public static final ImageView.ScaleType FIT_XY

缩放图片使用FILL. 。在XML中，使用语法：android:scaleType="fitXY"

public static final ImageView.ScaleType MATRIX

当绘制时使用图片矩阵缩放。图片矩阵可以使用setImageMatrix(Matrix)进行设定。在XML中，使用语法：android:scaleType="matrix"

公共方法

public static ImageView.ScaleType valueOf (String name)

参数

String name(名字)

返回值

ImageView.ScaleType

public static final ScaleType[] values ()

参数

NULL

返回值

ScaleType[]
```

<http://iosdevlog.com/2016/09/21/android-3.html>

## 参考文档
---

<http://iosdevlog.com/2016/09/18/android-0.html>