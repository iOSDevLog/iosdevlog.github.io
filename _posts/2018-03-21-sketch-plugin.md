---
layout: post
title: "sketch 插件开发官方文档"
author: iosdevlog
date: 2018-03-21 13:39:32 +0800
description: ""
category:  sketch
tags: [sketch, plugin]
---

1.  ## 入门

    1.  [插件基础](#插件基础)
    2.  [您的第一个插件](#您的第一个插件)
    3.  [开发环境](#开发环境)
    4.  [调试](#调试)
    5.  [ActionAPI](#ActionAPI)
    6.  [发布插件](#发布插件)

2.  ## 高级

    1.  [插件捆绑](#插件捆绑)
    2.  [插件，脚本和命令](#插件，脚本和命令)
    3.  [插件位置](#插件位置)
    4.  [更多关于CocoaScript](#更多关于CocoaScript)
    5.  [SketchTool](#SketchTool)

我们努力使Sketch成为梦想中的“设计师工具箱”。但是每个人都有不同的需求，也许你需要一个我们还没有实现的功能。不要担心：[插件已经可以满足您的需求](https://sketchapp.com/extensions/plugins/)，或者您可以轻松创建一个[插件](https://sketchapp.com/extensions/plugins/)。

如果您有兴趣扩展Sketch，那么您就位于正确的位置。在这里，我们展示Sketch可扩展性文档的概要以及如何快速构建您的第一个Sketch插件。

如果您只想使用现有的插件，请参阅[插件目录](https://sketchapp.com/extensions/plugins/)。

### 你可以用插件做什么？

Sketch中的插件可以做任何用户可以做的事情（甚至更多！）。例如：

* 根据复杂的规则选择文档中的图层
* 操作图层属性
* 创建新图层
* 以所有支持的格式导出资产
* 与用户交互（要求输入，显示输出）
* 从外部文件和Web服务获取数据
* 与剪贴板交互
* 操作Sketch的环境（编辑指南，缩放等...）
* 通过从插件调用菜单选项来自动化现有功能
* [设计规格](https://github.com/utom/sketch-measure)
* [内容生成](https://github.com/timuric/Content-generator-sketch-plugin)
* [透视转换](https://github.com/jamztang/MagicMirror)

查看Sketch插件的最简单方法是通过[插件目录](https://sketchapp.com/extensions/plugins/)。您可以浏览有用的插件，安装它们以尝试它们，并了解如何将Sketch扩展到您自己的设计场景。

### 编写一个扩展

我们创建了一个小工具链，这使得创建一个新插件变得非常简单。这对[开始](https://developer.sketchapp.com/guides/first-plugin)非常[有用](https://developer.sketchapp.com/guides/first-plugin)，你也可以找到现有的插件[示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples/)。

扩展程序是用JavaScript编写的。Sketch提供了一个小型REPL式控制台，您可以在尝试构建插件之前试用其API。

### 扩展想法

Sketch功能的许多优秀社区创意可以更好地实现为插件而不是核心产品的一部分。这样用户就可以通过安装正确的插件来挑选他们想要的功能。Sketch团队在[插件请求库中](https://github.com/sketchplugins/plugin-requests/issues)跟踪可能的插件为GitHub问题。如果你正在寻找一个伟大的插件来构建，请看看这些问题。

## 下一步

* [您的第一个插件](https://developer.sketchapp.com/guides/first-plugin) - 尝试创建一个简单的Hello World插件。
* [扩展API](https://developer.sketchapp.com/reference/) - 了解Sketch可扩展性API。
* [扩展示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples/) - 您可以查看和构建的扩展示例列表。
* [开发者论坛](http://sketchplugins.com/) - 一个论坛，插件开发者分享他们关于Sketch的所有知识的知识。

# 插件基础
---

在磁盘上，插件只是以特定布局排列的文件夹。

它包含一个或多个脚本。每个脚本定义一个或多个以某种方式扩展Sketch的命令。它还可以包含命令用于执行任何操作的任何其他可选资源（如图像）。

插件脚本使用JavaScript编写。

## 术语

在我们进一步讨论之前，让我们定义一些术语。

* *插件*：一组*脚本*，*命令*和其他资源组合在一起作为一个独立单元
* *Plugin Bundle*：磁盘上的文件夹，其中包含组成*插件*的文件
* *操作*：用户所做的事情（选择菜单或更改文档）触发*命令*
* *命令*：一个插件可以定义多个命令; 通常每一个都与不同的菜单或键盘快捷键相关联，并导致执行不同的*处理*程序。
* *Handler*：执行一些代码来实现*Command*的函数。
* *脚本*：包含一个或多个实现*处理程序*的*命令*的一个或多个JavaScript文件。

## 我如何制作插件？

到现在为止，你可能想知道如何开始写你自己的。

开始使用插件最简单的方法是打开Sketch，打开文档并`control + shift + k`打开`Run Script`面板。你不需要安装任何东西; 你可以打开它并在那里实验。如果您想使用真实的开发环境（您需要为了分发插件），请查看[开发环境](https://developer.sketchapp.com/guides/preferences)页面。

最小的插件示例如下所示：

```js
export default function(context) {
  context.document.showMessage('Hello, world!')
}
```

它在Sketch文档底部呈现一个Toast “Hello，world！”。

接下来的几个指南将逐渐向您介绍插件的内部工作。我们将检查插件的构建块：[清单](https://developer.sketchapp.com/guides/plugin-bundles/)和脚本。一旦你掌握了它们，你可以创建复杂的插件！

## 关于JavaScript的说明

Sketch插件是用JavaScript编写的，所以我们假设您对JavaScript语言有基本的了解。如果您觉得不太自信，我们建议[您刷新JavaScript知识，](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)以便更轻松地进行跟踪。

我们还在示例中使用了一些ES6语法。我们尽量少用，因为它还是比较新的，但我们鼓励您熟悉[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，[let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)和[const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)语句。

该脚本不在浏览器或节点环境中运行，而是在每个本机MacOS和Sketch API都暴露的[特殊环境](https://developer.sketchapp.com/guides/cocoascript/)中运行。这是一个先进的，但有必要真正理解如何建立更先进的东西。

原文：<https://developer.sketchapp.com/guides/>

# 您的第一个插件
---

本文档将带您创建您的第一个Sketch插件（“Hello World”），并将解释Sketch的基本扩展性概念。

在本演练中，您将向Sketch添加一个新命令，该命令将显示一个简单的“Hello World”消息。在稍后的演练中，您将与Sketch画布交互并查询用户当前选定的图层。

## 先决条件

您需要安装[Node.js](https://nodejs.org/en/)并且可以使用它`$PATH`。Node.js包括[npm](https://www.npmjs.com/)，Node.js包管理器，它将用于安装Sketch插件开发人员的工具链。

## 生成一个新的插件

将自己的功能添加到Sketch的最简单方法是通过添加命令。一个命令注册一个回调函数，该函数可以从插件菜单或键绑定中调用。

我们编写了一个小工具链，[`skpm`](https://github.com/skpm/skpm)以帮助您入门。安装`skpm`并搭建一个新的插件：

```bash
$ npm install -g skpm

$ skpm create my-plugin

$ cd my-plugin
```

## 运行你的插件

* 构建插件： `npm run build`
* 启动Sketch，打开文档
* 选择`Plugins`> `my-plugin`>`My Command`
* 恭喜！您刚刚创建并执行了您的第一个Sketch命令！

## 插件的结构

运行后，生成的插件应该具有以下结构：

```bash
.
├── .gitignore
├── README.md
├── src                         # sources
│   ├── manifest.json           # plugin's manifest
│   └── my-command.js           # source code of the command
├── node_modules
│   └── skpm                    # the sketch plugin developer toolchain
├── my-plugin.sketchplugin      # compilation output, the actual plugin
│   └── Contents
│       ├── Resources
│       └── Sketch
│           ├── manifest.json
│           └── my-command.js
└── package.json
```

让我们通过所有这些文件的目的，并解释他们做了什么：

### 插件清单： `manifest.json`

* 每个Sketch插件必须有一个描述它及其功能的manifest.json文件。
* Sketch在启动过程中读取此文件。
* 请阅读`manifest.json` [清单参考](https://developer.sketchapp.com/guides/plugin-bundles/#manifest)以获取更多信息。

### `package.json`

如果您之前查看过nodejs包，则必须熟悉它`package.json`。它描述了你的包（在这种情况下是插件）的依赖关系，并包含一些关于它的元数据。

你会注意到一个特殊的领域：`skpm`。你可以在这里指定关于你的插件的元数据（而不是在这里`manifest.json`）。作为一个经验法则，我通常会`manifest.json`在将所有其他信息放入时将相关命令的信息放入`package.json`（skpm将在编译时将这些信息添加到manifest.json中，以便您不必复制它们）。

### `src/my-command.js`

这是一个插件命令定义的地方。它被引用`manifest.json`并且必须导出一个函数。

## 一个简单的改变

在中`src/my-command.js`，尝试替换命令实现以显示所选图层的数量：

```js
export default function(context) {
  const selectedLayers = context.selection
  const selectedCount = selectedLayers.length

  if (selectedCount === 0) {
    context.document.showMessage('No layers are selected.')
  } else {
    context.document.showMessage(`${selectedCount} layers selected.`)
  }
}
```

通过运行重新构建插件`npm run build`。打开一个Sketch文档，选择一些图层。当您运行my-plugin命令时，您现在应该可以看到所选图层的数量。

> 专业提示：您可以通过运行自动重建插件 `npm run watch`

## 发布您的扩展

阅读关于如何[共享插件](https://developer.sketchapp.com/guides/publishing-plugins/)。

## 下一步

在这个演练中，我们看到了一个非常简单的插件。

如果您想更详细地了解插件API，请尝试以下主题：

* [扩展API概述](https://developer.sketchapp.com/reference/) - 了解Sketch可扩展性的可能性。
* [其他插件示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples) - 查看我们的示例插件项目列表。

原文：<https://developer.sketchapp.com/guides/first-plugin/>

# 开发环境
---

如果您花费了开发Plugins for Sketch的不少重要时间，则可以使用这些首选项对工作流程进行一些改进。

由于并非所有Sketch用户都是插件开发人员，因此在“首选项”面板中为这些首选项设置UI并没有任何意义。您需要使用Terminal.app来启用/禁用它们。

## 为插件定义一个代码编辑器

有最喜欢的代码编辑器？你可以告诉Sketch使用它来编辑插件。例如，如果你使用[Atom]，(https://atom.io/)你可以这样做：

```bash
$ defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist "Plugin Editor" "/usr/local/bin/atom"
```

并重新启动Sketch，您会看到一些新的菜单项：

* 转到首选项>插件并右键单击任何列出的插件。您将看到一个“编辑代码...”选项，该选项将启动编辑器并打开所选的插件代码。
* 打开插件菜单，你会看到一个'编辑插件...'选项，它将启动你的编辑器并打开整个'插件'文件夹。

## 调整“自定义插件...”编辑器

要更改“运行脚本...”面板中使用的字体（例如，使用SF Mono），可以这样做：

```bash
$ defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFont "SF Mono Light"
```

要回到默认设置（Andale Mono），只需删除首选项：

```bash
$ defaults delete ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFont
```

要更改编辑器的字体大小（默认值为12），请使用

```bash
$ defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFontSize 14
```

## 监听Action API中的所有操作

**警告：**这是一项非常昂贵的操作，并且会影响Sketch的性能。请*仅在您的开发系统上使用此功能*，而**不要在客户的计算机上启用此功能**。

当与新的合作[操作API](https://developer.sketchapp.com/reference/action/)，你可能想（试图找到时专门听取多个事件，*其* 事件是您要使用的一个）。

为此，请使用`actionWildcardsAllowed`首选项。如果设置为`YES`，则允许脚本为事件注册通配符处理程序。这是默认关闭的，它可能会对性能产生不利影响，因此请小心处理。

```bash
$ defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist actionWildcardsAllowed -bool YES
```

一旦你这样做了，你可以通过`*`在你的`handlers.actions`对象中添加一个键来告诉你的插件为每个操作调用一个方法`manifest.json`：

```diff
{
  ...
  "handlers": {
+    "actions": {
+      "*": "onActionHandler"
+    }
  }
  ...
}
```

## 运行前始终重新加载脚本

出于性能原因，Sketch会缓存Plugins文件夹的内容。这对用户来说非常方便，因为插件运行速度非常快，但如果您是开发人员，则会让您的生活变得艰难。这就是为什么我们添加了一个首选项来禁用此缓存机制并强制Sketch始终从磁盘重新加载插件的代码：

```bash
$ defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist AlwaysReloadScript -bool YES
```

如果启用此功能，只要保存脚本，就可以在Sketch中进行测试了（再见，只是为了测试一个小小的改变而重新启动它）！

请注意，此设置决定了当Sketch为脚本创建新的JavaScript上下文时，脚本的来源是否从光盘重新加载。如果是`NO`，源会被缓存，如果是`YES`，源始终会从光盘重新加载。

然而，当一个新的JavaScript上下文产生时，它不会做的事情就会改变。对于长时间运行的脚本，相同的上下文保存在内存中（它必须是 - 正在运行的脚本正在使用它），直到脚本退出。因此，如果您正在测试长时间运行的脚本，您仍然必须找到停止脚本的方法，以避免上下文丢失（通常意味着重新启动Sketch或设置`coscript.setShouldKeepAround(false)`）。

## 检查WebView

如果你的插件使用webview，很可能你需要在某个时候检查它。

为此，您需要添加首选项：

```bash
$ defaults write com.bohemiancoding.sketch3 WebKitDeveloperExtras -bool true
```

然后你可以简单地右键点击你的web视图并点击`Inspect`。检查员应该出现。

原文：<https://developer.sketchapp.com/guides/preferences/>

# 调试
---

开发Sketch插件时，您可能需要一些方法来了解代码运行时发生了什么。

## 日志

调试JavaScript代码最常用的方法是`console.log`在关键步骤中添加一堆。不幸的是，JavaScriptCore（[Sketch插件运行](https://developer.sketchapp.com/guides/cocoascript/)的[上下文](https://developer.sketchapp.com/guides/cocoascript/)）没有提供`console`。相反，调用的特殊方法`log`是可用的。

有几个选项可以查看这些日志：

* 打开Console.app并查找Sketch日志
* 看看这个文件`~/Library/Logs/com.bohemiancoding.sketch3/Plugin Output.log`
* 运行`skpm log`它将输出上面的文件（`skpm log -f`对日志进行流式处理）

`skpm`将填充`console`以便`console.log`照常使用。除了使用`log`场景后面的方法之外，它还会将日志转发给[`sketch-dev-tools`](https://github.com/skpm/sketch-dev-tools)。

## `debugger` 和变量检查

当插件运行时，Sketch会创建一个与其关联的JavaScript上下文。可以使用Safari检查此上下文。

在Safari中，转到`Develop`> *`name of your machine`*> `Automatically Show Web Inspector for JSContexts`。而且你可能想要启用，`Automatically Pause Connecting to JSContext`否则检查器将会关闭，然后才能与之交互（当脚本运行完成后，上下文将被销毁）。

现在，您可以在代码中使用断点，在运行时检查变量的值等。

## Objective-C类内省(Introspection)

Sketch中的插件系统可让您完全访问应用程序的内部结构和macOS中的核心框架。Sketch使用Objective-C构建，其类被桥接到JavaScript。知道你正在处理哪些类以及定义了哪些方法通常很有用。

您可以使用由网桥定义的一些自省方法来访问这些信息。例如：

```js
String(context.document.class()) // MSDocument

var mocha = context.document.class().mocha()

mocha.properties() // array of MSDocument specific properties defined on a MSDocument instance
mocha.propertiesWithAncestors() // array of all the properties defined on a MSDocument instance

mocha.instanceMethods() // array of methods defined on a MSDocument instance
mocha.instanceMethodsWithAncestors()

mocha.classMethods() // array of methods defined on the MSDocument class
mocha.classMethodsWithAncestors()

mocha.protocols() // array of protocols the MSDocument class inherits from
mocha.protocolsWithAncestors()
```

## Sketch的开发工具

我们创建了一个小的Sketch特定工具来帮助您调试插件，并希望让您的生活更轻松。它采用Sketch插件的形式，您可以[在此](https://github.com/skpm/sketch-dev-tools/releases/latest)下载并随其启动`cmd + option + j`。

原文：<https://developer.sketchapp.com/guides/debugging-plugins/>

# ActionAPI
---

在Sketch 3.8中，我们引入了Action API：一种让插件对应用程序中的事件作出反应的方式。使用它，插件作者可以编写在触发某些操作时执行的代码，如“打开文档”，“保存”，“添加画板”......

## 什么是操作？

操作是应用程序中发生的事件，通常是用户交互的结果。操作有名称`CloseDocument`，`DistributeHorizontally`或者`TogglePresentationMode`，你可以告诉你的插件在这些操作被触发时运行代码。

## 我如何注册我的插件来“聆听”一个操作？

简单：你只需在`manifest.json`你的插件已有的文件中添加一个处理程序。

我们将为该`OpenDocument`操作添加一个新的处理程序：

```diff
"commands" : [
  ...
+  {
+    "script" : "my-action-listener.js",
+    "name" : "My Action Listener",
+    "handlers" : {
+      "actions": {
+        "OpenDocument": "onOpenDocument"
+      }
+    },
+    "identifier" : "my-action-listener-identifier"
+  }
  ...
],
```

我们告诉我们的插件，我们希望`onOpenDocument`在文档打开时运行该功能，所以让我们将其添加到`my-action-listener.js`：

```js
export function onOpenDocument(context) {
  context.document.showMessage('Document Opened')
}
```

保存所有内容，构建插件，现在，无论何时在Sketch中打开文档，您都应该看到一个小小的吐司(Toast)横幅，上面写着“文档已打开”。

## 操作上下文

当一个操作被触发时，Sketch可以向目标函数发送一些关于操作本身的信息（例如选择改变时选择的图层，或者打开新文档时的当前文档）。我们称之为操作上下文，并且可以使用`context`作为目标函数的参数发送的操作`context.actionContext`。

但请记住，**并非所有操作都会设置Action Context**。事实上，他们中的大多数目前都没有，所以如果您认为您想在Action Context中访问某些内容，请向我们发送便条，然后尽快添加。

## `begin`/ `finish`操作

一些操作（如`SelectionChanged`）实际上发生在两个阶段：`begin`和`finish`。如果你想调用一个函数只对其中的一个，你可以添加一个处理程序`SelectionChanged.begin`，或`SelectionChanged.finish`。如果您不添加任何内容，该操作将被触发两次。

## 找到正确的操作

有关API中所有可用操作的列表，请查看[操作参考部分](https://developer.sketchapp.com/reference/action)。

> 专业提示：有时浏览列表的工作量太大，而您只想要更直接一些。对于这些情况，您可以[听取所有操作](https://developer.sketchapp.com/guides/preferences/#listen-to-all-actions-in-the-action-api)以找到您需要的一个。

再次，如果有任何事件想要添加到列表中，请告诉我们，我们将尝试添加它（由于性能原因，某些事件不在列表中，例如“图层被拖动”）。

## 下一步

如果您想更详细地了解Action API，请尝试以下主题：

* [Action API参考](https://developer.sketchapp.com/reference/action/) - 了解可用操作的完整列表。
* [其他插件示例](https://github.com/BohemianCoding/SketchAPI/tree/develop/examples) - 查看我们的示例插件项目列表。

原文：<https://developer.sketchapp.com/guides/action-api/>

# 发布插件
---

Sketch插件列在GitHub存储库中。本文档解释了如何在那里发布它以及如何让Sketch接收插件的更新。

## 第一次发布

Sketch插件列在GitHub存储库中：<https://github.com/sketchplugins/plugin-directory>。

要将您的插件添加到列表中，请使用关于您的插件的信息打开PR。合并后，您的插件将显示在此处：<https://sketchapp.com/extensions/plugins/>

如果您使用`skpm`，第一次使用插件发布`skpm publish`，它会自动为您创建PR。

## 发布更新

从Sketch v45起，Sketch提供了官方支持的机制来更新应用程序中的插件。

如果您的插件已经内置了自己的更新机制，我们鼓励您转向使用新系统。这将改善用户体验，因为用户将能够在应用程序的“首选项”面板中管理选项卡内的所有已安装插件。

启动时，我们检查所有安装插件的更新，如果有任何问题，我们会在Sketch的窗口上显示一个徽章。点击它会让用户访问应用程序的首选项，在那里他们将能够更新他们的插件。

目前Sketch只允许用户更新到最新版本。将来的Sketch版本可能会为用户提供更多的选项来选择可以下载和安装哪个插件版本。

您有两种解决方案可以选择使用此更新机制：

### 1.使用 `skpm`

通过运行`skpm publish`，它会自动发布插件的更新版本，并确保Sketch可以提取它。

### 2.手动

`manifest.json`包含在您的插件包中的文件中有一个额外的条目，您需要定义更新才能正常工作。

该条目被调用`appcast`，它是一个指定appcast文件的URL的字符串。

#### `appcast.xml`文件

appcast文件包含有关插件更新的信息，例如可用更新的版本以及可从中下载更新的位置。Sketch下载此文件以确定是否有可用的插件更新。

Appcast符合[Sparkle文档](https://sparkle-project.org/documentation/)和[发布更新页面中](https://sparkle-project.org/documentation/publishing/#publishing-an-update)描述的Sparkle定义的appcast 。对于Sketch插件，仅支持.zip文件作为附件。

当用于插件时，最小和最大系统版本不涉及操作系统的版本。究竟如何将它们用于更高版本的Sketch中仍未确定。

以下Appcast示例列出了插件的三个不同版本。每个版本都有自己的下载链接和简要说明文字。

```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0" xmlns:sparkle="http://www.andymatuschak.org/xml-namespaces/sparkle"  xmlns:dc="http://purl.org/dc/elements/1.1/">
  <channel>
    <title>Hello World Sketch Test Plugin</title>
    <link>http://sparkle-project.org/files/sparkletestcast.xml</link>
    <description>Brilliant Hello World Plugin</description>
    <language>en</language>
      <item>
        <title>Version 1.1</title>
        <description>
          <![CDATA[
            <ul>
              <li>Minor update v1.1</li>
            </ul>
          ]]>
        </description>
        <enclosure url="https://brillianthello.sketchplugins.com/files/HelloWorldSketchPluginTestv11.zip" sparkle:version="1.1" />
      </item>
      <item>
        <title>Version 1.2</title>
        <description>
          <![CDATA[
            <ul>
            <li>Minor update v1.2</li>
            </ul>
          ]]>
        </description>
        <enclosure url="https://brillianthello.sketchplugins.com/files/HelloWorldSketchPluginTestv12.zip" sparkle:version="1.2" />
      </item>
      <item>
        <title>Version 2.0</title>
        <description>
          <![CDATA[
            <ul>
            <li>Major update v2.0</li>
            </ul>
          ]]>
        </description>
        <enclosure url="https://brillianthello.sketchplugins.com/files/HelloWorldSketchPluginTestv20.zip" sparkle:version="2.0" />
      </item>
  </channel>
</rss>
```

### 在您的插件中实现启动和关闭方法

如果你的插件做了任何需要初始化的事情，你应该把这个`Startup`处理器作为插件的一部分。执行`Shutdown`处理程序也是一样，你应该实现你的插件需要的任何清理代码。你可能已经在使用这些事件，但是插件更新比以前更重要。

当插件更新时，正在更新的版本将发送该`Shutdown`操作。新版本将发送一个`Startup`操作。

例如，如果您的插件在Sketch中显示了一些用户界面元素，则应删除`Shutdown`处理程序中的那些元素。通过这种方式，新插件将能够显示已更新的用户界面组件以及所有旧用户界面元素已被删除。

对于插件所维护的任何持久数据也是如此。任何未保存的信息应在`Shutdown`调用时写入磁盘。

不要在`Startup`可以稍后运行的处理程序中包含代码。

### 故障排除

所以你已经遵循了所有的步骤，你的插件还没有更新？试试这些：

* 删除`PluginsWarehouse`居住的文件夹。这是我们缓存插件下载的地方，如果您已经测试了不同版本的appcast，那么您可能在那里有一些值得清理的旧东西。`~/Library/Application Support/com.bohemiancoding.sketch3/`
* 确保`manifest.json`您下载的ZIP中有与您的appcast中的版本号相匹配的版本号。如果appcast表示ZIP包含v1.2，但实际的ZIP表示它是v1.1，则安装将不起作用。

原文：<https://developer.sketchapp.com/guides/preferences/>

# 插件捆绑
---


插件是一个或多个**脚本**的集合。每个脚本定义一个或多个以某种方式扩展Sketch的**命令**。

在磁盘上，插件是具有`.sketchplugin`文件扩展名的文件夹，包含文件和子文件夹。

严格来说，插件实际上是一个[OS X软件包](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1)，被安排为[OS X软件包](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1)。

包是Finder向用户呈现的任何目录，就好像它是单个文件一样（您可以使用Finder中的“ **显示包内容”**命令查看内部）。

一个包是一个具有标准化层次结构的目录，该目录包含可执行代码和该代码使用的资源。

Sketch插件不允许本机编译代码，但我们确实使用标准包布局（例如，资源位于包中的资源/文件夹中），特定于插件的文件位于Sketch /目录中。

## 插件捆绑文件夹结构

Bundles包含一个`manifest.json`文件，一个或多个`.cocoascript`文件（包含用CocoaScript或JavaScript编写的脚本），它们实现Plugins菜单中显示的命令以及任意数量的共享库脚本和资源文件。

这是一个例子：

```bash
mrwalker.sketchplugin
  Contents/
    Sketch/
      manifest.json
      shared.js
      Select Circles.cocoascript
      Select Rectangles.cocoascript
    Resources/
      Screenshot.png
      Icon.png
```

最关键的文件是`manifest.json`文件，它告诉Sketch其他所有内容。

## 表现

清单是一个JSON文件，其中包含有关插件，其命令和资源的元数据。

它描述了诸如全名，描述和作者姓名等内容。它列出了插件定义的任何命令的名称，并告诉Sketch调用相应的菜单项以及将它们放入哪个菜单。

这是一个例子：

```json
{
  "name": "Select Shapes",
  "description": "Plugins to select and deselect shapes",
  "author": "Joe Bloggs",
  "homepage": "https://github.com/example/sketchplugins",
  "version": "1.0",
  "identifier": "com.example.sketch.shape-plugins",
  "appcast": "https://excellent.sketchplugin.com/excellent-plugin-appcast.xml",
  "compatibleVersion": "3",
  "bundleVersion": 1,
  "commands": [
    {
      "name": "All",
      "identifier": "all",
      "shortcut": "ctrl shift a",
      "script": "shared.js",
      "handler": "selectAll"
    },
    {
      "name": "Circles",
      "identifier": "circles",
      "script": "Select Circles.cocoascript"
    },
    {
      "name": "Rectangles",
      "identifier": "rectangles",
      "script": "Select Rectangles.cocoascript"
    }
  ],
  "menu": {
    "items": ["all", "circles", "rectangles"]
  }
}
```

这个插件被称为“选择形状”。它定义了三个命令“全部”，“圆”和“矩形”，它们将被放置在“选择形状”菜单中。

这个插件可以通过Sketch进行更新。Sketch将在指定的位置下载文件`appcast`并使用它来确定是否有更新。

将此文件进一步解压缩，以下是支持的密钥及其用途：

#### `name`

这个插件的名称。默认情况下，它将用作插件菜单命令出现的子菜单的名称。

#### `description`

描述此插件的命令（或命令）所做的字符串。

#### `author`

指定插件作者的字符串。

#### `authorEmail`

指定如何通过电子邮件与插件作者联系的可选字符串。

#### `homepage`

可选字符串，指定用户在线资源以查找更多信息或为插件提供反馈。

#### `version`

例如，一个字符串，指定插件的[语义版本](http://semver.org/)。`1.0``1.1.1`

#### `identifier`

一个字符串，指定插件的唯一标识符。

例如，强烈建议使用反向域语法`com.example.sketch.shape-plugins`。

Sketch在内部使用该字符串来跟踪插件，为其存储设置等。

#### `appcast`

指定appcast文件的URL的字符串。appcast文件包含有关插件更新的信息，例如可用更新的版本以及可从中下载更新的位置。Sketch下载此文件以确定是否有可用的插件更新。

#### `compatibleVersion` 和 `maxCompatibleVersion`

一个字符串，指定[版本](http://semver.org/)素描在其中作者已测试了插件，例如`3`，`3.1`，`3.2.2`。

目前（Sketch3.4）这是一个可选键，但我们可以在[插件页面](https://www.sketchapp.com/plugins/)的某个时刻将它用作过滤选项。

它在内部使用[BCCompareVersions](https://github.com/BohemianCoding/BCFoundation/blob/develop/Source/BCVersionComparison.m#L11)函数来分割字符串`.`，然后比较每个组件的整数值。

#### `bundleVersion`

元数据包的布局版本。如果排除，则假定值为1。

这只是我们面向未来的机制。如果将来我们看到bundleVersion> 1的插件，我们就会知道我们可以以不同的方式处理元数据中的其他值。

现在可以忽略它。

#### `disableCocoaScriptPreprocessor`

这是一个高级设置，默认为`false`。设置`true`为时，它将禁用CocoaScript自己的预处理器。这样，您就可以使用诸如browserify或ES6模块语法的构建系统来开发您的插件。

将此选项设置为`true`执行以下操作：

* 禁用`@import`支持，您必须手动处理您的导入
* 禁用括号语法（即`[obj msg:]`：），则只能使用点语法

#### `commands`

插件定义的一组命令。

数组中的每个项目都是一个字典，用于指定命令的名称，快捷方式和其他属性。有关更多详细信息，请参阅[插件命令](https://developer.sketchapp.com/guides/plugin-bundles/#plugin-commands)。

#### `menu`

描述此插件中命令的菜单布局的字典。

请参阅[插件菜单](https://developer.sketchapp.com/guides/plugin-bundles/#plugins-menu)以获取有关该词典内容的更多详细信息，以及如何构建每个插件的菜单。

## 插件命令

插件定义一个或多个用户执行的命令。

清单中的命令数组描述了这些。数组中的每个条目都是一个字典，具有以下属性：

#### `name`

命令的显示名称。该值在插件菜单中使用。

#### `identifier`

一个字符串，用于指定插件捆绑中命令的唯一标识符。这用于一致地将命令映射到操作，而不考虑命令名称的变化。

#### `shortcut`

一个可选的字符串，指定了该命令的默认快捷键，例如：`ctrl t`，`cmd t`，`ctrl shift t`。

#### `script`

`Sketch`实现此命令的脚本的插件包文件夹内的相对路径。

#### `handler`

用脚本调用此命令的函数的名称。该函数必须采用单个`context`参数，这是一个带有当前文档和选择项等键的字典。如果未指定，则该命令预期为`onRun`：

```js
var onRun = function (context) {
  var doc = context.document;
  var selection = context.selection;
  …
}
```

## 插件菜单

当它加载插件时，Sketch为它创建一个菜单，并使用清单文件中“菜单”字典中的信息填充该菜单。

该字典可以包含以下键。

#### `title`

指定用于子菜单的标题的字符串。

#### `items`

这是一个列出要包含在菜单中的项目的数组。

它可以包含两种类型的项目：

* 一个给出命令标识符的字符串
* 描述子菜单的字典（包含“标题”和“项目”）

#### `isRoot`

默认情况下，此字典中列出的菜单项将显示在菜单中，其名称由*标题*键指定。

如果指定了isRoot键，并且值为true，则这些项目将插入到插件菜单的根级别，而不是插入到子文件夹中。在这种情况下，*标题*密钥将被忽略。

*这个键在子菜单中被忽略。*

### 菜单示例

这是一个例子。它在名为“My Plugin Menu”的菜单中定义了三个命令。菜单的前两项对应于插件的两个命令，但第三项是名为“My Plugin Submenu”的子菜单。这个子菜单中有一个项目（插件命令的第三个项目）：

```json
{
  "menu": {
    "title": "My Plugin Menu",
    "items": [
      "command1-identifier",
      "command2-identifier",
      {
        "title": "My Plugin Submenu",
        "items": ["command3-identifier"]
      }
    ]
  }
}
```

## 处理程序

插件命令由处理程序实现。

这些只是生活在`.cocoascript`Plugin包中的一个文件中的JavaScript函数，它包含一个包含某个上下文的参数。

这里有一个简单的例子：

```js
var doMyCommand = function(context) {
  context.document.currentPage().deselectAllLayers()
}
```

在清单文件中，您可以指定一个描述插件定义的每个命令的字典。

在这本词典中，*脚本*和*处理程序*键告诉Sketch要查看哪个脚本文件，以及要运行哪个处理程序。

您可以自由地将每个命令实现放入其自己的脚本文件中，或将它们全部放入单个文件中。

您必须为每个命令指定*脚本*密钥。

如果将每个命令放入其自己的脚本文件中，则可以省略*处理程序*密钥。在这种情况下，Sketch将默认调用`onRun`处理程序。

如果将多个命令处理程序放入同一个脚本文件中，则需要为每个脚本文件使用*处理程序*密钥，因为它们不能全部使用`onRun`处理程序！

原文：<https://developer.sketchapp.com/guides/plugin-bundles/>

# 插件，脚本和命令
---

Sketch中的插件定义了一个或多个命令，其中Sketch将显示菜单项。

这些命令中的每一个实际上都是作为一个JavaScript函数实现的（我们称之为处理程序），位于该包中的脚本文件中。

每个脚本可以包含尽可能多的处理程序，并且每个命令都可以由不同的处理程序实现，因此，无论您是按照每个命令安排一个脚本，还是将所有命令处理程序放在单个脚本文件中，都由您决定。

因此，要了解如何制作插件，首先需要了解如何编写Sketch脚本。

教你如何编写JavaScript代码超出了这些页面的范围，所以我们假设你已经知道了这一点。如果没有，互联网上有很多好的[学习资源](https://developer.sketchapp.com/resources/)！

## 脚本语法

Sketch中的脚本使用[CocoaScript](https://github.com/ccgus/CocoaScript)编写。

这是一个桥梁，可让您编写可调用本机Objective-C / Cocoa的JavaScript脚本。

使用它，你可以用JavaScript编写你的插件的逻辑，但是当你想让它做某事时，可以调用实现Sketch的实际类和方法。

基础如下：

* 你会像往常一样编写JavaScript代码
* 使用桥接器，您可以从主机应用程序（在本例中为Sketch）或从系统本身获取Objective-C对象
* 基本的Objective-C对象具有等同的JavaScript（如字符串和数字），通常可以以与JS版本相同的方式使用
* 您可以像在JS中一样读取和写入自定义Objective-C对象的属性
* 您可以使用熟悉的JavaScript语法或Objective-C方括号语法来调用自定义Objective-C对象的方法。

（有关[更多](https://developer.sketchapp.com/guides/cocoascript/)详细信息，请参阅[更多关于CocoaScript](https://developer.sketchapp.com/guides/cocoascript/)页面。）

当您的脚本被Sketch调用时，您会传递一些*上下文*，包括表示当前Sketch文档和选择的Objective-C对象。

然后，您可以读取属性，执行计算并调用这些对象的方法，以完成脚本的目的。

## 脚本上下文

当用户选择插件菜单命令时，Sketch会查找要调用的处理程序（CocoaScript函数）以及调用它的脚本文件。

当处理程序被调用时，它会传递一个*上下文*变量。这包含一些重要的属性，您可以使用它们访问您需要的对象。

例如，selection属性为您提供当前文档中选定图层的列表：

```js
var onRun = function(context) {
  var selection = context.selection
  for (var i = 0; i < selection.count(); i++) {
    var layer = selection[i]
    log('layer ' + layer.name + ' is selected.')
  }
}
```

Sketch中的所有插件都可以访问以下默认变量：

* **command**：[`MSPluginCommand`](https://developer.sketchapp.com/reference/class/MSPluginCommand/)表示当前正在执行的脚本命令的对象
* **文档**：[`MSDocument`](https://developer.sketchapp.com/reference/class/MSDocument/)代表当前文档的对象
* **plugin**：[`MSPluginBundle`](https://developer.sketchapp.com/reference/class/MSPluginBundle/)表示包含当前正在执行的脚本的插件包的对象
* **scriptPath**：`NSString`包含当前正在执行的脚本的完整路径
* **scriptURL**：与**scriptPath**类似，但是作为NSURL对象
* **选择**`NSArray`当前文档中**选择的**一个或多个图层; 这个数组中的每一项都是一个[`MSLayer`](https://developer.sketchapp.com/reference/class/MSLayer/)对象

## 尝试脚本

尝试简单脚本的最简单方法是通过**插件>自定义插件...**菜单项。

这给你一个文本字段，你可以输入你的脚本。

点击**运行**按钮将执行脚本并在下面板显示任何输出或错误。

您可以使用此界面进行探索和实验。

## 创建一个插件

一旦你有一个你想要开发成适当的插件的脚本，你可以使用**Run Custom Script ...**表单中的**Save ...**按钮。

这将创建一个Plugin文件夹（称为[Plugin Bundle](https://developer.sketchapp.com/guides/plugin-bundles/)）并将脚本保存到其中。

生成的插件将具有单个命令和单个脚本文件。执行该命令将调用`onRun`脚本中的函数，该函数将包含您输入的代码。

从这个起点开始，您可以通过直接编辑文件夹中的文件来扩展您的插件。

你可以添加更多的代码到你的`onRun`函数，添加更多的功能，甚至更多的脚本文件。

通过编辑`manifest.json`插件文件夹中的文件，您可以自定义命令的名称，输入描述，甚至可以展开插件以定义多个命令。

有关更多信息，请参阅[插件包](https://developer.sketchapp.com/guides/plugin-bundles/)。

原文：<https://developer.sketchapp.com/guides/plugin-scripts/>

# 插件位置
---


当Sketch启动时，它会扫描磁盘上的文件夹以查找插件。

```bash
~/Library/Application Support/com.bohemiancoding.sketch3/Plugins
```

*（〜这里是你的主文件夹的简写，例如`/Users/iosdevlog`）*

您可以轻松访问此插件文件夹`Alt`，方法是打开插件菜单并选择“显示插件文件夹”。

### 安装插件

如果您双击.sketchplugin文件，Sketch会将其复制到您的Plugins文件夹中。它实现的任何命令应立即显示在**插件**菜单中。

或者，您可以通过简单地将它们自己移动到插件文件夹来安装插件。

*注意：Sketch也支持使用别名和指向单个插件的链接，或支持插件文件夹本身。这允许您将它们放置在其他地方（例如，Dropbox文件夹以保持Sketch同步的多个安装）。*

### 删除插件

要删除插件，只需选择**插件>管理插件...**菜单选项，选择要从列表中删除的插件，然后右键单击插件或单击齿轮图标，然后选择*卸载“插件名称”*：

![Uninstall](https://developer.sketchapp.com/images/developer/plugin-uninstall.png)

插件提供的任何命令都将立即从**插件**菜单中删除。

或者，您可以取消选中列表中的任何插件，以在不卸载它的情况下禁用它。

原文：<https://developer.sketchapp.com/guides/installing-plugins/>


# 更多关于CocoaScript
---

Sketch插件可以通过[Mocha](https://github.com/logancollins/Mocha)和[CocoaScript实现](https://github.com/ccgus/CocoaScript)，它允许您使用JavaScript编写的外部脚本使用Objective-C / Cocoa代码。该桥负责JavaScript和Cocoa之间的翻译，因此您可以专注于重要的部分（即使Sketch成为可怕的东西）。

来自CocoaScript的README：

> CocoaScript建立在Apple的JavaScriptCore之上，这是与Safari相同的JavaScript引擎。所以，当你在CocoaScript中编写代码时，你确实在编写JavaScript。
> 
> CocoaScript还包含一个桥梁，可让您通过JavaScript访问Apple的Cocoa框架。这意味着除了标准JavaScript库之外，您还可以使用许多精彩的类和函数。

## JavaScript环境

您的插件脚本不会在浏览器中运行，但会在JavaScriptCore上下文中运行。因此它运行的JavaScript环境有点不常见。

* 在[JavaScript的标准库](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)可用。
* 其他的都不是。这意味着`setTimeout`，`fetch`，`console`等都是不可用。
* NodeJS核心模块不可用。

话虽这么说，如果你使用`skpm`，它会自动填充工具有些事情你：[`console`](https://github.com/skpm/sketch-polyfill-console)，[`setTimeout`](https://github.com/skpm/sketch-polyfill-setTimeout)，[`setInterval`](https://github.com/skpm/sketch-polyfill-setInterval)和[`fetch`](https://github.com/skpm/sketch-polyfill-fetch)。

## 访问Cocoa和Sketch API

您可以从CocoaScript访问所有Cocoa和Sketch API。

Objective-C属性的行为与在桥的JavaScript端应该一样。

Objective-C方法作为对象的不透明JavaScript代理的属性公开。

将选择器名称转换为JavaScript属性名称时采取以下步骤：

* 所有冒号都转换为下划线（最新的下划线是可选的）。
* 选择器的每个组件都连接成一个没有分隔的字符串。

这样，一个选择器如`executeOperation:withObject:error:`转换为函数名称`executeOperation_withObject_error()`。

例如，如果你想打开一个File Picker面板，你可以使用[NSOpenPanel](https://developer.apple.com/documentation/appkit/nsopenpanel?language=objc)类：

```js
var openPanel = NSOpenPanel.openPanel()
openPanel.setCanChooseDirectories(false)
openPanel.setCanChooseFiles(true)
openPanel.setCanCreateDirectories(false)
openPanel.setDirectoryURL(NSURL.fileURLWithPath('~/Documents/'))

openPanel.setTitle('Choose a file')
openPanel.setPrompt('Choose')
openPanel.runModal()
```

如果您需要更多关于Cocoa的信息，请查看[参考资料](https://developer.sketchapp.com/resources/)部分。

## 一些特定的全局变量

### 指针

对于某些Obj-C选择器，您可能需要传递一个指针。这在JavaScript中不存在，所以有一种全局方法来创建一个：

```js
var ptr = MOPointer.alloc().init()
var ptrToSomething = MOPointer.alloc().initWithValue(something)
```

### 长时间运行脚本

如果您的脚本正在进行异步操作，我们需要告诉Sketch保留它并且不要垃圾收集它。

你可以通过访问`COScript`：

```js
COScript.currentCOScript().shouldKeepAround = true
```

脚本完成其工作后，不要忘记释放它：

```js
COScript.currentCOScript().shouldKeepAround = false
```

## 下一步

有关这座桥如何运作的更多信息，请查看[Mocha README](https://github.com/logancollins/Mocha)，它确实是完整的（但需要一些Obj-C的概念）。

原文：<https://developer.sketchapp.com/guides/cocoascript/>

# SketchTool
---

SketchTool是一个与Sketch捆绑在一起的命令行实用程序，它允许您使用Sketch文档执行一些操作，例如检查它们或导出资产。它还允许您从命令行控制Sketch以执行一些操作。

## 安装

SketchTool 与Sketch（和Sketch Beta）捆绑在一起。你可以找到它。

```bash
Sketch.app/Contents/Resources/sketchtool/bin/sketchtool
```

建议您在Sketch中使用它，而不是将其复制到其他位置，以便始终使用最新版本（更新Sketch时更新SketchTool，并且您需要使用更新后的版本进行阅读使用最新版本的Sketch保存的文件）。

> 注意：SketchTool需要**OSX 10.11**或更高版本。

### 重要

SketchTool可以免费使用，但它没有绝对担保。这就是说，如果您发现任何错误或有任何功能请求，请发送电子邮件给我们，我们将尽我们所能改善它。

如果所使用的所有字体已安装在系统上，SketchTool只能导出文档。

请注意，Sketch的未来版本将更改文件格式，因此请确保始终运行最新版本的工具。

## 用法

要了解可用的命令，请运行

```bash
$ sketchtool help
```

看到帮助。

以下是您可以使用SketchTool执行的一些示例

### 转储文件

```bash
$ sketchtool dump path/to/document.sketch
```

以JSON格式获取文档结构的转储。

如果您需要查看文档的元数据，但不想完整转储，则可以使用

```bash
$ sketchtool metadata path/to/document.sketch
```

你会得到类似的东西：

```json
{
  "commit" : "b8111e3393c4ca1f2399ecfdfc1e9488029ebe7b",
  "pagesAndArtboards" : {
    "E6890372-BE93-4E4C-ACD1-8F8B10862938" : {
      "name" : "Page 1",
      "artboards" : {
        "214B376A-C4A3-47A9-9B87-DFBC49A6EFE0" : {
          "name" : "Artboard 2"
        },
        "F8FE177A-5D6D-4A37-8BD1-B246A83A9C21" : {
          "name" : "Artboard 1"
        }
      }
    }
  },
  "version" : 97,
  "fonts" : [

  ],
  "compatibilityVersion" : 93,
  "app" : "com.bohemiancoding.sketch3",
  "autosaved" : 0,
  "variant" : "NONAPPSTORE",
  "created" : {
    "commit" : "b8111e3393c4ca1f2399ecfdfc1e9488029ebe7b",
    "appVersion" : "48.2",
    "build" : 47327,
    "app" : "com.bohemiancoding.sketch3",
    "compatibilityVersion" : 93,
    "version" : 97,
    "variant" : "NONAPPSTORE"
  },
  "saveHistory" : [
    "NONAPPSTORE.47327"
  ],
  "appVersion" : "48.2",
  "build" : 47327
}
```

## 导出资产

您可以使用SketchTool导出Sketch文档中的资源。SketchTool可以导出预定义的资源（即：在Sketch UI中可导出的图层和画板）或任何你想要的图层。

## 导出画板

运行

```bash
$ sketchtool export artboards path/to/document.sketch
```

将导出文档中的所有画板，无论其可导出状态如何。如果画板已设置为可导出，则SketchTool将导出所有尺寸和格式。否则，默认情况下，它们将以PNG格式以1x导出，您可以使用命令行选项指定自定义格式或大小：

```bash
$ sketchtool export artboards path/to/document.sketch --formats=jpg
```

您可以一次导出多个格式：

```bash
$ sketchtool export artboards path/to/document.sketch -formats = jpg，png，svg
```

要查看SketchTool支持哪些格式，请运行`sketchtool list formats`。

要定义大小，你可以这样做：

```bash
$ sketchtool export artboards path/to/document.sketch --scales=1,2
```

这会给你1x和2x版本的画板。

默认情况下，文件被导出到当前文档，但您可以像这样定义输出路径：

```bash
$ sketchtool export artboards path/to/document.sketch --output=output/path
```

如果不想导出所有画板，可以通过使用图层ID 的item或items选项来告诉SketchTool要导出的画板：

```bash
$ sketchtool export artboards path/to/document.sketch --item=214B376A-C4A3-47A9-9B87-DFBC49A6EFE0
```

（获取美工板的ID，使用`sketchtool metadata`或`sketchtool list artboards`）。

有关导出画板时可以执行的其他操作的更多信息，请参阅`sketchtool help export artboard`。

## 导出图层，切片或页面

图层，切片和页面就像画板一样工作，所以您可以阅读前一节用'图层'，'切片'或'页面'替换'画板'

## 获取文档预览

```bash
$ sketchtool export preview path/to/document.sketch
```

将为您提供文档中最后编辑页面的PNG预览，并将其另存为preview.png。SketchTool将尝试渲染100％的预览，但如果文档太大，则会缩小预览，使其适合2048 x 2048像素的矩形。

## 运行一个插件

SketchTool可以告诉Sketch启动并运行一个插件。如果您正在持续集成系统上测试插件，或者您需要自动执行无聊任务，这非常有用。

想象一下，我们有这个代码的插件：

```js
context.document.showMessage("Remote plugin running!")
```

我们从Run Script ...面板中将它保存为'Remote Plugin' ，然后运行：

```bash
$ sketchtool run ~/Library/Application\ Support/com.bohemiancoding.sketch3/Plugins/Remote\ Plugin.sketchplugin com.bohemiancoding.sketch.runscriptidentifier
```

请注意，这`com.bohemiancoding.sketch.runscriptidentifier`是Sketch在保存插件时使用的默认命令标识符，但在您的情况下它可能会有所不同。如果您只想运行包中的第一个命令，则可以使用`""`而不是标识符。

SketchTool现在将启动Sketch，等待文档打开，然后运行我们的插件。Sketch将成为最前端的应用程序，但如果您希望它保留在后台（例如，您正在运行代码编辑器的测试，并且不希望Sketch捕获焦点），则可以使用该`--without-activating`选项。

原文：<https://developer.sketchapp.com/guides/sketchtool/>

# 参考
---

Sketch中的插件系统可让您完全访问应用程序的内部结构和macOS中的核心框架。所以你有一个强大的能力来构建几乎*任何东西*。

然而，强大的能力有很大的责任，所以你需要在每个Sketch版本中留意你的代码。我们会在重构时不时更改Sketch的内部结构，因此您的插件可能会调用一些已重命名或删除的方法。

我们确实意识到这当然不是理想的。这就是为什么我们支持内部和插件之间的JavaScript API。我们希望它覆盖了90％的用例。如果没有，您可以随时进入内部，风险自担。

下面的页面包含插件可以侦听的所有操作的简要说明，以及一些可以与之交互的关键Sketch类。这是JavaScript API，它在Sketch版本中保持稳定。

* [Javascript API](https://developer.sketchapp.com/reference/api)
* [操作](https://developer.sketchapp.com/reference/action)

尽管我们不打算记录内部信息，但您可以查看3种信息来源：

* [官方的AppKit文件](https://developer.apple.com/documentation/appkit?language=objc)：这是建立在Apple框架上的Sketch。
* [基础](https://developer.apple.com/documentation/foundation?language=objc)：更重要的苹果课程和服务。
* [Sketch Headers](https://github.com/abynim/Sketch-Headers)（Thanks @abynim）：这是Sketch使用的所有类的标题。如果您的插件由于使用了已删除的方法而与新版本分离，则可以检查差异以查找替换。

再一次，最后一个环节是自负风险，我们不会记录或冻结这些，但我们希望给你做任何事情的权力。

要了解如何使用这些Objective-C类，请查看[CocoaScript文档](https://developer.sketchapp.com/guides/cocoascript/)。


原文：<https://developer.sketchapp.com/reference/>

# 资源
---

## JavaScript

### 彻彻底底的新手

* 在[Codecademy](https://www.codecademy.com/tracks/javascript)学习JavaScript的基础[知识](https://www.codecademy.com/tracks/javascript)
* [Eloquent JavaScript](http://eloquentjavascript.net/)，一本关于JavaScript，编程和数字奇迹的书。

### 有经验的开发者

* 在[Mozilla上](https://developer.mozilla.org/en/Learn/JavaScript)学习Web> JavaScript[](https://developer.mozilla.org/en/Learn/JavaScript)
* [重新介绍JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)
* [JavaScript：The Good Parts](http://shop.oreilly.com/product/9780596517748.do)，一本O'Reilly的书。

## Cocoa

* [AppKit](https://developer.apple.com/documentation/appkit?language=objc)是Sketch构建的主要Apple框架之一。
* [基础](https://developer.apple.com/documentation/foundation?language=objc)，更重要的苹果课程和服务。

## CocoaScript

* [Sketch-Plugins-Cookbook](https://github.com/turbobabr/Sketch-Plugins-Cookbook)，来自[Andrey Shakhmin的一系列](https://github.com/turbobabr)精彩技巧和信息
* [为插件开发人员绘制插件片段](https://medium.com/sketch-app-sources/sketch-plugin-snippets-for-plugin-developers-e9e1d2ab6827#.a3xn6hth6)
* [我做了一个Sketch插件,你也可以](https://medium.com/sketch-app-sources/i-made-a-sketch-plugin-you-can-too-58a28b7277f1#.52umaxe3i)
* [debugging-sketch-plugins](https://sketchplugindev.james.ooo/debugging-sketch-plugins-11cafc86df87#.64891ewop)
* [我如何在不知道代码的情况下为我的团队制作Sketch插件](http://hackingui.com/design/how-to-create-a-sketch-plugin/)
* [程序员设计不同：为什么我为Sketch 3构建了一个CSS插件](https://medium.com/sketch-app-sources/programmers-design-differently-why-i-built-a-css-plugin-for-sketch-3-52a1246305a4#.v0qjvzsfd)
* [runner-speed-up-your-sketch-workflow](https://medium.com/sketch-app-sources/runner-speed-up-your-sketch-workflow-fba470ed43c1#.bgdpr68wy)

## 示例插件

* [Github](https://github.com/BohemianCoding/ExampleSketchPlugins)上[提供了](https://github.com/BohemianCoding/ExampleSketchPlugins)一些示例插件[](https://github.com/BohemianCoding/ExampleSketchPlugins)
* 一个模板/示例Sketch插件，在Interface Builder中内置UI，并通过黑魔法连接到CocoaScript：[Sketch-NibUITemplatePlugin](https://github.com/romannurik/Sketch-NibUITemplatePlugin)

## 第三方插件

* [Sketch插件](https://github.com/sketchplugins/plugin-directory)，GitHub上托管的Sketch插件列表。
* [awesome-sket.ch](http://awesome-sket.ch/)，Sketch插件的[精选](http://awesome-sket.ch/)列表。
* [sketchplugins.com](http://sketchplugins.com/)，Sketch Plugin开发者的邮件列表。
* [Sketchpacks](http://www.sketchpacks.com/)，查找并发现Sketch最有用的插件

## 工具

* [SketchTool](https://sketchapp.com/tool) - 用于从`.sketch`文档中导出页面和切片的`OS X`命令行应用程序。
* [sketchapp-scripter](https://github.com/timuric/sketchapp-scripter)，由Timur Carpeev创建的一个Atom包，用于从Atom编辑器运行Sketch脚本。
* [class-dump](http://stevenygard.com/projects/class-dump/)。我们尽力记录所有内容，但如果您喜欢冒险类型，则可能需要玩这个游戏。
* [sketchpacks-relay](https://github.com/apps/sketchpacks-relay/)，[sketchpacks](https://sketchpacks.com/)。将您的Sketch插件发布到Sketchpacks插件注册表。自动[为您的Appcast Feed](https://docs.sketchpacks.com/developers/publishing/appcast.html)提供原生插件更新。
* [skpm](https://skpm.io/) - 用于创建，制作和发布Sketch插件的实用程序。

原文: <https://developer.sketchapp.com/resources/>
