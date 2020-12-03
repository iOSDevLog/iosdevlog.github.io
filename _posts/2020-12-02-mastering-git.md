---
layout: post
title: "Mastering Git 掌握 Git"
author: iosdevlog
date: 2020-12-02 11:03:07 +0800
description: ""
cover-img: https://jeffkreeftmeijer.com/git-flow/git-flow.png
category: 
tags: git, Software Engineering, Tools & Libraries, iOS & Swift Tutorials, Android & Kotlin Tutorials, Server-Side Swift, Unity Tutorials, macOS Tutorials
---

Take the solid foundation laid by the Beginning Git course, and build upon it. Focus on fixing real-world problems, as you take a multi-user Git repository and work through the final steps of releasing a software product.

掌握入门 `Git` 课程奠定的坚实基础，并以此为基础。当您使用多用户 `Git` 仓库并完成发布软件产品的最后步骤时，请专注于解决实际问题。

```
fork → clone → branch → add → commit → push → pull request
派生 → 克隆 → 分支 → 添加 → 提交 → 推送 → 拉取请求（PR）
```
 
## Who is this for? 目标用户 [^1]

This course builds on the concepts raised on Beginning Git. Taking this course, you should have a basic understanding of Git. You should know how to create new branches, merge your changes, review logs and commit changes.

* Covered concepts
* How Git works
* Merging conflicts from different branches
* Storing changes between branches
* Creating custom commands in Git
* Rewriting commit history
* Ignoring committed files
* Incorporating specific changes between branches
* Revert changes
* Git user interfaces


本课程以 [*Beginning Git*](https://www.iosdevlog.com/2020-12-02-beginning-git/) 提出的概念为基础。学习本课程，您应该对 `Git` 有基本的了解。您应该知道如何创建新分支，合并更改，查看日志和提交更改。

* 涵盖的概念
* `Git` 如何工作
* 合并来自不同分支的冲突
* 在分支之间存储更改
* 在 `Git` 中创建自定义命令
* 重写提交历史
* 忽略提交的文件
* 合并分支之间的特定更改
* 还原更改
* `Git` 用户界面

## Implementation of Git Git 的实现

If you've been using Git for a while, you might be wondering how it actually works. In the first video of the Mastering Git video course, you'll discover how Git is built on top of a simple key-value store-based file system, and what power this provides to you.

如果您已经使用 `Git` 一段时间了，您可能想知道它的实际工作原理。在 *Mastering Git* 视频课程的第一个视频中，您将发现 `Git` 是如何在基于键值存储的简单文件系统之上构建的，并为您提供了什么功能。
 
## Merge Conflicts 合并冲突

In the Beginning Git video series, you discovered how powerful the branch-and-merge paradigm is. But merging isn't always as simple as it might first appear. In this video you will learn how to handle merge conflicts—which occur when Git cannot work out how to automatically combine changes.

在 *Begining Git* 视频系列中，您发现了分支合并范式的强大功能。但是合并并不总是像它第一次出现那样简单。在本视频中，您将学习如何处理合并冲突，当Git无法解决如何自动合并更改时，就会发生合并冲突。
 
## Stashes 贮藏

Git stashes offer a great way for you to create a temporary snapshot of what you're working on, without having to create a full-blown commit. Discover when that might be useful, and how to go about it.

`Git 贮藏` 为您提供了一种很好的方式来创建您正在处理的内容的临时快照，而不必创建完整的提交。发现什么时候可能有用，以及如何去做。
 
## Aliases 别名

Do you ever find yourself typing out the same long Git commands repeatedly? Well, now there's another way. Git aliases allow you to create new Git commands that can streamline your workflow.

您是否发现自己重复输入相同的长 `Git` 命令？好吧，现在还有另一种方式。`Git` 别名使您可以创建新的 `Git` 命令，以简化工作流程。
 
## Rebase: A Merge Alternative 变基：合并替代

Rebasing is often thought of as a mystical and complex tool. This video will demystify one of the primary uses of Git rebase—integrating branches without merge commits.

变基通常被认为是一个神秘而复杂的工具。该视频将揭开 `Git rebase` 的主要用途之一的神秘面纱-集成分支而不合并提交。
 
## Rebase: Rewriting History 变基：重写历史记录

Rebase is a whole lot more powerful than just as a replacement for merge. It offers the ability to completely rewrite the history of your git repo. Discover how in this action-packed video.


Rebase的功能远不止于合并的强大。它提供了完全重写git repo历史记录的功能。探索这个动感十足的视频。
 
## Gitignore After the Fact 出事后 Gitignore

Gitignore is easy right? If you've been using it for a while you'll know that isn't always true. Discover how you can fix problems with gitignore such as handling files that have been accidentally committed to the repository.

`Gitignore` 很容易吧？如果您已经使用了一段时间，您将知道并非总是如此。了解如何解决 `gitignore` 问题，例如处理意外提交到仓库的文件。
 
## Cherry Picking 选择合并（采摘樱桃）

Cherry picking provides a way to grab single commits from other branches, and apply them to your own branch. Learn how to achieve this, and why you might want to in the next video in the Mastering Git video course.

`Cherry Picking` 提供了一种从其他分支获取单个提交并将其应用于您自己的分支的方法。在 `Mastering Git` 视频课程的下一个视频中了解如何实现此目标以及为什么要这么做。

## Filter Branch 筛选分支

Interactive rebase allows you to rewrite history one commit at a time. But what if you want to automate that? In this video you'll see how you can use the filter-branch tool to programmatically rewrite history—kinda like a nerdy time traveller.

交互式 `rebase` 允许您一次重写一次提交的历史记录。但是，如果要自动化该怎么办？在此视频中，您将看到如何使用过滤器分支工具以编程方式重写历史记录，就像一个讨厌的时光旅行者。
 
## Many Faces of Undo 多种撤消

One of the common questions associated with git is "how can I get out of this mess?". In this video you'll learn about the different "undo" commands that git provides—what they are and when to use them.

与 `git` 相关的常见问题之一是“如何摆脱混乱？”。在本视频中，您将学习 `git` 提供的各种“撤消”命令-它们是什么，以及何时使用它们。
 
## GUIs: GITK

In the first of three videos about GUIs for Git, you'll take a look at Gitk and GitGUI—two simple tools that are distributed with Git itself.

在有关 `Git` 的 GUI 的三个视频的第一部分中，您将了解 `Gitk` 和 `GitGUI`，这是两个与 `Git` 一起分发的简单工具。
 
## GUIs: SourceTree

Next up on the whistlestop tour of popular Git GUIs is SourceTree. Learn how to get started with SourceTree, and some of the top tools it offers.

接下来是流行的 `Git` GUI的哨声之旅是 `SourceTree`。了解如何开始使用 `SourceTree` 及其提供的一些顶级工具。
 
## GUIs: GitUp

And finally, it's GitUp—objectively the best Git GUI there is. Find out why as we take a quick tour of how to get started with GitUp.

最后，它是 `GitUp`，这是客观上最好的 `Git GUI`。在快速浏览 `GitUp` 入门指南时，请找出原因。
 
## Conclusion 结论

A quick summary of what you've learnt throughout this Mastering Git video course. Remember that Git is a tool that you can use irrespective of your chosen platform, but that it is very much a tool.

关于您在本 *Mastering Git* 视频课程中学到的内容的快速摘要。请记住，无论您选择哪种平台，`Git` 都是您可以使用的工具，但它实际上是一个工具。

## 参考

[^1]: [Mastering Git](https://www.raywenderlich.com/4289-mastering-git)