---
layout: post
title: "CentOS python 虚拟环境 virtualenvwrapper"
description: ""
category: 
tags: [python]
---

## CentOS 安装 pip

```bash
$ sudo yum install python-pip
$ sudo pip install --upgrade pip
```

## 虚拟环境

<http://virtualenvwrapper.readthedocs.io/en/latest/install.html>

```bash
$ sudo pip install virtualenvwrapper
$ vim .bashrc
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Project
source /usr/bin/virtualenvwrapper.sh
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'

$ mkvirtualenv venv # venv 是新建虚拟环境的名称
$ mkvirtualenv venv --python=python2.7 # 创建python2.7的虚拟环境
$ workon venv # 进入虚拟环境
$ deactivate # 退出虚拟环境
```
