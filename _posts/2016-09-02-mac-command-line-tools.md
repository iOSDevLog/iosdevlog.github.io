---
layout: post
title: "Mac OS 下的基本命令行工具安装"
description: "Android Studio"
category: 
tags: [Tools]
---


![tools](/assets/images/Tools/tools.png)

## HomeBrew

<http://brew.sh/index_zh-cn.html>

OS X 不可或缺的套件管理器

获取 Homebrew

{% highlight ruby %}
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

> homebrew要安装的文件默认是下载到/Library/Caches/Homebrew目录里面

## Ruby

RVM 是一个命令行工具，可以提供一个便捷的多版本 Ruby 环境的管理和切换。

<https://rvm.io/>

如果你打算学习 Ruby / Rails, RVM 是必不可少的工具之一。

这里所有的命令都是再用户权限下操作的，任何命令最好都不要用 **sudo**.

#### RVM 安装

<https://ruby-china.org/wiki/rvm-guide>

{% highlight ruby %}
$ brew install gpg
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ curl -sSL https://get.rvm.io | bash -s stable
$ source ~/.bashrc
$ source ~/.bash_profile
{% endhighlight %}

* 修改 RVM 的 Ruby 安装源到 Ruby China 的 Ruby 镜像服务器，这样能提高安装速度

{% highlight ruby %}
$ echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db
{% endhighlight %}

#### Ruby 的安装与切换

* 列出已知的 Ruby 版本

{% highlight ruby %}
$ rvm list known
{% endhighlight %}

* 安装一个 Ruby 版本

{% highlight ruby %}
$ rvm install 2.3.0 --disable-binary
{% endhighlight %}

> 这里安装了最新的 2.3.0, rvm list known 列表里面的都可以拿来安装。

* 切换 Ruby 版本

{% highlight ruby %}
$ rvm use 2.3.0
{% endhighlight %}

> 如果想设置为默认版本，这样一来以后新打开的控制台默认的 Ruby 就是这个版本

{% highlight ruby %}
$ rvm use 2.3.0 --default
{% endhighlight %}

* 查询已经安装的ruby

{% highlight ruby %}
$ rvm list

rvm rubies

ruby-2.2.0 [ x86_64 ]
ruby-2.2.2 [ x86_64 ]
ruby-2.2.3 [ x86_64 ]
=* ruby-2.3.0 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
{% endhighlight %}

#### gems

请尽可能用比较新的 RubyGems 版本，建议 2.6.x 以上。

{% highlight ruby %}
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://gems.ruby-china.org/
$ gem update --system
$ gem -v
2.6.6
{% endhighlight %}

> 确保只有 gems.ruby-china.org

## cocoapods

<https://guides.cocoapods.org>

{% highlight ruby %}
$ gem install cocoapods
{% endhighlight %}

#### 使用CocoaPods时遇到pod setup失败

<http://www.cocoachina.com/bbs/read.php?tid=193398>

{% highlight bash %}
$ cd ~/.cocoapods/repos/
$ git clone https://github.com/CocoaPods/Specs
$ mv Specs master
$ pod setup
{% endhighlight %}

> 解释：pod setup的本质就是将https://github.com/CocoaPods/Specs上的Specs项目clone到/Users/用户名/.cocoapods/repos目录下。若此目录下已有Specs项目，则会将项目更新到最新的状态。由于Specs很大，容易导致pod setup失败。这时就需要我们手动安装Specs。若直接从github上下载zip文件，由于缺少git文件，会导致cocoa pods不使用。

## iTerm2

<http://www.iterm2.com>

![iTerm2](http://www.iterm2.com/img/logo.jpg)

[Download](https://iterm2.com/downloads/stable/iTerm2-3_0_4.zip)

## Oh My ZSH!

<http://ohmyz.sh>

Your terminal never felt this good before.

Via curl

{% highlight bash %}
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
{% endhighlight %}

Via wget

{% highlight bash %}
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
{% endhighlight %}

## jekyll

<http://jekyllrb.com>

{% highlight bash %}
$ gem install jekyll bundler
$ gem install jekyll-sitemaplo
$ rake post title="Mac command line tools"
$ jekyll serve
{% endhighlight %}

> 我把 [iosdevlog.com](iosdevlog.com) clone 下载后，新建些本次 *post*。

## 命令行先介绍到这里，下一期介绍一些界面工具。
