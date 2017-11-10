---
layout: post
title: "TipCalculator"
author: iosdevlog
date: 2017-11-10 16:43:27 +0800
description: ""
category: android
tags: [android6]
---

![1](/assets/images/Android/Android6/2/1.png)

# GridLayout

2 列

```
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:columnCount="2"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:useDefaultMargins="true"
    tools:context="com.iosdevlog.tipcalculator.MainActivity">

    <EditText
        android:id="@+id/amountEditText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_column="0"
        android:layout_columnSpan="2"
        android:layout_gravity="fill_horizontal"
        android:layout_row="0"
        android:digits="0123456789"
        android:ems="10"
        android:inputType="number"
        android:maxLength="6" />

    <TextView
        android:id="@+id/amountTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_column="0"
        android:layout_columnSpan="2"
        android:layout_gravity="fill_horizontal"
        android:layout_row="0"
        android:background="@color/amount_background"
        android:elevation="@dimen/elevation"
        android:hint="@string/enter_amount"
        android:padding="@dimen/textview_padding"
        android:textAppearance="?android:attr/textAppearanceMedium" />

    <TextView
        android:id="@+id/percentTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_column="0"
        android:layout_gravity="center_horizontal|right"
        android:layout_row="1"
        android:text="@string/tip_percentage"
        android:textAppearance="?android:attr/textAppearanceMedium" />

    <SeekBar
        android:id="@+id/percentSeekBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_column="1"
        android:layout_gravity="fill_horizontal"
        android:layout_row="1"
        android:indeterminate="false"
        android:max="30"
        android:progress="15" />

    <TextView
        android:id="@+id/tipLabelTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:text="@string/tip"
        android:textAppearance="?android:attr/textAppearanceMedium" />

    <TextView
        android:id="@+id/tipTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="fill_horizontal"
        android:background="@color/result_background"
        android:elevation="@dimen/elevation"
        android:gravity="center"
        android:padding="@dimen/textview_padding"
        android:textAppearance="?android:attr/textAppearanceMedium" />

    <TextView
        android:id="@+id/totalLabelTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:text="@string/total"
        android:textAppearance="?android:attr/textAppearanceMedium" />

    <TextView
        android:id="@+id/totalTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="fill_horizontal"
        android:background="@color/result_background"
        android:elevation="@dimen/elevation"
        android:gravity="center"
        android:padding="@dimen/textview_padding"
        android:textAppearance="?android:attr/textAppearanceMedium" />
</GridLayout>

```

# Manifast.xml

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.iosdevlog.tipcalculator">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:screenOrientation="portrait"
            android:windowSoftInputMode="stateAlwaysVisible">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

`android:screenOrientation="portrait"` 竖屏

`android:windowSoftInputMode="stateAlwaysVisible"` 进入应用显示软键盘