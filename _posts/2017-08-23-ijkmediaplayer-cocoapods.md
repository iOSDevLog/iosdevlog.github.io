---
layout: post
title: "上传ijkplayer到cocoapods"
description: ""
category: ijkplayer
tags: [cocoapods]
---

# <https://github.com/iOSDevLog/ijkplayer>
---

编译 [Bilibili/ijkplayer](https://github.com/Bilibili/ijkplayer)

```bash
git clone https://github.com/Bilibili/ijkplayer.git
cd ijkplayer
git checkout -B latest k0.8.2

./init-ios.sh

cd ios
./compile-ffmpeg.sh clean
./compile-ffmpeg.sh all

open IJKMediaPlayer/IJKMediaPlayer.xcodeproj
```

## 生成通用 Framework

![1](/assets/images/iOS/cocoapods/ijkplayer/1.png)

![2](/assets/images/iOS/cocoapods/ijkplayer/2.png)

![3](/assets/images/iOS/cocoapods/ijkplayer/3.png)

![4](/assets/images/iOS/cocoapods/ijkplayer/4.png)

![5](/assets/images/iOS/cocoapods/ijkplayer/5.png)

### 添加脚本

```bash
# Sets the target folders and the final framework product.
# 如果工程名称和Framework的Target名称不一样的话，要自定义FRAMEWORK_NAME
# FRAMEWORK_NAME=${PROJECT_NAME}
FRAMEWORK_NAME=IJKMediaFramework
# Install dir will be the final output to the framework.
# The following line create it in the root folder of the current project.
INSTALL_DIR=${SRCROOT}/Products/${FRAMEWORK_NAME}.framework
# Working dir will be deleted after the framework creation.
WORKING_DIR=build
DEVICE_DIR=${WORKING_DIR}/Release-iphoneos/${FRAMEWORK_NAME}.framework
SIMULATOR_DIR=${WORKING_DIR}/Release-iphonesimulator/${FRAMEWORK_NAME}.framework
# -configuration ${CONFIGURATION}
# Clean and Building both architectures.
xcodebuild -configuration "Release" -target "${FRAMEWORK_NAME}" -sdk iphoneos clean build
xcodebuild -configuration "Release" -target "${FRAMEWORK_NAME}" -sdk iphonesimulator clean build
# Cleaning the oldest.
if [ -d "${INSTALL_DIR}" ]
then
    rm -rf "${INSTALL_DIR}"
fi
mkdir -p "${INSTALL_DIR}"
cp -R "${DEVICE_DIR}/" "${INSTALL_DIR}/"
# Uses the Lipo Tool to merge both binary files ([arm_v7] [i386] [x86_64] [arm64]) into one Universal final product.
lipo -create "${DEVICE_DIR}/${FRAMEWORK_NAME}" "${SIMULATOR_DIR}/${FRAMEWORK_NAME}" -output "${INSTALL_DIR}/${FRAMEWORK_NAME}"
rm -r "${WORKING_DIR}"
open "${INSTALL_DIR}"
```

## 将Framework提交Cocoapods

### 在github.com上面创建新仓库

![6](/assets/images/iOS/cocoapods/ijkplayer/6.png)

### 把IJKMediaFramework.framework添加到仓库

```bash
$ git lfs track IJKMediaFramework.framework/IJKMediaFramework
$ git add IJKMediaFramework.framework/IJKMediaFramework
$ git add -A
$ git commit
$ git push
```

### 创建 ijkplayer.podspec 

<https://guides.cocoapods.org/making/making-a-cocoapod.html>

```bash
$ pod spec create ijkplayer
$ cat ijkplayer.podspec
```

```ruby
Pod::Spec.new do |s|
  s.name         = "ijkplayer"
  s.version      = "1.0.0"
  s.summary      = "ijkplayer framework."

  s.description  = <<-DESC
bilibili/ijkplayer  IJKMediaFramework 上传到 cococapods
                   DESC

  s.homepage     = "https://github.com/iOSDevLog/ijkplayer"

  s.license      = { :type => "GNU Lesser General Public License v2.1", :text => <<-LICENSE
		   GNU LESSER GENERAL PUBLIC LICENSE
		   Version 2.1, February 1999
		   https://github.com/iOSDevLog/ijkplayer/raw/master/LICENSE
                 LICENSE
               }

  s.author             = { "iosdevlog" => "iosdevlog@iosdevlog.com" }
  s.social_media_url   = "http://weibo.com/iOSDevLog"

  s.platform     = :ios, "7.0"

  s.source       = { :http => "https://raw.githubusercontent.com/iOSDevLog/ijkplayer/master/IJKMediaFramework.framework.zip" }

  s.ios.vendored_frameworks = 'IJKMediaFramework.framework'
  s.frameworks  = "AudioToolbox", "AVFoundation", "CoreGraphics", "CoreMedia", "CoreVideo", "MobileCoreServices", "OpenGLES", "QuartzCore", "VideoToolbox", "Foundation", "UIKit", "MediaPlayer"
  s.libraries   = "bz2", "z", "stdc++"

  s.requires_arc = true

end
```

### 去除Swift警告

```bash
$ echo "2.3" > .swift-version
$ git push
```

### 验证编写的podspec文件

```bash
$ pod lib lint
$ pod spec lint ijkplayer.podspec
...
ijkplayer.podspec passed validation.
```

## 使用Trunk服务提交到Cocoapod官方在Github的specs

### 注册Trunk服务

```bash
$ pod trunk register iosdevlog@iosdevlog.com 'iosdevlog' --verbose
$ pod trunk me
  - Name:     iosdevlog
  - Email:    iosdevlog@iosdevlog.com
  - Since:    August 23rd, 00:48
  - Pods:     None
  - Sessions:
  - August 23rd, 00:48 - December 29th, 00:50. IP: 119.137.52.190
$ pod trunk push ijkplayer.podspec
```
