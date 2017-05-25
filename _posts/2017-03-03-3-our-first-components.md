---
layout: post
title: "3. 第一个组件"
description: "本系列的前两篇文章讨论的很多。在今天的会议中，让我们深入了解一些代码，并编写我们的第一个React应用程序。"
category: 
tags: [React]
---

让我们重温第一天介绍的`Hello world`应用程序。这次，略有不同：

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
    var app = <h1>Hello world</h1>;
    var mountComponent = document.querySelector('#app');
    ReactDOM.render(app, mountComponent);
  </script>
</body>
</html>
```

### 加载React库

我们在包含了React的来源作为`<script>`标签在`<head>`元素。在我们开始编写我们的React应用程序之前放置我们的`<script>`加载标签很重要，以便我们使用`ReactReactDOM`。

`head`还有一个`script`标签，包括一个库，babel-core。但是什么babel-core？

### Babel

我们提到对ES6的支持仍然不稳定。为了使用ES6，最好是将ES6 JavaScript转换为ES5 JavaScript。

**Babel**是一个翻译ES6到ES5的库。

在`body`里面我们有一个`script`。在里面`script`，我们定义了我们的第一个React应用程序。请注意，`script`标签具有`type`的`text/babel`：

```html
<script type="text/babel">
```

这告诉Babel，我们希望它处理这个script主体内的JavaScript的执行。我们可以使用ES6 JavaScript编写我们的React应用程序，并确保Babel将在仅支持ES5的浏览器中处理它的执行。

### React应用程序

在Babel `script`体中，我们定义了我们的第一个React应用程序。我们的应用程序由一个单一的元素组成`<h1>Hello world</h1>`。呼叫`ReactDOM.render()`实际上将我们的小React应用程序放置在页面上。没有调用ReactDOM.render()，DOM中不会呈现任何内容。的第一个参数ReactDOM.render()是什么来呈现，第二个是，其中：

```javascript
ReactDOM.render(<what>, <where>)
```

我们写了一个React应用程序。我们的“app”是一个代表一个h1标签的React元素。但这不是很有趣。富Web应用程序接受用户输入，根据用户交互更改其形状，并与Web服务器通信。让我们通过构建我们的第一个React组件来开始接触这个力量。

## 组件和更多

我们在本系列的开头提到，所有React应用程序的核心是组件。理解React组件的最好方法是编写它们。我们将把React组件写成ES6类。

让我们来看一个我们要调用的组件App。像所有其他React组件一样，这个ES6类将从React.ComponentReact包中扩展类：

```javascript
class App extends React.Component {
  render() {
    return <h1>Hello from our app</h1>
  }
}
```

> 所有React组件至少需要一个render()函数。此render()函数应返回浏览器DOM元素的虚拟DOM表示形式。

> 有多种方法来编写React组件。在本系列中，我们将介绍几种编写方法。我们将使用的最常见的形式是上面使用的ES6类表示。
>
> 我们可以编写我们的App组件的另一种方式是使用该React.createClass()函数。
>
> ```javascript
> var App = React.createClass({
>   render: function() {
>     return <h1>Hello from our app</h1>
>   }
> });
> ```
>
> 到目前为止，社区一直采用ES6类声明。但是这两种方法都产生具有相同特性的React组件。

在我们的index.html，我们用之前的新App组件替换我们的JavaScript。

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
    class App extends React.Component {
      render() {
        return <h1>Hello from our app</h1>
      }
    }
  </script>
</body>
</html>
```

然而，什么都不会在屏幕上呈现。你还记得为什么吗？

我们没有告诉React我们要在屏幕上渲染任何东西，或者在什么地方渲染它。我们需要ReactDOM.render()再次使用函数来表达React我们想要呈现的内容和位置。

添加ReactDOM.render()函数将在屏幕上呈现我们的应用程序：

```javascript
var mount = document.querySelector('#app');
ReactDOM.render(<App />, mount);
```

请注意，我们可以使用`App`类来渲染我们的React应用程序，就像它是一个内置的DOM组件类型（像`<h1 />`和`<div />`标签一样）。这里我们使用它，就像它是一个带尖括号的元素：`<App />`。

我们的React组件的行为就像我们页面上的任何其他元素一样，我们可以像构建一个本地浏览器树一样构建一个组件树。

虽然我们现在渲染一个React组件，我们的应用程序仍然缺乏丰富性或交互性。很快，我们将看到如何使React组件数据驱动和动态。但首先，在本系列的下一期中，我们将探讨如何对图层组件进行分层。嵌套组件是丰富的React Web应用程序的基础。
