---
layout: post
title: "Linux(CentOS) 下 is not in the sudoers file 的解决方法"
description: ""
category: 
tags: []
---

使用root帐户执行`visudo`。

```bash
# visudo

## Allow root to run any commands anywhere
root            ALL=(ALL)        ALL
# 添加下面这一行,其它和上面的一样
iosdevlog        ALL=(ALL)        ALL
```

