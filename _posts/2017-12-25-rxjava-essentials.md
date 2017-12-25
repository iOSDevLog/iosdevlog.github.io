---
layout: post
title: "《RxJava Essentials》 读书笔记"
author: iosdevlog
date: 2017-12-25 11:22:28 +0800
description: ""
category: 读书笔记
tags: [RxJava, Android]
---

# 源码 <https://github.com/hamen/rxjava-essentials>


从纯Java的观点看，RxJava Observable类源自于经典的Gang Of Four的观察者模式。

它添加了三个缺少的功能：

* 生产者在没有更多数据可用时能够发出信号通知：onCompleted()事件。
* 生产者在发生错误时能够发出信号通知：onError()事件。
* RxJava Observables 能够组合而不是嵌套，从而避免开发者陷入回调地狱。

| Pattern | 一个返回值 | 多个返回值  |
| ------------- |:-------------:|:-----:|
| Synchronous |`T getData()`| `Iterable<T>` |
| Asynchronous | `Future<T> getData()`|`Observable<T> getData()`|

## 你什么时候使用观察者模式？
---

* 当你的架构有两个实体类，一个依赖另一个，你想让它们互不影响或者是独立复用它们时。
* 当一个变化的对象通知那些与它自身变化相关联的未知数量的对象时。
* 当一个变化的对象通知那些无需推断具体是谁的对象时。

## RxJava观察者模式工具包
---

在RxJava的世界里，我们有四种角色：

* Observable
* Observer
* Subscriber
* Subjects

Observables和Subjects是两个“生产”实体，Observers和Subscribers是两个“消费”实体。

## Observable
---

当我们异步执行一些复杂的事情，Java提供了传统的类，例如Thread、Future、FutureTask、CompletableFuture来处理这些问题。当复杂度提升，这些方案就会变得麻烦和难以维护。最糟糕的是，它们都不支持链式调用。

RxJava Observables被设计用来解决这些问题。它们灵活，且易于使用，也可以链式调用，并且可以作用于单个结果程序上，更有甚者，也可以作用于序列上。无论何时你想发射单个标量值，或者一连串值，甚至是无穷个数值流，你都可以使用Observable。

Observable的生命周期包含了三种可能的易于与Iterable生命周期事件相比较的事件，下表展示了如何将Observable async/push 与 Iterable sync/pull相关联起来。

| Event| Iterable(pull)|Observable(push)|
| ------------- |:-------------:|:-----:|
| 检索数据|`T next()`| `onNext(T)` |
| 发现错误| `throws Exception`|`onError(Throwable)`|
| 完成    |`!hasNext()`|`onCompleted()`|

使用Iterable时，消费者从生产者那里以同步的方式得到值，在这些值得到之前线程处于阻塞状态。相反，使用Observable时，生产者以异步的方式把值推给观察者，无论何时，这些值都是可用的。这种方法之所以更灵活是因为即便值是同步或异步方式到达，消费者在这两种场景都可以根据自己的需要来处理。

为了更好地复用Iterable接口，RxJava Observable类扩展了GOF观察者模式的语义。引入了两个新的接口：

* onCompleted() 即通知观察者Observable没有更多的数据。
* onError() 即观察者有错误出现了。

## 热Observables和冷Observables
---

从发射物的角度来看，有两种不同的Observables:热的和冷的。

* 一个"热"的Observable典型的只要一创建完就开始发射数据，因此所有后续订阅它的观察者可能从序列中间的某个位置开始接受数据（有一些数据错过了）。
* 一个"冷"的Observable会一直等待，直到有观察者订阅它才开始发射数据，因此这个观察者可以确保会收到整个数据序列。

### Observable.create()

create()方法使开发者有能力从头开始创建一个Observable。它需要一个OnSubscribe对象,这个对象继承Action1,当观察者订阅我们的Observable时，它作为一个参数传入并执行call()函数。

```java
Observable.create(new Observable.OnSubscribe<Object>(){
    @Override
    public void call(Subscriber<? super Object> subscriber) {

    }
});
```

### Observable.from()

从一个已有的列表中创建一个Observable序列：

```java
Observable<Integer> observableString = Observable.from(items);
```

### Observable.just()

