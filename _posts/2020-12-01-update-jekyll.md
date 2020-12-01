---
layout: post
title: "Update jekyll"
author: iosdevlog
date: 2020-12-01 15:32:13 +0800
description: ""
category: github
tags: [jekyll]
---

## Bundle Jekyll

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

## Beautiful Jekyll

<http://beautifuljekyll.com>

Method 1: Using remote_theme with a GitHub repository Show Steps

* Create a new GitHub repository or go to an existing repository
* Add remote_theme: daattali/beautiful-jekyll@5.0.0 to your _config.yml file (make sure to remove any previous theme or remote_theme parameters that may have been there before)
* Go to Settings, scroll down to the GitHub Pages section, and choose “master branch” as the source
* Your website will be at https://<yourusername>.github.io\<projectname>

## Create post

`rake post title="update jekyll"`

## GitHub warning

```
Dependabot cannot update kramdown to a non-vulnerable version
The latest possible version that can be installed is 1.17.0 because of the following conflicting dependency:

jekyll (3.8.7) requires kramdown (~> 1.14)
The earliest fixed version is 2.3.0.
View logs or learn more about troubleshooting Dependabot errors.
```

kramdown

```diff
 #
 # This will help ensure the proper Jekyll version is running.
 # Happy Jekylling!
-gem "jekyll", "~> 3.8.5"
+# gem "jekyll", "~> 3.8.5"

 # This is the default theme for new Jekyll sites. You may change this to anything you like.
 # gem "minima", "~> 2.0"

 # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
 # uncomment the line below. To upgrade, run `bundle update github-pages`.
-# gem "github-pages", group: :jekyll_plugins
+gem "github-pages", group: :jekyll_plugins

 # If you have any plugins, put them here!
 ```

 `bundle update`