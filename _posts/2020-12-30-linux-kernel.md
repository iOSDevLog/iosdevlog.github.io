---
layout: post
title: "LinuxLab 真板开发：内核构建"
author: iosdevlog
date: 2020-12-30 17:09:32 +0800
description: ""
cover-img: /assets/images/LinuxLab/IMX6ULL/kernel.png
category: 
tags: [LinuxLab, IMX6ULL, linux, kernel]
---

## 切换到 ebf-imx6ull

```bash
make BOARD=arm/ebf-imx6ull
git branch
# * next
. tools/helper/complete.sh
make [TAB]
```

## 修改内核

进入内核源码目录，做一些修改。

```bash
vi src/linux-stable/init/main.c
```

`kernel_init` 添加添加一条打印。

ubuntu@linux-lab:/labs/linux-lab/src/linux-stable$ `git diff init/main.c`

```diff
diff --git a/init/main.c b/init/main.c
index e083fac08aed..c8041fa17d6c 100644
--- a/init/main.c
+++ b/init/main.c
@@ -1078,6 +1078,8 @@ static int __ref kernel_init(void *unused)
 
 	rcu_end_inkernel_boot();
 
+	printk(KERN_DEBUG "Hello, IMX6ULL from Linux Lab.");
+
 	if (ramdisk_execute_command) {
 		ret = run_init_process(ramdisk_execute_command);
 		if (!ret)
```

### 编译安装

```bash
$ make kernel-build
$ make modules-install
```

### 上传

```bash
make kernel-upload
```

### 启动新 Image

```bash
make boot
```

### 查看启动日志

```bash
debian@npi:~$ dmesg | grep from                                                
[    0.000000] random: get_random_bytes called from start_kernel+0xa0/0x434 wi0
[    0.000000] rcu:     RCU restricting CPUs from NR_CPUS=4 to nr_cpu_ids=1.   
[    1.543896] sii902x bound to mxs-lcdif from 21c8000.lcdif                   
[    1.646126] Console IMX rounded baud rate from 114943 to 114900             
[    7.438844] Hello, IMX6ULL from Linux Lab.                                  
[    9.745109] fec 20b4000.ethernet eth2: renamed from eth0                    
[   15.209116] systemd-journald[206]: Received request to flush runtime journa1
```

可以看到我们在 `init/main.c` 里添加的打印已经生效了。

![kernel](/assets/images/LinuxLab/IMX6ULL/kernel.png)

### 查看一下内核信息：

```bash
debian@npi:~$ uname -a                                                         
Linux npi 4.19.35+ #2 SMP PREEMPT Wed Dec 30 17:33:39 CST 2020 armv7l GNU/Linux
```

是刚才编译的，说明我们修改的内核确实起作用了。

## 调试 Linux 内核

### 虚拟开发板内核自动化调试

```bash
make feature feature=debug
make kernel-olddefconfig
make kernel
make kernel-debug
```

使用 WebVNC， 会自动打开两个窗口，一个显示原有内核，如果使用 bash，需要打开 2 个终端，一个做服务端，一个为客户端，详情请查阅文档。

默认设置了一些断点，可以通过以下命令查看：

```bash
cat ./gdb/kernel.default
```

自己随便玩。

* r: run 运行到断点
* n: next 到下一条语句
* s: step 单步执行
* b: breakpoint 下断点
* l: list 查看源码

### 真板启动调试

```bash
$ make kernel-debug
LOG: This feature is not implemented for real boards.
```

> 真板还没加，这个不太好弄，可以试试 kgdb over serial。

我还是先熟悉虚拟开发板的内核调试吧。