---
layout: post
title: "Beginning Git 入门 Git"
author: iosdevlog
date: 2020-12-02 09:09:47 +0800
description: ""
category: 
cover-img: https://git-scm.com/images/branching-illustration@2x.png
tags: [git, Software Engineering, Tools & Libraries, iOS & Swift Tutorials, Android & Kotlin, Tutorials Server-Side Swift, Unity Tutorials, macOS Tutorials]
---

In this introduction to using Git for source control you'll learn everything from cloning and creating repos, through committing and ignoring files, to managing remotes and pull requests.

在使用 `Git` [^1] 进行源代码控制的简介中，您将学到从克隆和创建仓库，通过提交和忽略文件到管理远程和拉取请求的所有内容。
 
## Who is this for? 目标用户

* Anyone looking to learn how to work with Git. While the course is designed for software engineers, the course is accessible to any person looking to incorporate source control into their workflow.
* Covered concepts
* What is source control
* How Git works compared to other similar systems
* Cloning repos
* Creating a remote repo
* Committing changes
* Staging changes
* Ignoring files both locally and globally
* Viewing detailed histories
* Creating and deleting branches
* Pushing changes to remote services
* Merging changes
* Creating pull requests

* 任何想学习如何使用 `Git` 的人。尽管该课程是为软件工程师设计的，但是希望将源代码控制纳入其工作流程的任何人都可以访问该课程。
* 涵盖的概念
* 什么是源代码控制
* 与其他类似系统相比，`Git` 的工作方式
* 克隆仓库
* 创建一个远程仓库
* 提交变更
* 分期变更
* 忽略本地和全局文件
* 查看详细的历史记录
* 创建和删除分支
* 将更改推送到远程服务
* 合并变更
* 创建请求请求

## Introduction 简介 [^2]

Even if you’ve never used Git or Subversion before, you’ve probably practiced version control. Discover what version control is, and how exactly Git can help you with your source code.

即使您以前从未使用过 `Git` 或 `Subversion`，也可能已经练习过版本控制。了解什么是版本控制，以及 `Git` 如何为您提供源代码帮助。
 
## Cloning a Repo 克隆仓库

One of the ways you might start out with Git is by creating your own copy of somebody else's repository. Discover how to clone a remote repo to your local machine, and what constitutes "forking" a repository.

您可以从 `Git` 开始的一种方法是创建您自己的他人仓库副本。了解如何将远程仓库克隆到本地计算机，以及什么构成 **派生** 仓库。

```bash
git clone https://github.com/iOSDevLog/iOSDevLog.github.io iOSDevLog
```
 
## Creating a Repo 创建一个仓库

If you are starting a new project, and want to use Git for source control, you first need to create a new repository. Learn how you can get started initialising a new Git repository, and then look at some conventions that all code repos should adopt.

如果您要开始一个新项目，并想使用 `Git` 进行源代码控制，则首先需要创建一个新的仓库。了解如何开始初始化新的 `Git` 仓库，然后查看所有代码仓库应采用的一些约定。

```bash
git init
git add -A
git commit -m "First Commit!"
```
 
## Creating a Remote 创建一个远程

Code, like team sports, is meant to be shared with other people. Discover how you can create a remote for your new Git repo, and push it to GitHub for all your friends to enjoy.

就像团队运动一样，代码也应与其他人共享。探索如何为新的 `Git` 仓库创建一个远程服务器，并将其推送到 `GitHub`，供所有朋友使用。

```bash
git remote add origin https://github.com/iOSDevLog/iOSDevLog.github.io
git push --set-upstream origin master
# or
git push -u origin master
```
 
## Committing Changes 提交变更

A Git repo is made up of a sequence of commits—each representing the state of your code at a point in time. Discover how to create these commits to track the changes you make in your code.

`Git` 仓库由一系列提交组成，每个提交代表一个时间点的代码状态。探索如何创建这些提交以跟踪您在代码中所做的更改。

```bash
git config --global core.editor vim
git commit # use vim
git commit -m "First Commit!"
git commit --amend # 追加到最后一次提交
```
 
## The Staging Area 暂存区

Before you can create a Git commit, you have to use the  **add**  command. What does it do? Discover how to use the staging area to great effect through the interactive git add command.

在创建 `Git` 提交之前，您必须使用 `add` 命令。它有什么作用？通过交互式 `git add` 命令发现如何使用暂存区来产生巨大效果。

