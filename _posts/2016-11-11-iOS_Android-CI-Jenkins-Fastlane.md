---
layout: post
title: "Android 持续集成之 Jenkins 持续交付之 Fastlane"
description: "AndroidDevLog"
category: 
tags: [Android, iOS]
---


## gogs

<http://gogs.io/>

Gogs (Go Git Service) 是一款极易搭建的自助 Git 服务。

### Install

下载 *gogs*, 解压后放到 */Application*

```bash
$ /Applications/gogs/gogs web &
```

打开 *gogs* 地址: *localhost:3000*

## fastlane

<https://fastlane.tools/>

![intro-fastlane-tree](https://fastlane.tools/assets/img/intro-fastlane-tree.png)

### Install

```bash
$ gem install fastlane --verbose
```

* [`deliver`](https://github.com/fastlane/fastlane/tree/master/deliver): Upload screenshots, metadata, and your app to the App Store
* [`supply`](https://github.com/fastlane/fastlane/tree/master/supply): Upload your Android app and its metadata to Google Play
* [`snapshot`](https://github.com/fastlane/fastlane/tree/master/snapshot): Automate taking localized screenshots of your iOS app on every device
* [`screengrab`](https://github.com/fastlane/fastlane/tree/master/screengrab): Automate taking localized screenshots of your Android app on every device
* [`frameit`](https://github.com/fastlane/fastlane/tree/master/frameit): Quickly put your screenshots into the right device frames
* [`pem`](https://github.com/fastlane/fastlane/tree/master/pem): Automatically generate and renew your push notification profiles
* [`sigh`](https://github.com/fastlane/fastlane/tree/master/sigh): Because you would rather spend your time building stuff than fighting provisioning
* [`produce`](https://github.com/fastlane/fastlane/tree/master/produce): Create new iOS apps on iTunes Connect and Dev Portal using the command line
* [`cert`](https://github.com/fastlane/fastlane/tree/master/cert): Automatically create and maintain iOS code signing certificates
* [`spaceship`](https://github.com/fastlane/fastlane/tree/master/spaceship): Ruby library to access the Apple Dev Center and iTunes Connect
* [`pilot`](https://github.com/fastlane/fastlane/tree/master/pilot): The best way to manage your TestFlight testers and builds from your terminal
* [`boarding`](https://github.com/fastlane/boarding): The easiest way to invite your TestFlight beta testers
* [`gym`](https://github.com/fastlane/fastlane/tree/master/gym): Building your iOS apps has never been easier
* [`match`](https://github.com/fastlane/fastlane/tree/master/match): Easily sync your certificates and profiles across your team using Git
* [`scan`](https://github.com/fastlane/fastlane/tree/master/scan): The easiest way to run tests for your iOS and Mac apps

### Quick Start

* [Getting started on fastlane for iOS](https://docs.fastlane.tools/getting-started/ios/setup/)
* [Getting started on fastlane for Android](https://docs.fastlane.tools/getting-started/android/setup/)

iOS 会要求输入 苹果开发者帐号 发布到 *App Store* 或者 是 *TestFlight*

## jenkins

<https://jenkins.io/>

Jenkins是一个开源软件项目，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

### Install

下载 *jenkins.war*, 把 *jenkins.war* 放到 */Application*

```bash
$ java -jar /Applications/jenkins.war --httpPort=8000 &
```

打开 *jenkins* 地址: *localhost:8000*

![1](/assets/images/mobile/fastlane/1.png)
![2](/assets/images/mobile/fastlane/2.png)
![3](/assets/images/mobile/fastlane/3.png)
![4](/assets/images/mobile/fastlane/4.png)
![5](/assets/images/mobile/fastlane/5.png)
![6](/assets/images/mobile/fastlane/6.png)
![7](/assets/images/mobile/fastlane/7.png)
![8](/assets/images/mobile/fastlane/8.png)
![9](/assets/images/mobile/fastlane/9.png)
![10](/assets/images/mobile/fastlane/10.png)
![11](/assets/images/mobile/fastlane/11.png)
![12](/assets/images/mobile/fastlane/12.png)
![13](/assets/images/mobile/fastlane/13.png)
![14](/assets/images/mobile/fastlane/14.png)
![15](/assets/images/mobile/fastlane/15.png)
