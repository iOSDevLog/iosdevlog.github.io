---
layout: post
title: "Xcode 设置开发语言为简体中文"
author: iosdevlog
date: 2021-07-01 12:09:21 +0800
description: ""
cover-img: https://github.com/iOSDevLog/LocalizationsDemo/raw/main/Screenshots/Chinese.png
category: 
tags: []
---

## 开发语言

在国际化时，如果大部分用户不是英语，我们就需要设置默认语言。

`Demo.xcodeproj/project.pbxproj`

```diff
                        compatibilityVersion = "Xcode 9.3";
-                       developmentRegion = en;
+                       developmentRegion = zh-Hans;
                        hasScannedForEncodings = 0;
```

![developmentRegion](https://github.com/iOSDevLog/LocalizationsDemo/raw/main/Screenshots/developmentRegion.png)

## App 名称

`InfoPlist.strings`

![InfoPlist.strings](https://github.com/iOSDevLog/LocalizationsDemo/raw/main/Screenshots/InfoPlist.strings.png)

```c
"CFBundleName" = "本地化";
```

## 字符串本地化

`Localizable.strings`

```c
"选好了" = "Done";
```

## 图片本地化

![Assets.xcassets](https://github.com/iOSDevLog/LocalizationsDemo/raw/main/Screenshots/Assets.xcassets.png)


## 演示

![Chinese](https://github.com/iOSDevLog/LocalizationsDemo/raw/main/Screenshots/Chinese.png)
![English](https://github.com/iOSDevLog/LocalizationsDemo/raw/main/Screenshots/English.png)

## GitHub

<https://github.com/iOSDevLog/LocalizationsDemo>
