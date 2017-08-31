---
layout: post
title: "在CentOS上面安装git服务器gogs"
description: ""
category: 
tags: [git]
---

# gogs 官方网站 <http://gogs.io>
---

Gogs 中文说明 

<https://github.com/gogits/gogs/blob/master/README_ZH.md>

找到下面的 [使用 Gogs 搭建自己的 Git 服务器](https://blog.mynook.info/post/host-your-own-git-server-using-gogs/)

按照说明安装

> 如果提示git没有sudo权限
> `git is not in the sudoers file.  This incident will be reported.`
> 先切换到有sudo权限的用户执行，再su回来就可以了。

```bash
$ sudo useradd -d /home/git git # 添加git用户
$ sudo passwd git # 设置git密码
$ su - git # 切换到git帐户
$ mkdir .ssh # 为以后使用ssh
$ wget -c https://dl.gogs.io/0.11.29/linux_amd64.tar.gz # 下载二进制文件
$ tar xzvf linux_amd64.tar.gz # 解压
$ cd gogs/
$ ls  # 查看解压后的文件
gogs  LICENSE  public  README.md  README_ZH.md  scripts  templates
$ mysql -u root -p < scripts/mysql.sql # 建立数据库
$ mysql -u root -p # 初始化数据库
MariaDB [(none)]> create user 'gogs'@'localhost' identified by 'gogs'; # 创建一个新用户 gogs
Query OK, 0 rows affected (0.03 sec)

MariaDB [(none)]> grant all privileges on gogs.* to 'gogs'@'localhost'; # 将数据库 gogs 的所有权限都赋予该用户
Query OK, 0 rows affected (0.01 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit;
Bye

$ sudo cp scripts/systemd/gogs.service /etc/systemd/system/ # 服务脚本
$ systemctl list-unit-files | grep gogs # 检查是否有gogs服务
$ systemctl status gogs.service # 查看gogs状态
● gogs.service - Gogs
   Loaded: loaded (/etc/systemd/system/gogs.service; disabled; vendor preset: disabled)
   Active: inactive (dead)

$ sudo systemctl start gogs.service # 启动gogs服务
```

最后就可以打开3000端口安装了。

http://[ip]:3000/install
