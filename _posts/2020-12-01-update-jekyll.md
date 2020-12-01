---
layout: post
title: "update jekyll"
author: iosdevlog
date: 2020-12-01 15:32:13 +0800
description: ""
category: github
tags: [jekyll]
---

## 升级 Jekyll

<https://stackoverflow.com/questions/53135863/macos-mojave-ruby-config-h-file-not-found>

<https://github.com/orta/cocoapods-keys/issues/198#issuecomment-510909030>

```sh
brew install rbenv ruby-build
echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc
source ~/.zshrc
rbenv install 2.7.2
rbenv global 2.7.2

gem install bundler jekyll
bundle update --bundler
bundle update

bundle exec jekyll serve
```

## Create post

`rake post title="update jekyll"`
