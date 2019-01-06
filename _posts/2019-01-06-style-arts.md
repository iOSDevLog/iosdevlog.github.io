---
layout: post
title: 深度学习应用：iOS 上的图像风格迁移
author: iosdevlog
date: 2019-01-06 10:46:55 +0800
description: ""
category: 机器学习
tags: []
---

![fast-style-transfer-coreml](https://upload-images.jianshu.io/upload_images/910914-18b9afdf70d90d56.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图像风格迁移，用 `python` 就可以实现，如果想要在手机上面（不联网）查看效果怎么办呢？

如果你是用 iOS 系统，你一定听说过 `Prisma`，它赢得了 2016 年度最佳应用程序，就是这样，它在短短几秒钟内，可以将你的图片转换成你所选择的任何风格。

![Prisma.png](https://upload-images.jianshu.io/upload_images/910914-97ed7ab5bfd7a663.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里我们使用 iOS 11 推出的 `CoreML` 实现 `Prisma` 类似的功能。

`Android`版的见 `tensorflow` 官方提供的例子:<https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/android/>

![TF Stylize](https://upload-images.jianshu.io/upload_images/910914-18df9f617cd04987.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先需要用 `Tensorflow` 训练好模型，之后可以用 Apple 官方提供的转换工具 [coremltools](https://pypi.org/project/coremltools/) 导出成 iOS 11 支持的 CoreML 格式。

新建 `StyleArts` 项目，将导出的 `*.mlmodel` 文件拖入项目。

具体实现细节可以参考我改的代码 <https://github.com/iOSDevLog/StyleArts> 或者 GitHub 上面其它的实现。

![StyleArts.PNG](https://upload-images.jianshu.io/upload_images/910914-05ca7b6b82952441.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

移动端虽然不适合训练机器学习模型，不过可以围魏救赵，通过导出 PC 上面训练好的模型也可以体验人工智能带来的便利。

想体验 [StyleArts AppStore](https://itunes.apple.com/cn/app/stylearts/id1437044305?|mt=8) 效果的可在 App Store 里面搜索 `StyleArts`。

![StyleArtsAppStore.PNG](https://upload-images.jianshu.io/upload_images/910914-7913c5b7dc19288b.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

