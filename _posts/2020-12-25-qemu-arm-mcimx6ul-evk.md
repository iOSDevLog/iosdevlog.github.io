---
layout: post
title: "LinuxLab 真板开发：编译 qemu-arm-mcimx6ul-evk"
author: iosdevlog
date: 2020-12-25 18:53:14 +0800
description: ""
cover-img: /assets/images/LinuxLab/IMX6ULL/error.png
category: 
tags: [LinuxLab, IMX6ULL]
---

## Ubuntu 18.04 VirtualBox 

### 运行 qemu-arm-mcimx6ul-evk

```bash
git clone https://gitee.com/tinylab/qemu-arm-mcimx6ul-evk
cd qemu-arm-mcimx6ul-evk/
sudo apt install make gcc-arm-linux-gnueabihf gcc bison flex libssl-dev dpkg-dev lzop
sudo apt install libsdl2-image-dev -y
sudo vim /etc/apt/sources.list
# deb http://cz.archive.ubuntu.com/ubuntu xenial main
sudo apt update
sudo apt install libpng12-0 -y
```

./boot.sh

```bash
linux-lab login: root
[   36.390412] usb_otg1_vbus: disabling
# uname -a
Linux linux-lab 5.4.0-dirty #1 SMP Wed Apr 8 23:55:27 CST 2020 armv7l GNU/Linux
# poweroff
```

### 编译内核 

