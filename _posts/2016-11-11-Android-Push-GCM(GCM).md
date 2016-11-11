---
layout: post
title: "Android 之推送 FCM(GCM)"
description: "AndroidDevLog"
category: 
tags: [AndroidDevLog]
---
{% include JB/setup %}

## 源码 

<https://github.com/AndroidDevLog/AndroidDevLog/tree/master/233.%20Push>

## 文档

<https://firebase.google.com>

<https://firebase.google.com/docs/cloud-messaging/android/client?hl=zh-cn>

<http://www.codeproject.com/Articles/1121218/Android-Firebase-Cloud-Messaging-Tutorial>

## 测试

<https://console.firebase.google.com/project/push-cbf8f/notification>

<http://apns-gcm.bryantan.info>

## 先决条件

* 一台运行 Android 2.3 (Gingerbread) 或更新版本并运行 Google Play 服务 9.6.1 或更新版本的设备。
* Android SDK 管理器 中的 Google Play 服务 SDK
* Android Studio 1.5 或更高版本
* Android Studio 项目及其捆绑包名称。

## FCM

CREATE NEW PROJECT / 新建项目

![1](/assets/images/Android/AndroidDevLog/push/1.png)

![2](/assets/images/Android/AndroidDevLog/push/2.png)

![3](/assets/images/Android/AndroidDevLog/push/3.png)

![4](/assets/images/Android/AndroidDevLog/push/4.png)

![5](/assets/images/Android/AndroidDevLog/push/5.png)

## 将 Firebase 添加至您的应用

现在，可以将 Firebase 添加至您的应用了。为此，您的应用需要有一个 Firebase 项目和一个 Firebase 配置文件。

1. 如果您还没有 Firebase 项目，请在 Firebase console 中创建一个。 如果已经有一个与您的移动应用关联的现有 Google 项目，请点击 Import Google Project。 否则，请点击 Create New Project。
2. 点击 Add Firebase to your Android app 并按设置步骤进行操作。如果在导入现有 Google 项目，这可能是自动进行的，您只需下载配置文件即可。
3. 出现提示时，输入您的应用的包名称。输入您应用使用的包名称十分重要。只有当您将一个应用添加至您的 Firebase 项目时才能进行此设置。
4. 最后，您将下载一个 google-services.json 文件。您可以随时重新下载此文件。
5. 通常，如果尚未下载，请将此复制到您的项目模块文件夹，通常为 app/。

> 注：如果您有多个构建变体含有已定义的不同包名称，则必须在 Firebase console 中将每个应用添加到您的项目。

## 添加 SDK

如果您想将 Firebase 内容库集成到您的一个项目中，则需为准备 Android Studio 项目执行几项基本任务。您可能在向应用添加 Firebase 的过程中已经完成此步操作。

首先，请向您的根级 build.gradle 文件添加一条规则，以包含 Google 服务插件：

```gradle
buildscript {
    // ...
    dependencies {
        // ...
        classpath 'com.google.gms:google-services:3.0.0'
    }
}
```

然后在您的模块 Gradle 文件（通常为 app/build.gradle）中，在文件底部添加 apply plugin 行，以启用 Gradle 插件：

```
apply plugin: 'com.android.application'

android {
  // ...
}

dependencies {
  // ...
  compile 'com.google.firebase:firebase-core:9.6.1'
}

// ADD THIS AT THE BOTTOM
apply plugin: 'com.google.gms.google-services'
```
