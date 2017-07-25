---
layout: post
title: "Cocoa and UIKit Asynchronous APIs 1. Hello RxSwift"
description: ""
category: RxSwift
tags: [RxSwift, iOS]
---

<<RxSwift - Reactive Programming with Swift>>

## Cocoa and UIKit Asynchronous APIs / Cocoa和UIKit异步API
---

Apple has provided lots of APIs in the iOS SDK that help you write asynchronous code. You’ve used these in your projects, and probably haven’t given them a second thought because they are so fundamental to writing mobile apps.
You’ve probably used most of the following:

苹果公司在iOS SDK中提供了大量API，可帮助您编写异步代码。你在项目中使用了这些，可能没有更多想想，因为它们是编写移动应用程序的根本。
您可能已经使用了以下大部分内容：

* NotificationCenter: To execute a piece of code any time an event of interest happens, such as the user changing the orientation of the device, or the software keyboard showing or hiding on the screen.
* NotificationCenter：在任何感兴趣的事件发生时执行一段代码，例如用户更改设备的方向，或显示或隐藏在屏幕上的软件键盘。
* The delegate pattern: To define methods to be executed by another class or API at arbitrary times. For example, in your application delegate you define what should happen when a new remote notification arrives, but you have no idea when this piece of code will be executed, or how many times it will execute.
* 委托模式：定义在任意时间由另一个类或API执行的方法。例如，在您的应用程序委托中，您定义当新的远程通知到达时应该发生什么，但是您不知道这段代码将被执行，或者执行多少次。
* Grand Central Dispatch: To help you abstract the execution of pieces of work. You can schedule code to be executed sequentially in a serial queue, or run a multitude of tasks concurrently on different queues with different priorities.
* GCD：帮助您抽象执行工作。您可以调度在串行队列中顺序执行的代码，或者在具有不同优先级的不同队列上同时运行多个任务。
* Closures: To create detached pieces of code that you can pass between classes so each class can decide whether to execute it or not, how many times, and in what context.
* 闭包：创建可以在类之间传递的分离的代码段，以便每个类可以决定是否执行它，多少次以及在什么上下文中。

![Cocoa and UIKit Asynchronous APIs](/assets/images/Rx/RxSwift/Cocoa_and_UIKit_Asynchronous_APIs.png)

## Asynchronous programming glossary / 异步编程词汇表
---

1. State, and specifically, shared mutable state / 状态，特别，共享可变状态
1. Imperative programming / 命令式编程
1. Side effects / 副作用
1. Declarative code / 声明代码
1. Reactive systems / 反应体系

* Responsive: Always keep the UI up to date, representing the latest app state. 
* 响应：始终保持用户界面最新，代表最新的应用状态。
* Resilient: Each behavior is defined in isolation and provides for flexible error recovery.
* 适应力强的：每个行为被隔离定义并提供灵活的错误恢复。
* Elastic: The code handles varied workload, often implementing features such as lazy pull-driven data collections, event throttling, and resource sharing.
* 弹性：代码处理不同的工作负载，通常实现诸如延迟拉驱动数据收集，事件调节和资源共享等功能。
* Message driven: Components use message-based communication for improved reusability and isolation, decoupling the lifecycle and implementation of classes.
* 消息驱动：组件使用基于消息的通信来提高可重用性和隔离度，解除生命周期和类的实现。

### The three building blocks of Rx code are observables, operators, and schedulers.
---

### observables

* A next event: An event which "carries" the latest (or "next") data value. This is the way observers "receive" values.
* 下一个事件：“携带”最新（或“下一个”）数据值的事件。 这是观察员“收到”价值观。
* A completed event: This event terminates the event sequence with success. It means the Observable completed its life-cycle successfully and won’t emit any other events.
* 完成事件：此事件成功终止事件序列。 这意味着Observable可以成功完成其生命周期，也不会发生任何其他事件。
* An error event:The Observable terminates with an error and it will not emit other events.
* 错误事件：Observable出现错误并终止，不会发出其他事件。

### operators

![Operators](/assets/images/Rx/RxSwift/Operators.png)

### schedulers

![Schedulers](/assets/images/Rx/RxSwift/Schedulers.png)