> -i
> --interactive
> 以交互方式将工作树中的修改内容添加到索引。可以提供可选的路径参数，以将操作限制为工作树的子集。有关详细信息，请参见`‘交互模式’'。
> 
> -p
> --patch
> 交互地在索引和工作树之间选择补丁块并将它们添加到索引中。这让用户有机会在将修改后的内容添加到索引之前查看差异。
> 
> 这可以有效地运行 add --interactive，但是会绕过初始命令菜单，而直接跳转到 patch 子命令。有关详细信息，请参见`‘交互模式’'。

```bash
git add . # 添加当前目录所有更改
git add -A # 当前仓库所有更改
```

## Ignoring Files 忽略文件

Sometimes, there are things that you really don’t want to store in your source code repository. Whether it be the diary of your teenage self, or build artifacts, you can tell Git to ignore them via the gitignore file.

有时，有些事情您确实不想存储在源代码仓库中。无论是您青少年时期的日记，还是生成物，您都可以通过 **gitignore** 文件告诉 `Git` 忽略它们。

1. GitHub Ignore [^3]
1. gitignore.io 可以选多种种类的 gitignore [^4]
 
## Viewing History 查看历史

There’s very little point in creating a nice history of your source code if you can’t explore it. In this video you’ll discover the versatility of the git log command—displaying branches, graphs and even filtering the history.

如果您无法探索源代码，那么创建一个很好的历史记录几乎没有意义。在此视频中，您将发现 `git log` 命令的多功能性-显示分支，图形甚至过滤历史记录。

```bash
git log
    --oneline # 每一个提交压缩到了一行
    --graph # ASCII 图像来展示提交历史的分支结构
    --decorate # 显示分支，标签等引用
    --all
# -s 参数省略每次 commit 的注释，仅仅返回一个简单的统计。
# -n 参数按照 commit 数量从多到少的顺利对用户进行排序
git shortlog -sn
git brame file # file 各行对应的 commit 及作者
```
 
## Branching 分支

The real power in Git comes from its branching and merging model. This allows you to work on multiple things simultaneously. Discover how to manage branches, and exactly what they are in this next video.

`Git` 的真正功能来自其分支和合并模型。这使您可以同时处理多个事情。在下一个视频中，了解如何管理分支以及它们的确切含义。

```bash
git branch -av # 本地和远程分支
git branch newBranch oldBranch
git checkout -b newBranch
# 等于以下 2 句
git branch newBranch
git checkout newBranch
```
 
## Merging 合并

Branches in Git without merging would be like basketball without the hoop—fun, sure, but with very little point. In this video you’ll learn how you can use merging to combine the work on multiple branches back into one.

不合并的 `Git` 中的分支就像没有箍的篮球一样，当然，但是一点都不有趣。在本视频中，您将学习如何使用合并将多个分支上的工作合并为一个。

```bash
git merge newBranch # 将 newBranch 合并到当前分支
git merge newBranch oldBranch # 将 newBranch 合并到 oldBranch
```
 
## Syncing with a Remote 与远程同步

Now that you’ve been working hard on your local copy of the Git repository, you want to know how you can share this with your friends. See how you can share through using remotes, and how you can use multiple remotes at the same time.

既然您已经在 `Git` 信息库的本地副本上进行了艰苦的工作，那么您想知道如何与朋友共享它。了解如何通过使用远程进行共享，以及如何同时使用多个远程。

```bash
git remote -v # 远程地址
git remote add upstream remote_url # 添加远程
```
 
## Pull Requests 拉取请求

GitHub introduced the concept of a Pull Request, which is essentially a managed merge with a large amount of additional metadata. Pull requests are key to a GitHub-based workflow, so you’ll discover how to use them in this video.

`GitHub` 引入了 **拉取请求** 的概念，该请求本质上是具有大量其他元数据的托管合并。拉取请求是基于 `GitHub` 的工作流的关键，因此您将在本视频中发现如何使用它们。

```bash
git pull origin master
# 等于以下 2 句
git fetch origin master
git merge origin/master
```
 
## Conclusion 总结

The Beginning Git video course took you from knowing nothing about Git, all the way to completing everything you need to know to use it in your daily development life. But wait… there’s more.

`Git` 入门视频课程使您从对 `Git` 一无所知，直到完成在日常开发中使用它所需的一切。但是等等...还有更多。

## 参考

[^1]: [Git 官方网站](https://git-scm.com)
[^2]: [Beginning Git](https://www.raywenderlich.com/4418-beginning-git)
[^3]: [GitHub gitignore](https://github.com/github/gitignore)
[^4]: [gitignore.io](https://gitignore.io/)