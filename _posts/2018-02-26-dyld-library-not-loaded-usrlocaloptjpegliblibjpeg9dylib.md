---
layout: post
title: "dyld: Library not loaded: /usr/local/opt/jpeg/lib/libjpeg.9.dylib"
author: iosdevlog
date: 2018-02-26 17:33:34 +0800
description: ""
category: 
tags: [tennsorflow]
---

在mac系统下，执行谷歌机器学习框架 Tesseract时，报错： dyld: Library not loaded: /usr/local/opt/jpeg/lib/libjpeg.9.dylib

原因是在`/usr/local/opt/jpeg/lib/`路径下找不到 **libjpeg.9.dylib** 文件。

# 解决方法: 

以下命令按顺序在终端进行执行：

```
$ wget -c http://www.ijg.org/files/jpegsrc.v9c.tar.gz
$ tar xzf wpegsrc.v9d.tar.gz
$ cd jpeg-9d
$ ./configure
$ make
$ cp ./.libs/libjpeg.9.dylib /usr/local/opt/jpeg/lib
```

> 参考：<https://github.com/Homebrew/homebrew-php/issues/4358#issuecomment-320645331>
