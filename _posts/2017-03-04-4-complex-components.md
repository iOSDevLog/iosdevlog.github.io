---
layout: post
title: "4. 复杂组件"
description: "很好，我们已经构建了我们的第一个组件。现在让我们来看一下，开始构建一个更复杂的界面。"
category: 
tags: [React]
---


## 原文：[Complex Components](https://www.fullstackreact.com/30-days-of-react/day-4/)
---

前一部分，我们开始构建我们的第一个 `React` 组件。在本节中，我们将继续使用我们的App组件，并开始构建一个更复杂的UI。

我们可能会看到一个常见的网页元素是用户时间轴。例如，我们可能会有一个应用程序显示事件发生的历史，如Facebook和Twitter等应用程序。

我们可以在单个组件中构建整个UI。然而，在单个组件中构建整个应用程序不是一个好主意，因为它可能非常大，复杂且难以测试。

```html
class Timeline extends React.Component {
  render() {
    return (
      <div className="notificationsFrame">
        <div className="panel">
          <div className="header">
            
            <div className="menuIcon">
              <div className="dashTop"></div>
              <div className="dashBottom"></div>
              <div className="circle"></div>
            </div>

            <span className="title">Timeline</span>

            <input
              type="text"
              className="searchInput"
              placeholder="Search ..." />

            <div className="fa fa-search searchIcon"></div>
          </div>
          <div className="content">
            <div className="line"></div>
            <div className="item">

              <div className="avatar">
                <img 
                alt='doug'
                src="http://www.croop.cl/UI/twitter/images/doug.jpg" />
              </div>

              <span className="time">
                An hour ago
              </span>
              <p>Ate lunch</p>
            </div>

            <div className="item">
              <div className="avatar">
                <img
                  alt='doug' src="http://www.croop.cl/UI/twitter/images/doug.jpg" />
              </div>

              <span className="time">10 am</span>
              <p>Read Day two article</p>
            </div>

            <div className="item">
              <div className="avatar">
                <img
                  alt='doug' src="http://www.croop.cl/UI/twitter/images/doug.jpg" />
              </div>

              <span className="time">10 am</span>
              <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
            </div>

            <div className="item">
              <div className="avatar">
                <img
                  alt='doug' src="http://www.croop.cl/UI/twitter/images/doug.jpg" />
              </div>

              <span className="time">2:21 pm</span>
              <p>Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.</p>
            </div>

          </div>
        </div>
      </div>
    )
  }
}
```

![demo1](/assets/images/react/30/04/demo1.png)

## 拆分一下

比将其构建在单个组件中更好的做法是，我们将其分解成多个组件。

看这个组件，整个大组件有两个独立的部分：

1. 标题栏
1. 内容

![breakdown](/assets/images/react/30/04/breakdown.png)

我们可以将组件的内容部分划分成个别的关注点。内容部分内有3个不同的项目组件。

![breakdown-2](/assets/images/react/30/04/breakdown-2.png)

> 如果我们想进一步，我们甚至可以将标题栏分解成3个组件，菜单按钮，标题和搜索图标。如果我们需要，我们可以进一步深入每一个。
>  
> 决定划分组件的深度比科学更显得艺术。

在任何情况下，它通常是开始寻找使用的想法应用一个好主意组件。通过将我们的应用程序分解成组件，变得更容易测试，更容易跟踪哪些功能在哪里。

## 容器组件

要构建我们的通知应用程序，让我们开始构建容器来保存整个应用程序。我们的容器只是另外两个组件的**包装器**。

这些组件都不需要特殊的功能，它们看起来类似于我们的 `HelloWorld` 组件，因为它只是一个具有单个渲染功能的组件。 

我们来构建一个我们将要调用的包装器组件App，它们可能类似于：

```javascript
class App extends React.Component {
  render() {
    return (
      <div className="notificationsFrame">
        <div className="panel">
          {/* content goes here */}
        </div>
      </div>
    )
  }
}
```

> 请注意，我们使用 React 中调用的属性 `className`，而不是 HTML 版本 `class`。请记住，我们不是直接写 `DOM`，因此不会写 `HTML`，而是 `JSX`（这只是JavaScript）。
>
> 我们使用 `className` 的原因 `class` 是 `JavaScript` 是一个保留字。

## 子组件

当组件嵌套在另一个组件中时，它被称为*子组件*。组件可以有多个子组件。然后将使用子组件的组件称为*父组件*。

随着包装部件的定义，我们可以建立我们 `title` 和 `content` 通过，主要成分，从我们最初的设计抓住了源头，并把源文件到每个组件。

例如，标题组件看起来像这样，容器元素 `<div className="header">` ，菜单图标，标题和搜索栏：

```javascript
class Header extends React.Component {
  render() {
    return (
      <div className="header">
        <div className="fa fa-more"></div>

        <span className="title">Timeline</span>

        <input
          type="text"
          className="searchInput"
          placeholder="Search ..." />

        <div className="fa fa-search searchIcon"></div>
      </div>
    )
  }
}
```

![demo2](/assets/images/react/30/04/demo2.png)

最后，我们可以 `Content` 使用时间轴项目编写组件。每个时间轴项目被包装在单个组件中，具有与其相关联的头像，时间戳和一些文本。

```javascript
class Content extends React.Component {
  render() {
    return (
      <div className="content">
        <div className="line"></div>

      {/* Timeline item */}
        <div className="item">
          <div className="avatar">
            <img 
            alt='Doug'
            src="http://www.croop.cl/UI/twitter/images/doug.jpg" />
            Doug
          </div>

          <span className="time">
            An hour ago
          </span>
          <p>Ate lunch</p>
          <div className="commentCount">
            2
          </div>
        </div>

        {/* ... */}

      </div>
    )
  }
}
```

> 为了在 React 组件中编写注释，我们必须将其作为 JavaScript 中的多行注释放在括号中。

## 把它们放在一起

现在，我们有我们这两个*子组件*，我们可以设置`Header`和`Content`组件是孩子的的`App`组成部分。App然后，我们的组件可以使用这些组件，就像它们是浏览器内置的 HTML 元素一样。我们的新 `App` 组件，标题和内容现在看起来像：

```javascript
class App extends React.Component {
  render() {
    return (
      <div className="notificationsFrame">
        <div className="panel">
          <Header />
          <Content />
        </div>
      </div>
    )
  }
}
```

![headDemo](/assets/images/react/30/04/headDemo.png)

有了这些知识，我们现在有能力编写多个组件，我们可以开始构建更复杂的应用程序。

但是，您可能会注意到此应用没有任何用户交互或自定义数据。事实上，正如我们现在所说的那样，我们的React应用程序并不比直接构建不复杂的HTML容易。

在下一节中，我们将介绍如何使我们的组件更加动态，并使用 `React` 进行*数据驱动*。

