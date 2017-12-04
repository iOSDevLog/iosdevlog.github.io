---
layout: post
title: "RxAndroidBle"
author: iosdevlog
date: 2017-11-24 14:51:38 +0800
description: ""
category: android
tags: [ble, bluetooth]
---

Android Studio 3.0.1
Build #AI-171.4443003, built on November 10, 2017
JRE: 1.8.0_152-release-915-b08 x86_64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
Mac OS X 10.13.1

# import RxAndroidBle & BaseRecyclerViewAdapterHelper

AndroidManifest.xml

```
<uses-permission
    android:name="android.permission.ACCESS_COARSE_LOCATION"
    android:maxSdkVersion="22" />
```

build.gradle(Project: xxx)

```
allprojects {
    repositories {
        ...
        maven { url "https://jitpack.io" }
    }
}
```

build.gradle(Module: app)

```
dependencies {
    ...
    implementation 'com.polidea.rxandroidble:rxandroidble:1.4.3'
    implementation 'com.github.CymChad:BaseRecyclerViewAdapterHelper:2.9.34'
}
```

