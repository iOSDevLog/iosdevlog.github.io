---
layout: post
title: "drawable or mipmap"
description: ""
category: android
tags: [android, drawable, mipmap]
---

mipmap folders for diffrent DPIs, No more diffrent DPIs drawable folders. 4个不同分辨率的mipmap文件夹，与不同DPI的drawable文件夹没啥区别。 Should I put all my resources in the mipmap folders, or just the app icon? 我应该把所有资源都放到mipmap文件夹么？或者只放应用的图标？ 
        
> 答案：The mipmap folders are for placing your app icons in only. Any other drawable assets you use should be placed in the relevant drawable folders as before.

>  mipmap文件夹只放应用图标。其他需要使用的drawable资源象之前一样放到对应的drawable文件夹。

<https://stackoverflow.com/questions/28065267/mipmap-vs-drawable-folders>