[https://github.com/Embedfire/ebf_linux_kernel](https://github.com/Embedfire/ebf_linux_kernel)

## LinuxLab Ubuntu 20.04 (macOS Big Sur 11.1)

[泰晓科技/Linux Lab](https://gitee.com/tinylab/linux-lab)

```bash
sudo useradd --create-home --shell /bin/bash --user-group --groups adm,sudo iosdevlog
sudo passwd iosdevlog
sudo -su iosdevlog
whoami
# iosdevlog
```

### 下载 LinuxLab

```bash
git clone https://gitee.com/tinylab/cloud-lab.git
cd cloud-lab/ && tools/docker/choose linux-lab
```

### 运行 LinuxLab

```bash
tools/docker/run linux-lab
tools/docker/bash
```

### IMX6ULL + 野火 Patch

```bash
make list | grep imx6ul
# [ arm/mcimx6ul-evk ]:
make BOARD=mcimx6ul-evk
make boot
```

### LinuxLab 开发 Linux (真板还不支持以下操作)

```bash
make kernel-download
make kernel-checkout
make kernel-patch
make kernel-defconfig
make kernel-menuconfig
make kernel
make boot
```

### Linux Kernel 手动编译

链接: [https://pan.baidu.com/s/1AZHdBMnB63zLbZjbsa73xg](https://pan.baidu.com/s/1AZHdBMnB63zLbZjbsa73xg)  密码: csnu

```bash
# 下载 Linux 源码
make kernel-download
cd /labs/linux-lab/src/patch/linux
# 解压patch补丁
tar xzvf ebf_6ull_patch.v4.19.35.tar.gz
ls v4.19.35 | wc
#    5194    5194  321387
# 进入 linux stable 内核目录
cd /labs/linux-lab/src/linux-stable
# 检出 linux stable 4.19.35 标签
git checkout v4.19.35
# 打补丁
git am /labs/linux-lab/src/patch/linux/v4.19.35/*.patch
# Applying: MLK-11265-10 ARM: configs: add imx v7 support
# Applying: MLK-11265-1 ARM: dts: add imx7d soc dtsi support
# ...
# 基本 1 秒 1 个补丁，可能要等 1 个多小时
```

![patch.png](/assets/images/LinuxLab/IMX6ULL/patch.png)

2 个 多小时后终于好了。

![PatchEnd.png](/assets/images/LinuxLab/IMX6ULL/PatchEnd.png)

添加 `sdma-imx6q.bin`

```bash
cp sdma-imx6q.bin firmware/sdma-imx6q.bin
git add -f .
git commit -m "sdma-imx6q.bin"
```

![sdma-imx6q.bin](/assets/images/LinuxLab/IMX6ULL/sdma-imx6q.bin.png)

编译内核前建议先安装以下工具

```bash
sudo apt install make gcc-arm-linux-gnueabihf gcc bison flex libssl-dev dpkg-dev lzop
# 报错，编辑 /etc/apt/sources.list
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vi /etc/apt/sources.list
```

ubuntu 20.04 官方源

```bash
# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://archive.ubuntu.com/ubuntu focal main restricted
# deb-src http://archive.ubuntu.com/ubuntu focal main restricted

## Major bug fix updates produced after the final release of the
## distribution.
deb http://archive.ubuntu.com/ubuntu focal-updates main restricted
# deb-src http://archive.ubuntu.com/ubuntu focal-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team. Also, please note that software in universe WILL NOT receive any
## review or updates from the Ubuntu security team.
deb http://archive.ubuntu.com/ubuntu focal universe
# deb-src http://archive.ubuntu.com/ubuntu focal universe
deb http://archive.ubuntu.com/ubuntu focal-updates universe
# deb-src http://archive.ubuntu.com/ubuntu focal-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://archive.ubuntu.com/ubuntu focal multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal multiverse
deb http://archive.ubuntu.com/ubuntu focal-updates multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal-updates multiverse

## N.B. software from this repository may not have been tested as
## extensively as that contained in the main release, although it includes
## newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review
## or updates from the Ubuntu security team.
deb http://archive.ubuntu.com/ubuntu focal-backports main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal-backports main restricted universe multiverse

## Uncomment the following two lines to add software from Canonical's
## 'partner' repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
# deb http://archive.canonical.com/ubuntu focal partner
# deb-src http://archive.canonical.com/ubuntu focal partner

deb http://archive.ubuntu.com/ubuntu focal-security main restricted
# deb-src http://archive.ubuntu.com/ubuntu focal-security main restricted
deb http://archive.ubuntu.com/ubuntu focal-security universe
# deb-src http://archive.ubuntu.com/ubuntu focal-security universe
deb http://archive.ubuntu.com/ubuntu focal-security multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal-security multiverse
```

更新源

```bash
sudo apt update
sudo apt --fix-broken install
sudo apt install make gcc-arm-linux-gnueabihf gcc bison flex libssl-dev dpkg-dev lzop
```

arm-linux-gnueabihf-gcc 9.3.0

```bash
$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc-cross/arm-linux-gnueabihf/9/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: ../src/configure -v --with-pkgversion='Ubuntu 9.3.0-17ubuntu1~20.04' --with-bugurl=file:///usr/share/doc/gcc-9/README.Bugs --enable-languages=c,ada,c++,go,d,fortran,objc,obj-c++,gm2 --prefix=/usr --with-gcc-major-version-only --program-suffix=-9 --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --libdir=/usr/lib --enable-nls --with-sysroot=/ --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-libitm --disable-libquadmath --disable-libquadmath-support --enable-plugin --enable-default-pie --with-system-zlib --without-target-system-zlib --enable-libpth-m2 --enable-multiarch --enable-multilib --disable-sjlj-exceptions --with-arch=armv7-a --with-fpu=vfpv3-d16 --with-float=hard --with-mode=thumb --disable-werror --enable-multilib --enable-checking=release --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=arm-linux-gnueabihf --program-prefix=arm-linux-gnueabihf- --includedir=/usr/arm-linux-gnueabihf/include
Thread model: posix
gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)
```

一键编译内核 deb 包

```bash
./make_deb.sh
...
make[6]: *** [/labs/linux-lab/src/linux-stable/Makefile:1052: drivers] Error 2
make[5]: *** [Makefile:146: sub-make] Error 2
make[4]: *** [Makefile:24: __sub-make] Error 2
make[3]: *** [debian/rules:4: build] Error 2
dpkg-buildpackage: error: debian/rules build subprocess returned exit status 2
make[2]: *** [/labs/linux-lab/src/linux-stable/scripts/package/Makefile:80: bindeb-pkg] Error 2
make[1]: *** [/labs/linux-lab/src/linux-stable/Makefile:1365: bindeb-pkg] Error 2
make[1]: Leaving directory '/labs/linux-lab/src/linux-stable/build_image/build'
make: *** [Makefile:146: sub-make] Error 2
```

失败了，明天再看。

![error.png](/assets/images/LinuxLab/IMX6ULL/error.png)

## 升级 deb

[4. 构建野火Debian系统固件 - [野火]i.MX Linux开发实战指南 文档](http://doc.embedfire.com/linux/imx6/base/zh/latest/building_image/building_debian.html)

[update-fire-kernel.sh](http://update-fire-kernel.sh/)

```bash
#!/bin/sh -e

_do () {
         $@ || ( cp -rf /tmp/boot /; echo "kernel update failed: $@"; exit -1; )
}

if [ ! -f /boot/vmlinuz* ]; then
        echo "error:fire kernel no exit!"
else
        cp -rf /boot /tmp

   _do dpkg -r linux-image-$(uname -r)

        _do dpkg -i $1

        if [ -f /boot/vmlinuz* ]; then
                rm -rf /tmp/boot
        else
                cp -rf /tmp/boot /
        fi
fi
```

执行升级操作

```bash
chmod +x ./update-fire-kernel.sh
sudo ./update-fire-kernel.sh ./linux-image-4.19.71-imx-r1_1stable_armhf.deb
```