如果我们已经有了一个传统的Java函数，我们想把它转变为一个Observable又改怎么办呢？我们可以用`create()`方法，正如我们先前看到的，或者我们也可以像下面那样使用以此来省去许多模板代码：


```java
Observable<String> observableString = Observable.just(helloWorld());
```

```java
private String helloWorld() {
    return "Hello World";
}
```

### Observable.empty(),Observable.never(),和Observable.throw()

当我们需要一个Observable毫无理由的不再发射数据正常结束时，我们可以使用`empty()`。我们可以使用`never()`创建一个不发射数据并且也永远不会结束的Observable。我们也可以使用`throw()`创建一个不发射数据并且以错误结束的Observable。

### Subject = Observable + Observer

subject是一个神奇的对象，它可以是一个Observable同时也可以是一个Observer：它作为连接这两个世界的一座桥梁。一个Subject可以订阅一个Observable，就像一个观察者，并且它可以发射新的数据，或者传递它接受到的数据，就像一个Observable。很明显，作为一个Observable，观察者们或者其它Subject都可以订阅它。

一旦Subject订阅了Observable，它将会触发Observable开始发射。如果原始的Observable是“冷”的，这将会对订阅一个“热”的Observable变量产生影响。

RxJava提供四种不同的Subject：

* PublishSubject
* BehaviorSubject
* ReplaySubject.
* AsyncSubject

### PublishSubject

Publish是Subject的一个基础子类。

```java
PublishSubject<String> stringPublishSubject = PublishSubject.create();
stringPublishSubject.onNext("Hello World");
```

### BehaviorSubject

简单的说，BehaviorSubject会首先向他的订阅者发送截至订阅前最新的一个数据对象（或初始值）,然后正常发送订阅后的数据流。

```java
BehaviorSubject<Integer> behaviorSubject = BehaviorSubject.create(1);
```
在这个短例子中，我们创建了一个能发射整形(Integer)的BehaviorSubject。由于每当Observes订阅它时就会发射最新的数据，所以它需要一个初始值。
### ReplaySubject

ReplaySubject会缓存它所订阅的所有数据,向任意一个订阅它的观察者重发:
```java
ReplaySubject<Integer> replaySubject = ReplaySubject.create();
```

### AsyncSubject

当Observable完成时AsyncSubject只会发布最后一个数据给已经订阅的每一个观察者。

```java
AsyncSubject<Integer> asyncSubject = AsyncSubject.create();
```

## RxAndroid
---

RxAndroid是RxJava家族的一部分。它基于RxJava1.0.x,在普通的RxJava基础上添加了几个有用的类。大多数情况下，它为Android添加了特殊的调度器。

### 工具

#### Lombok

Lombok使用注解的方式为你生成许多代码。大多数情况下我们使用其生成getter/setter、toString()、equals()、hashCode()。它来自于一个Gradle依赖和Android Studio插件。

#### Butter Knife

Butter Knife使用注解的方式来帮助我们免去写findViewById()和设置点击监听的痛苦。至于Lombok,我们可以通过导入依赖和安装Android Studio插件来获得更好的体验。

#### Retrolambda

Android Studio 默认支持 lambda，不用些插件

* create 从头创建一个Observable
* from 从列表创建一个Observable
* just 你可以将一个函数作为参数传给just方法，你将会得到一个已存在代码的原始Observable版本。在一个新的响应式架构的基础上迁移已存在的代码，这个方法可能是一个有用的开始点。
* repeat 重复发射
* defer 推迟这个Observable的创建直到观察者订阅时
* range 函数用两个数字作为参数：第一个是起始点，第二个是我们想发射数字的个数。
* interval函数在你需要创建一个轮询程序时非常好用。
* timer 创建一个以初始值来延迟执行的interval版本，然后每隔N秒就发射一个新的数字

## 过滤Observables
---

* filter 过滤序列

