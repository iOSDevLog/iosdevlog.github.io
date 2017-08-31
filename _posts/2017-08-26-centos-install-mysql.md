---
layout: post
title: "CentOS 和 MacOS 安装 MySQL"
description: ""
category: 
tags: []
---

# MySQL官方网站 <https://www.mysql.com/>

## MacOS
---

安装 MySQL Community Edition (GPL) 版本

### mysql数据库安装和配置

Mac OS X 上安装 MySQL 默认是没有 my.cnf 配置文件的，MySQL 使用默认配置运行。
如果需要对 MySQL 进行定制，
从「/usr/local/mysql/support-files/」目录下的任意一个 .cnf 文件到「/etc/」目录下并重命名为 my.cnf，

```bash
$ sudo cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf
```

然后修改 my.cnf 即可进行定制了。主要是设置编码为utf-8。参照下面CentOS的配置。

如果没有什么特别的需求，默认的配置也使 MySQL 很好的运行了。

mysql服务启动和关闭可以在*系统偏好设置*里面操作

终端命令如下：

```bash
$ sudo /usr/local/mysql/support-files/mysql.server start
$ sudo /usr/local/mysql/support-files/mysql.server stop
```

## CentOS
---

MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可。开发这个分支的原因之一是：甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险，因此社区采用分支的方式来避开这个风险。MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。

```bash
$ sudo yum install mariadb-server mariadb 
```

mariadb数据库的相关命令是：

```bash
$ systemctl start mariadb  #启动MariaDB
$ systemctl stop mariadb  #停止MariaDB
$ systemctl restart mariadb  #重启MariaDB
$ systemctl enable mariadb  #设置开机启动
```

编码设置为 utf-8。

```bash

$ vim /etc/my.cnf
# iosdevlog start
[mysql]
default-character-set =utf8
# iosdevlog end

[mysqld]
# iosdevlog start
character-set-server = utf8
collation-server = utf8_general_ci
# iosdevlog end
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

$ mysql -u root -p # 查看编码
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.52-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```

### 源码安装 Connector/Python (Installing Connector/Python from a Source Distribution)

<https://dev.mysql.com/doc/connector-python/en/connector-python-installation-source.html>

```bash
$ tar xzf mysql-connector-python-VER.tar.gz
$ cd mysql-connector-python-VER
$ sudo python setup.py install
```
