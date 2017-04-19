---
layout: post
title: "1. 什么是 React?"
description: "今天，我们从一开始就开始。让我们看看React是什么，它是什么使得它。我们将讨论为什么要使用它。"
category: 
tags: [React]
---


## 原文：[What is React?](https://www.fullstackreact.com/30-days-of-react/day-1/)
---

在接下来的30天中，您将会对 [React](https://facebook.github.io/react/) Web框架及其生态系统的各个部分有一个大概的认识。

我们30天的冒险中的每一天都将基于前一天的材料，所以在系列的结尾，你不仅知道框架如何工作的术语，概念和基础，而且能够使用React下一个Web应用程序。

## 什么是React？

[React](https://facebook.github.io/react/)是一个用于构建用户界面的JavaScript库。它是Web应用程序的视图层。

所有React应用程序的核心是**组件(components)**。组件是一个自包含的模块，它提供一些输出。我们可以将类似按钮或输入字段的接口元素作为React组件。组件是可组合的。组件可以在其输出中包括一个或多个其他组件。

一般来说，为了编写React应用程序，我们编写了对应于各种接口元素的React组件。然后，我们将这些组件组织在定义应用程序结构的更高级组件中。

例如，采取一个表单。表单可能包含许多界面元素，例如输入字段，标签或按钮。窗体中的每个元素都可以写为React组件。然后我们写一个更高级的组件，形式组件本身。表单组件将指定表单的结构，并在其中包括每个这些接口元素。

重要的是，React应用程序中的每个组件都遵守严格的数据管理原则。复杂的交互式用户界面通常涉及复杂的数据和应用程序状态。React的表面区域是有限的，目的是给我们提供工具，以便能够预测我们的应用程序在给定的情况下的外观。我们在后面的课程中探讨这些原则。

## 好吧，那么我们如何使用呢？
---

React是一个JavaScript框架。使用框架就像在我们的HTML中包含一个JavaScript文件一样简单，并React在我们的应用程序的JavaScript中使用exports。

例如，React网站的Hello world示例可以如下简单：


```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Hello world</title>
  <!-- Script tags including React -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react-dom.min.js"></script>
  <script src="https://npmcdn.com/babel-core@5.8.38/browser.min.js"></script>
</head>
<body>
  <div id="app"></div>
  <script type="text/babel">
    ReactDOM.render(
      <h1>Hello world</h1>,
      document.querySelector('#app')
    );
  </script>
</body>
</html>
```

虽然它可能看起来有点可怕，JavaScript代码是一行动态添加Hello世界的页面。注意，我们只需要包括一些JavaScript文件，以使一切工作。

## 它是如何工作的？
---

与许多其前身不同，React不是直接在浏览器的文档对象模型（DOM）上运行，而是在**虚拟DOM(virtual DOM)**上运行。也就是说，而不是document在更改我们的数据之后在浏览器中操作（这可能很慢），它解决了其虚拟DOM中的更改。在更新虚拟DOM之后，React会智能地确定对实际DOM所做的更改。

[虚拟DOM](https://facebook.github.io/react/docs/dom-differences.html) 完全存在于内存中，并且是网络浏览器的DOM的表示。因此，当我们写一个React组件时，我们不是直接写入DOM，而是写一个虚拟组件，React将变成DOM。

在下一篇文章中，我们将看看这对我们构建React组件和跳到JSX并编写我们的第一个真正组件意味着什么。