![filter](https://rxjava.yuxingxin.com/images/chapter4_1.png)

* take 取开头的几个元素

![take](https://rxjava.yuxingxin.com/images/chapter4_2.png)

* takeLast 取结尾的几个元素

![takeLast](https://rxjava.yuxingxin.com/images/chapter4_3.png)

* distinct 去掉重复的
* distinctUntilChanged 忽略掉所有的重复并且只发射出新的值
* first & last 从一个从可观测源序列中创建只发射第一/最后一个元素的序列

![first](https://rxjava.yuxingxin.com/images/chapter4_8.png)

![last](https://rxjava.yuxingxin.com/images/chapter4_9.png)

* skip & skipLast 不发射、跳过

![skip](https://rxjava.yuxingxin.com/images/chapter4_10.png)

![skipLast](https://rxjava.yuxingxin.com/images/chapter4_11.png)

* elementAt 仅从一个序列中发射第n个元素

![elementAt](https://rxjava.yuxingxin.com/images/chapter4_12.png)

* sampling 创建一个新的可观测序列，它将在一个指定的时间间隔里由Observable发射最近一次的数值

![sampling](https://rxjava.yuxingxin.com/images/chapter4_13.png)

* timeout 一个Observable的限时的副本。如果在指定的时间间隔内Observable不发射值的话，它监听的原始的Observable时就会触发onError()函数。

![timeout](https://rxjava.yuxingxin.com/images/chapter4_14.png)

* debounce 过滤掉由Observable发射的速率过快的数据；如果在一个指定的时间间隔过去了仍旧没有发射一个，那么它将发射最后的那个。

![debounce](https://rxjava.yuxingxin.com/images/chapter4_15.png)

## 转换Observables
---

* Map 接收一个指定的Func对象然后将它应用到每一个由Observable发射的值上

![map](https://rxjava.yuxingxin.com/images/chapter5_1.png)

* FlatMap 提供一种铺平序列的方式，然后合并这些Observables发射的数据，最后将合并后的结果作为最终的Observable。

![flatMap](https://rxjava.yuxingxin.com/images/chapter5_2.png)

* ConcatMap 解决了flatMap()的交叉问题，提供了一种能够把发射的值连续在一起的铺平函数，而不是合并它们

![ConcatMap](https://rxjava.yuxingxin.com/images/chapter5_3.png)

* FlatMapIterable flatMapInterable()和flatMap()很像。仅有的本质不同是它将源数据两两结成对并生成Iterable，而不是原始数据项和生成的Observables

![FlatMapIterable](https://rxjava.yuxingxin.com/images/chapter5_4.png)

* SwitchMap switchMap()和flatMap()很像，除了一点：每当源Observable发射一个新的数据项（Observable）时，它将取消订阅并停止监视之前那个数据项产生的Observable，并开始监视当前发射的这一个。

![SwitchMap](https://rxjava.yuxingxin.com/images/chapter5_5.png)

* Scan 一个累积函数。scan()函数对原始Observable发射的每一项数据都应用一个函数，计算出函数的结果值，并将该值填充回可观测序列，等待和下一次发射的数据一起使用。

![Scan](https://rxjava.yuxingxin.com/images/chapter5_6.png)

![scan](https://rxjava.yuxingxin.com/images/chapter5_7.png)

* GroupBy 从列表中按照指定的规则分组元素

![GroupBy](https://rxjava.yuxingxin.com/images/chapter5_8.png)

* Buffer 将源Observable变换一个新的Observable，这个新的Observable每次发射一组列表值而不是一个一个发射。

![Buffer](https://rxjava.yuxingxin.com/images/chapter5_10.png)

![Buffer skip](https://rxjava.yuxingxin.com/images/chapter5_11.png)

![Buffer timespan](https://rxjava.yuxingxin.com/images/chapter5_12.png)

* Window 和buffer()很像，但是它发射的是Observable而不是列表

![Window](https://rxjava.yuxingxin.com/images/chapter5_13.png)

![Window skip](https://rxjava.yuxingxin.com/images/chapter5_14.png)

* Cast 它是map()操作符的特殊版本。它将源Observable中的每一项数据都转换为新的类型，把它变成了不同的Class

![Cast](https://rxjava.yuxingxin.com/images/chapter5_15.png)

## 组合Observables
---

* Merge 把两个甚至更多的Observables合并到他们发射的数据项里

![Merge](https://rxjava.yuxingxin.com/images/chapter6_1.png)

![Merge](https://rxjava.yuxingxin.com/images/chapter6_3.png)

* ZIP 多从个Observables接收数据，处理它们，然后将它们合并成一个新的可观测序列来使用

![ZIP](https://rxjava.yuxingxin.com/images/chapter6_4.png)

* Join 基于时间窗口将两个Observables发射的数据结合在一起

![Join](https://rxjava.yuxingxin.com/images/chapter6_6.png)

![Join](https://rxjava.yuxingxin.com/images/chapter6_8.png)

* combineLatest 

![combineLatest](https://rxjava.yuxingxin.com/images/chapter6_9.png)

* And、Then & When

![and](https://rxjava.yuxingxin.com/images/chapter6_11.png)

* Switch 发射多个Observables的Observable转换成另一个单独的Observable，后者发射那些Observables最近发射的数据项

![Switch](https://rxjava.yuxingxin.com/images/chapter6_12.png)

* StartWith 通过传递一个参数来先发射一个数据序列

![StartWith](https://rxjava.yuxingxin.com/images/chapter6_13.png)

## Schedulers-解决Android主线程问题
---

### StrictMode

StrictMode帮助我们侦测敏感的活动，如我们无意的在主线程执行磁盘访问或者网络调用。正如你所知道的，在主线程执行繁重的或者长时的任务是不可取的。因为Android应用的主线程时UI线程，它被用来处理和UI相关的操作：这也是获得更平滑的动画体验和响应式App的唯一方法。

为了在我们的App中激活StrictMode，我们只需要在MainActivity中添加几行代码，即onCreate()方法中这样：

```java
@Override
public void onCreate() { 
    super.onCreate();
    if (BuildConfig.DEBUG) {
        StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder().detectAll().penaltyLog().build()); 
        StrictMode.setVmPolicy(new StrictMode.VmPolicy.Builder().detectAll().penaltyLog().build());
    } 
}
```

我们并不想它总是激活着，因此我们只在debug构建时使用。这种配置将报告每一种关于主线程用法的违规做法，并且这些做法都可能与内存泄露有关：Activities、BroadcastReceivers、Sqlite等对象。

选择了penaltyLog()，当违规做法发生时，StrictMode将会在logcat打印一条信息。

### Schedulers

调度器以一种最简单的方式将多线程用在你的Apps的中。它们时RxJava重要的一部分并能很好地与Observables协同工作。它们无需处理实现、同步、线程、平台限制、平台变化而可以提供一种灵活的方式来创建并发程序。

RxJava提供了5种调度器：

* `.io()`
* `.computation()`
* `.immediate()`
* `.newThread()`
* `.trampoline()`

让我们一个一个的来看下它们：

#### Schedulers.io()

这个调度器时用于I/O操作。它基于根据需要，增长或缩减来自适应的线程池。我们将使用它来修复我们之前看到的`StrictMode`违规做法。由于它专用于I/O操作，所以并不是RxJava的默认方法；正确的使用它是由开发者决定的。

重点需要注意的是线程池是无限制的，大量的I/O调度操作将创建许多个线程并占用内存。一如既往的是，我们需要在性能和简捷两者之间找到一个有效的平衡点。

#### Schedulers.computation()

这个是计算工作默认的调度器，它与I/O操作无关。它也是许多RxJava方法的默认调度器：`buffer()`,`debounce()`,`delay()`,`interval()`,`sample()`,`skip()`。

#### Schedulers.immediate()

这个调度器允许你立即在当前线程执行你指定的工作。它是`timeout()`,`timeInterval()`,以及`timestamp()`方法默认的调度器。

#### Schedulers.newThread()

这个调度器正如它所看起来的那样：它为指定任务启动一个新的线程。

#### Schedulers.trampoline()

当我们想在当前线程执行一个任务时，并不是立即，我们可以用`.trampoline()`将它入队。这个调度器将会处理它的队列并且按序运行队列中每一个任务。它是`repeat()`和`retry()`方法默认的调度器。

### 非阻塞I/O操作

现在我们可以使用Schedulers.io()创建非阻塞的版本：

```java
public static void storeBitmap(Context context, Bitmap bitmap, String filename) {
    Schedulers.io().createWorker().schedule(() -> {
        blockingStoreBitmap(context, bitmap, filename);
    }); 
}
```

![none block io](https://rxjava.yuxingxin.com/images/chapter7_1.png)

### SubscribeOn ObserveOn

`subscribeOn()`方法用`Scheduler`来作为参数并在这个Scheduler上执行Observable调用。

```java
getApps()
    .onBackpressureBuffer()
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Observer<AppInfo>() { [...]
```

`observeOn()`方法将会在指定的调度器上返回结果：如例子中的UI线程。`onBackpressureBuffer()`方法将告诉Observable发射的数据如果比观察者消费的数据要更快的话，它必须把它们存储在缓存中并提供一个合适的时间给它们。

## 与REST无缝结合-RxJava和Retrofit
---

### 项目目标

我们将在已有的例子中创建一个新的`Activity`。这个`Activity`将通过StackExchange API从stackoverflow检索出最活跃的10位用户。App使用这些信息来展示一个包含用户头像、姓名、名望数以及住址的列表。对每一位用户，app使用OpenWeatherMap API来检索该用户住址当地的天气预报，并显示一个小天气图标。基于从StackOverflow检索的信息，app对列表中的每一位用户提供一个`onClick`事件，打开他们在个人信息中设定的个人网站或者Stack Overflow的个人主页。

### Retrofit

Retrofit是Square公司专为Android和Java设计的一个类型安全的REST客户端。它帮助你轻松地与任意REST API交互，并完美兼容RxJava：所有的JSON响应对象都被映射成原始的Java对象，并且所有的网络调用都基于Rxjava Observable这些对象。

使用API文档，我们可以定义我们从服务器接收的JSON响应数据。为了很容易的将JSON响应数据映射为我们的Java代码，我们将使用[jsonschema2pojo][1], 这个服务将灵活地生成所有与JSON响应数据相映射的Java类。

当我们把所有的Java model准备好后，我们就可以开始建立Retrofit。Retrofi使用标准的Java接口来映射API路由。例如例子中，我们将使用来自API的一个路由，下面是我们Retrofit的接口:

```java 
public interface StackExchangeService {
    @GET("/2.2/users?order=desc&sort=reputation&site=stackoverflow")
    Observable<User sResponse> getMostPopularSOusers(@Query("pagesize") int howmany);
}
``` 

`interface`接口只包含一个方法，即`getMostPopularSOusers`。这个方法用整型`howmany`作为一个参数并返回`UserResponse`的Observable。

当我们有了`interface`，我们可以创建`RestAdapter`类，为了更清楚的组织我们的代码，我们创建一个`SeApiManager`函数提供一种更适当的方式来和StackExchange API交互。

```java 
public class SeApiManager {
    private final StackExchangeService mStackExchangeService;

    public SeApiManager() {
        RestAdapter restAdapter = new RestAdapter.Builder()
            .setEndpoint("https://api.stackexchange.com")
            .setLogLevel(RestAdapter.LogLevel.BASIC)
            .build();
        mStackExchangeService = restAdapter.create(StackExchangeService.class);
    }

    public Observable<List<User>> getMostPopularSOusers(int howmany) {
        return mStackExchangeService
            .getMostPopularSOusers(howmany)
            .map(UsersResponse::getUsers)
            .subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread());
    }
}
``` 

为了简化例子，我们不再将这个类设计为它本该设计成的单例。使用依赖注入解决方案，如Dagger2，可使代码质量更高。

创建`RestAdapter`类，需要对客户端API设置几个重要的方面。这个例子中，我们设置了`endpoint`和`log level`。由于这个例子中URL只是硬编码，像这样使用外部资源来存储数据很重要。避免在代码中硬编码字符串是一个好的实践。

Retrofit把`RestAdapter`类和我们的API接口绑定在一起后就完成了创建。它返回给我们一个对象用来请求API。我们可以选择直接暴露这个对象，或者以某种封装方式来限制对它的访问。在这个例子中，我们封装它并只暴露`getMostPopularSOusers`方法。这个方法执行查询，使用Retrofit解析JSON响应数据。获得用户列表，并返回给订阅者。如你所见，使用Retrofit、RxJava和Retrolambda，我们几乎没有模板代码：它非常紧凑而且可读性很高。

现在，我们已经有一个API管理者来提供一个响应式的方法，它从远程API获取数据并给I/O调度器，解析映射最后为我们的消费者提供一个简洁的用户列表。

### 创建Activity类

我们将在`onCreate()`方法里创建`SwipeRefreshLayout`和`RecyclerView`；我们有一个`refreshList()`方法来处理用户列表的获取和展示，`showRefreshing()`方法来管理进度条和`RecyclerView`的显示。

我们的`refreshList()`函数看起来如下：

```java
private void refreshList() { 
    showRefresh(true);
    mSeApiManager.getMostPopularSOusers(10)
        .subscribe(users -> { 
            showRefresh(false);
            mAdapter.updateUsers(users);
        }, error -> { 
            App.L.error(error.toString());
            showRefresh(false);
        });
}
```

我们显示了进度条，从StackExchange API 管理器观测用户列表。一旦获取到列表数据，我们开始展示它并更新`Adapter`的内容并让`RecyclerView`显示为可见。


### 创建RecyclerView Adapter

我们从REST API获取到数据后，我们需要把它绑定View上，并用一个适配器填充列表。我们的RecyclerView适配器是标准的。它继承于`RecyclerView.Adapter`并指定它自己的`ViewHolder`：

```java
public static class ViewHolder extends RecyclerView.ViewHolder {
    @InjectView(R.id.name) TextView name;
    @InjectView(R.id.city) TextView city;
    @InjectView(R.id.reputation) TextView reputation;
    @InjectView(R.id.user_image) ImageView user_image;
    public ViewHolder(View view) { 
        super(view);
        ButterKnife.inject(this, view); 
    }
}
```

我们一旦收到来自API管理器的数据，我们可以设置界面上所有的标签：`name`,`city`和`reputation`。

为了展示用户的头像，我们将使用Sergey Tarasevich写的[Universal Image Loader][2]。实践证明，UIL是非常有名的好用的图片管理库。我们也可以使用Square公司的Picasso，Glide或者Facebook公司的Fresco。取决于你自己的喜好。最关键的是无需重复造轮子：库能够方便开发者并让他们更快速实现目标。

在我们的适配器中，我们可以这样：

```java
@Override
public void onBindViewHolder(SoAdapter.ViewHolder holder, int position) {
    User user = mUsers.get(position);
    holder.setUser(user); 
}
```

在`ViewHolder`，我们可以这样：

```java
public void setUser(User user) { 
    name.setText(user.getDisplayName());
    city.setText(user.getLocation());
    reputation.setText(String.valueOf(user.getReputation()));
    
    ImageLoader.getInstance().displayImage(user.getProfileImage(), user_image);
}
```

此时，我们可以允许代码获得一个用户列表，正如下图所示：

![StackOverFlow](http://iosdevlog.com/assets/images/Rx/RxJava/RxJavaEssentials/StackOverFlow.png)

### 检索天气预报

我们加大难度，将当地城市的天气加入列表中。**OpenWeatherMap**是一个灵活公共在线天气API，我们可以查询许多有用的预报信息。

和往常一样，我们将使用Retrofit映射到API然后通过RxJava来访问它。至于StackExchange API，我们将创建`interface`，`RestAdapter`和一个灵活的管理器：

```java
public interface OpenWeatherMapService {
    @GET("data2.5/weather")
    Observable<WeatherResponse> getForecastByCity(@Query("q") String city);
}
```

这个方法用城市名字作为参数提供当地的预报信息。我们像下面这样将接口和`RestAdapter`类绑定在一起：

```java
RestAdapter restAdapter = new RestAdapter.Builder()
        .setEndpoint("http://api.openweathermap.org")
        .setLogLevel(RestAdapter.LogLevel.BASIC)
        .build();
mOpenWeatherMapService = restAdapter.create(OpenWeatherMapService.class);
```

像以前一样，我们只有两件事需要立马去做：设置API端口和log级别。

`OpenWeatherMapApiManager`类将提供下面的方法：

```java
public Observable<WeatherResponse> getForecastByCity(String city) {
    return mOpenWeatherMapService.getForecastByCity(city)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread());
}
```

现在，我们有了用户列表，我们可以根据城市名来查询OpenWeatherMap获得天气预报信息。下一步是修改我们的`ViewHolder`类来为每位用户展示相应的天气图标。

我们使用这些工具方法先验证用户主页信息并获得一个合法的城市名字：

```java
private boolean isCityValid(String location) {
    int separatorPosition = getSeparatorPosition(location);
    return !"".equals(location) && separatorPosition > -1; 
}

private int getSeparatorPosition(String location) { 
    int separatorPosition = -1;
    if (location != null) {
        separatorPosition = location.indexOf(","); 
    }
    return separatorPosition; 
}

private String getCity(String location, int position) {
    if (location != null) {
        return location.substring(0, position); 
    } else {
        return ""; 
    }
}
```

借助一个有效的城市名，我们可以用下面命令来获得我们所需要天气的所有数据：

```java
OpenWeatherMapApiManager.getInstance().getForecastByCity(city)
```

用天气响应的结果，我们可以获得天气图标的URL：

```java
getWeatherIconUrl(weatherResponse);
```

用图标URL，我们可以检索到图标本身：

```java
private Observable<Bitmap> loadBitmap(String url) {
    return Observable.create(subscriber -> {
        ImageLoader.getInstance().displayImage(url,city_image, new ImageLoadingListener() { 
            @Override
            public void onLoadingStarted(String imageUri, View view) {
            }
            
            @Override
            public void onLoadingFailed(String imageUri, View view, FailReason failReason) {
                subscriber.onError(failReason.getCause()); 
            }
            
            @Override
            public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
                subscriber.onNext(loadedImage);
                subscriber.onCompleted(); 
            }
            
            @Override
            public void onLoadingCancelled(String imageUri, View view) {
                subscriber.onError(new Throwable("Image loading cancelled"));
            }
         });
    });
}
```

这个`loadBitmap()`返回的Observable可以链接前面一个，并且最后我们可以为这个任务返回一个单独的Observable：

```java
if (isCityValid(location)) {
    String city = getCity(location, separatorPosition);
    OpenWeatherMapApiManager.getInstance().getForecastByCity(city)
        .filter(response -> response != null)
        .filter(response -> response.getWeather().size() > 0)
        .flatMap(response -> {
            String url = getWeatherIconUrl(response);
            return loadBitmap(url);
        })
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Observer<Bitmap>() {
    
        @Override
        public void onCompleted() {
        }
        
        @Override
        public void onError(Throwable e) {
            App.L.error(e.toString()); 
        }
        
        @Override
        public void onNext(Bitmap icon) {
            city_image.setImageBitmap(icon); 
        }
    });
}
```

运行代码，我们可以在下面列表中为每个用户获得新的天气图标：

![StackOverFlowWeather](http://iosdevlog.com/assets/images/Rx/RxJava/RxJavaEssentials/StackOverFlowWeather.png)

### 打开网站

使用用户主页包含的信息，我们将会创建一个`onClick`监听器来导航到用户web页面，如果有的话，否则打开在Stack Overflow上的个人主页。

为了实现它，我们简单实现`Activity`类的接口，用来在适配器触发Android的`onClick`事件。

我们的`Adapter ViewHolder`指定这个接口：

```java
public interface OpenProfileListener {
    public void open(String url); 
}
```

`Activity`实现它：

```java
[...] implements SoAdapter.ViewHolder.OpenProfileListener { [...]
    mAdapter.setOpenProfileListener(this); 
[...]

@Override
public void open(String url) {
    Intent i = new Intent(Intent.ACTION_VIEW);
    i.setData(Uri.parse(url)); 
    startActivity(i);
}
```

`Activity`收到URL并用外部Android浏览器打开它。我们的`ViewHolder`负责在用户列表的每个卡片上创建`OnClickListener`并检查我们是打开Stack Overflow用户主页还是外部个人站：

```java
mView.setOnClickListener(view -> { 
    if (mProfileListener != null) {
        String url = user.getWebsiteUrl();
        if (url != null && !url.equals("") && !url.contains("search")) {
            mProfileListener.open(url); 
        } else {
            mProfileListener.open(user.getLink()); 
        }
    }
)};
```

一旦我们点击了，我们将直接重定向到预期的网站。在Android上，我们可以用RxAndroid的一种特殊形式（ViewObservable）以更加响应式的方式实现同样的结果。

```java
ViewObservable.clicks(mView)
    .subscribe(onClickEvent -> {
    if (mProfileListener != null) {
        String url = user.getWebsiteUrl();
        if (url != null && !url.equals("") && !url.contains("search")) { 
            mProfileListener.open(url);
        } else {
            mProfileListener.open(user.getLink());
        } 
    }
});
```

上面两块代码片段是等价的，你可以选择最喜欢的方式来实现。


[1]: http://www.jsonschema2pojo.org "官网"

[2]: https://github.com/nostra13/Android-Universal-Image-Loader
