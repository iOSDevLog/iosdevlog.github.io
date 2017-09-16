---
layout: post
title: "Windows Subsystem for Linux use agnoster in oh-my-zsh"
description: ""
category: 
tags: []
---

install 安装  [cmder](http://cmder.net/)

install 安装  [Windows Subsystem for Linux](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)

install 安装 [oy-my-zsh](http://ohmyz.sh/)

```bash
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

use *agnoster* Theme 使用 *agnoster* 主题

```bash
$ vim ~/.zshrc
ZSH_THEME="agnoster"
```

change *cmder* font 更改 *cmder* 字体

```bash
$ wget https://raw.githubusercontent.com/powerline/powerline/develop/font/PowerlineSymbols.otf
```

install PowerlineSymbols Font 安装 PowerlineSymbols 字体

right click on *cmder*， choose settings. Change *Main console font* to *PowerlineSymbols*， select *Font charset* to *Mac*.

右键*cmder*, 选择*settgins*。在*Main console font* 选中*PowerlineSymbols*，*Font charset* 选中 *Mac*。

![PowerlineSymbols](/assets/images/windows/linux/PowerlineSymbols.png)

![Settings](/assets/images/windows/linux/Settings.png)
