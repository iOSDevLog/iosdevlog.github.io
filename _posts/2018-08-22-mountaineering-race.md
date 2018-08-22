---
layout: post
title: "支付宝登山赛助手"
author: iosdevlog
date: 2018-08-22 11:00:37 +0800
description: ""
category: 
tags: []
---

## WebDriverAgent

登山赛 <https://github.com/iOSDevLog/Mountaineering-Race>

```
$ git clone https://github.com/facebook/WebDriverAgent
$ cd WebDriverAgent
$ brew install carthage
$ ./Scripts/bootstrap.sh
# open WebDriverAgent.xcodeproj with Xcode
# Xcode:
# - code sign (general and build_settings): WebDriverAgentLib/WebDriverAgentRunner
# - Product -> Destination -> <your device> 选择设备
# - Product -> Scheme -> WebDriverAgentRunner 选择方案
# - Product -> Test  `cmd+u` 测试
# - WebDriverAgentRunner -> Bundle Identifie -> "com.facebook.WebDriverAgentRunner.race"
```

* 打开 控制台 `cmd + shift + y` 显示如下内容

```
WebDriverAgentRunner-Runner[3730:734863] ServerURLHere->http://10.0.0.25:8100<-ServerURLHere
```

## libimobiledevice (iproxy)

```
$ brew install libimobiledevice
$ iproxy 8100 8100
# browse: http://localhost:8100/status 状态
{
  "value" : {
    "state" : "success",
    "os" : {
      "name" : "iOS",
      "version" : "11.4.1"
    },
    "ios" : {
      "simulatorVersion" : "11.4.1",
      "ip" : "10.0.0.25"
    },
    "build" : {
      "time" : "Aug 22 2018 10:29:19"
    }
  },
  "sessionId" : "28D5CD8E-50C7-49ED-A6B1-AF1926B15AFF",
  "status" : 0
}
# browse: http://localhost:8100/inspector
```

## auto_race.py

```sh
$ mkvirtualenv race --python=python3
(race) $ pip install Pillow
(race) $ pip install --pre facebook-wda
```

已实现功能：

* 开始
* 再来一局
* 左右移动

TODO：

* 金币位置
* 准确定位

