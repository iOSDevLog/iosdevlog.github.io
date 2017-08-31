---
layout: post
title: "linux, MacOS 查看端口占用"
description: ""
category: 
tags: []
---

用 lsof -i tcp:port 就可以查看端口占用情况，如果查看9001端口

```bash
$ lsof -i tcp:9001
COMMAND     PID      USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
python2.7 83946 iosdevlog    5u  IPv4 0x4ad66a83f7ff1e1d      0t0  TCP localhost:etlservicemgr (LISTEN)
```

如果想要停用端口的程序，找到占用端口程序的PID，即进程ID，然后用`kill`命令杀死。

```bash
$ kill -9 83946 # 83946 为上面命令的PID。
```
