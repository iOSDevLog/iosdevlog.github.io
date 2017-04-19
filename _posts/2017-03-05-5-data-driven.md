---
layout: post
title: "5. Data Driven"
description: "我们的应用程序中的硬编码数据不太理想。今天，我们将把我们的组件设置为由数据驱动，访问外部数据。"
category: 
tags: []
---

通过这一点，我们已经编写了我们的第一个组件并将其设置为子/父关系。但是，我们还没有将任何数据绑定到我们的React组件。虽然在React中写一个网站是一个更愉快的体验（在我们看来），我们还没有利用React的力量来显示任何动态数据。

今天我们来改一下

## 数据驱动

回想一下，昨天我们构建了包含头和活动列表的时间轴组件的开始：

我们将演示分解成组件，最终用静态JSX模板构建了三个独立的组件。每当我们改变网站的数据时，不得不更新我们组件的模板是不方便的。

而是让我们给出要使用的组件数据进行显示。我们从 `<Header />` 组件开始吧。现在，`<Header />` 组件只显示元素的标题 `Timeline`。这是一个很好的元素，它将是很好的能够重用它在我们的页面的其他部分，但标题是 `Timeline` 没有意义的每一次使用。

让我们告诉React，我们希望能够将标题设置为别的东西。

## 介绍属性

React允许我们以与HTML相同的语法向组件发送数据，使用组件上的特性或 `属性`。这类似于将 `src` 属性传递给图像标签。我们可以考虑 `<img />` 标签的属性，因为prop我们正在设置调用的组件img。

我们可以访问组件内的这些属性 `this.props`。让我们看看在动作中使用 `props`。

回想一下，我们将 `<Header />` 组件定义为：

```jsx
class Header extends React.Component {
  render() {
    return (
      <div className="header">
        <div className="menuIcon">
          <div className="dashTop"></div>
          <div className="dashBottom"></div>
          <div className="circle"></div>
        </div>

        <span className="title">
          {this.props.title}
        </span>

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

当我们使用该 `<Header />` 组件时，我们将它放在我们的 `<App />` 组件中：

```javascript
<Header />
```

我们可以 `title` 作为一个属性传递我们作为一个属性，`<Header />` 通过更新组件的使用设置调用 `title` 某个字符串的属性，如下所示：

```javascript
<Header title="Timeline" />
```

在我们的组件内部，我们可以 `title` 从课程中的 `this.props` 属性访问 `Header`。而不是像 `Timeline` 模板一样静态设置标题，我们可以将其替换为传入的属性。

```javascript
import React from 'react'

class Header extends React.Component {
  render() {
    return (
      <div className="header">
        <div className="menuIcon">
          <div className="dashTop"></div>
          <div className="dashBottom"></div>
          <div className="circle"></div>
        </div>

        <span className="title">
          {this.props.title}
        </span>

        <input
          type="text"
          className="searchInput"
          placeholder="Search ..." />

        <div className="fa fa-search searchIcon"></div>
      </div>
    )
  }
}

