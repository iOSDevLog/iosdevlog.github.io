---
layout: post
title: "android root"
author: iosdevlog
date: 2018-01-02 23:49:14 +0800
description: ""
category: android
tags: [root]
---

# 真机
---

<https://zh.kingoapp.com>

<https://www.kingoapp.com>

# 模拟器 emulator

Android emulator without "Play"

不带 Android Play 的模拟器

```
$ adb shell
generic_x86:/ $
generic_x86:/ $ exit
$ adb root
$ > adb shell
generic_x86:/ #
```

`adb root` 这条命令就可以root了。

<https://stackoverflow.com/questions/5095234/how-to-get-root-access-on-android-emulator>