export default Header
```
现在我们的 `<Header />` 组件将显示我们传入的字符串，`title` 当我们调用该组件时。例如，`<Header />` 像这样调用我们的组件四次：

```javascript
<Header title="Timeline" />
<Header title="Profile" />
<Header title="Settings" />
<Header title="Chat" />
```

结果四个 `<Header />` 组件加载完成后如下：

<div class="demo" id="demo2"></div>

很漂亮，是吗？现在我们可以复用 `<Header />` 组件, 使用一个动态 `title` 属性。

我们可以传递不仅仅是组件中的字符串。我们可以传递数字，字符串，各种对象，甚至功能！我们将进一步讨论如何定义这些不同的属性，以便稍后构建组件api。

通过数据变量而不是文本来设置时间轴内容。就像我们可以使用HTML组件一样，我们可以将多个 `props` 组件传递给组件。

回想一下，昨天我们定义了我们的 `Content` 容器，如下所示：

```javascript
class Content extends React.Component {
  render() {
    return (
      <div className="content">
        <div className="line"></div>

      {/* Timeline item */}
        <div className="item">
          <div className="avatar">
            <img src="http://www.croop.cl/UI/twitter/images/doug.jpg" />
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

和我们 `title` 一样，我们来看看 `props` 我们的 `Content` 组件需求：

* 用户的头像图片
* 活动的时间戳
* 活动项的文字
* 评论数量

假设我们有一个代表活动项目的JavaScript对象。我们将有一些字段，如字符串字段（文本）和日期对象。我们可能会有一些嵌套的对象 `user` 和 `comments`。例如：

```javascript
{
  timestamp: new Date().getTime(),
  text: "Ate lunch",
  user: {
    id: 1,
    name: 'Nate',
    avatar: "http://www.croop.cl/UI/twitter/images/doug.jpg"
  },
  comments: [
    { from: 'Ari', text: 'Me too!' }
  ]
}
```

就像我们将一个字符串标题传递给 `<Header />` 组件一样，我们可以把这个 activity 对象传递给 `Content` 组件。我们转换我们的组件来显示它的模板内的这个活动的细节。

为了将动态变量的值传递给一个模板，我们必须使用模板语法在我们的模板中呈现。例如：

```javascript
import React from 'react'

class Content extends React.Component {
  render() {
    const {activity} = this.props; // ES6 destructuring
    
    return (
      <div className="content">
        <div className="line"></div>

        {/* Timeline item */}
        <div className="item">
          <div className="avatar">
            <img
              alt={activity.text}
              src={activity.user.avatar} />
            {activity.user.name}
          </div>

          <span className="time">
            {activity.timestamp}
          </span>
          <p>{activity.text}</p>
          <div className="commentCount">
            {activity.comments.length}
          </div>
        </div>
      </div>
    )
  }
}

export default Content
```

> 我们在我们的类定义中使用了一点ES6，就是这个render()名为析构的函数的第一行。以下两行在功能上相当：
>
> ```javascript
> // these lines do the same thing
> const activity = this.props.activity;
> const {activity} = this.props; 
> ```
> 
> 解构使我们能够以更短，更紧凑的方式节省打字和定义变量。

然后，我们可以通过传递一个对象作为支持而不是硬编码的字符串来使用这个新内容。例如：

```javascript
<Content activity={moment1} />
```

<div class="demo" id="demo3"></div>

太棒了，现在我们有一个由一个对象驱动的活动项。但是，您可能已经注意到，我们将不得不使用不同的注释实现这个多次。相反，我们可以将一组对象传递到组件中。

假设我们有一个包含多个活动项目的对象：

```javascript
const activities = [
  {
    timestamp: new Date().getTime(),
    text: "Ate lunch",
    user: {
      id: 1, name: 'Nate',
      avatar: "http://www.croop.cl/UI/twitter/images/doug.jpg"
    },
    comments: [{ from: 'Ari', text: 'Me too!' }]
  },
  {
    timestamp: new Date().getTime(),
    text: "Woke up early for a beautiful run",
    user: {
      id: 2, name: 'Ari',
      avatar: "http://www.croop.cl/UI/twitter/images/doug.jpg"
    },
    comments: [{ from: 'Nate', text: 'I am so jealous' }]
  },
]
```

我们可以 `<Content />` 通过传递多个活动来重新阐述我们的使用，而不仅仅是一个：

```javascript
<Content activities={activities} />
```

但是，如果我们刷新视图什么都不会出现！我们需要先更新我们的 `Content` 组件以接受多个活动。正如我们以前了解到的，JSX真的只是由浏览器执行的JavaScript。我们可以在JSX内容中执行JavaScript函数，因为它将像浏览器的JavaScript一样运行。

我们将我们的活动项目 JSX 移动 `map` 到我们将针对每个项目运行的项目中。

```javascript
import React from 'react'

class Content extends React.Component {
  render() {
    const {activities} = this.props; // ES6 destructuring
    
    return (
      <div className="content">
        <div className="line"></div>

        {/* Timeline item */}
        {activities.map((activity) => {
          return (
            <div className="item">
              <div className="avatar">
                <img
                  alt={activity.text}
                  src={activity.user.avatar} />
                {activity.user.name}
              </div>

              <span className="time">
                {activity.timestamp}
              </span>
              <p>{activity.text}</p>
              <div className="commentCount">
                {activity.comments.length}
              </div>
            </div>
          );
        })}
        
      </div>
    )
  }
}

export default Content
```

<div class="demo" id="demo4"></div>

现在我们可以将任何数量的活动传递给我们的数组，`Content` 组件将处理它，但是如果我们现在离开组件，那么我们将有一个相对复杂的组件处理，包含和显示活动列表。像这样离开真的不是React的方式。

## 活动项（ActivityItem）

这里写一个组件包含显示单个活动项然后再建立一个复杂的 `Content` 组件是有意义的，我们可以移动责任。这也将使测试更容易，添加功能等

让我们更新我们的 `Content` 组件以显示组件列表 `ActivityItem`（我们将在下面创建）。

```javascript
import React from 'react'

import ActivityItem from './ActivityItem';

class Content extends React.Component {
  render() {
    const {activities} = this.props; // ES6 destructuring
    
    return (
      <div className="content">
        <div className="line"></div>

        {/* Timeline item */}
        {activities.map((activity) => (
          <ActivityItem
            activity={activity} />
        ))}
        
      </div>
    )
  }
}

export default Content
```

这不仅仅是简单易懂，而且使得这两个组件的测试更容易。

使用我们新鲜的 `Content` 组件，让我们创建 `ActivityItem` 组件。由于我们已经为此创建了视图 `ActivityItem`，所以我们需要做的就是将它从我们 `Content` 组件的模板复制为自己的模块。

```javascript
import React from 'react'

class ActivityItem extends React.Component {
  render() {
    const {activity} = this.props; // ES6 destructuring
    
    return (
      <div className="item">
        <div className="avatar">
          <img 
            alt={activity.text} 
            src={activity.user.avatar} />
          {activity.user.name}
        </div>

        <span className="time">
          {activity.timestamp}
        </span>
        <p>{activity.text}</p>
        <div className="commentCount">
          {activity.comments.length}
        </div>
      </div>
    )
  }
}

export default ActivityItem
```

<div class="demo" id="demo5"></div>

本周，我们使用React `props` 概念更新了由数据驱动的组件。在下一节中，我们将介绍有状态的组件。
