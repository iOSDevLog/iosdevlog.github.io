---
layout: post
title: "《Asynchronous Android Programming - Second Edition》读书笔记"
author: iosdevlog
date: 2017-12-25 16:10:11 +0800
description: ""
category: 读书笔记
tags: [Android]
---

# 源码：<https://github.com/PacktPublishing/Asynchronous-Android-Programming>
---

# 第1章 Asynchronous Programming in Android Android 里的异步编程
---

## Android software stack

## Dalvik runtime

## ART runtime

## Memory sharing and Zygote

zygote这个词的中文意思是“受精卵”。

Zygote本身是一个Native的应用程序，和驱动、内核等均无关系。

* First, the virtual machine and core libraries are already loaded into the memory. Not having to read this significant chunk of data from the filesystem to initialize the virtual machine drastically reduces the startup overhead.
* Second, the memory in which these core libraries and common structures reside is shared by the Zygote with all other applications, resulting in saving a lot of memory when the user is running multiple apps.

## Android process model

## Process ranks


* Foreground process: This is a process that hosts an activity or service that the user is currently interacting with: a service started in the foreground or service running its life cycle callbacks
* Visible process: This is a process that hosts a paused activity or service bounded to a visible activity
* Service process: This is a process that hosts a service not bound to a visible activity
* Background process: This is a process that hosts a non-visible activity; all background processes are sorted over a Least-Recently-Used (LRU) list, therefore, the most recently used processes are the last killed processes when they have the same rank
* Empty process: This is a process used to cache inactive Android components and to improve any component startup time

## Process sandboxing

Linux user ID (UID) 

## Android thread model

## The main thread

The main thread, also known as UI Thread, is also the thread where your UI event handling occurs, so to keep your application as responsible as possible, you should:

* Avoid any kind of long execution task, such as input/output (I/O) that could block the processing for an indefinite amount of time
* Avoid CPU-intensive tasks that could make this thread occupied for a long time

![Looper](/assets/images/Rx/RxJava/asynchronousAndroidProgramming-SecondEdition/Looper.png)

> Since the Android 5.0 (Lollipop), a new important thread named RenderThread was introduced to keep the UI animations smooth even when the main thread is occupied doing stuff.

## The Application Not Responding (ANR) dialog

## Maintaining responsiveness

* Network communications
* Input and output file operations on the local filesystem
* Image and video processing
* Complex math calculations
* Text processing
* Data encoding and decoding

![async](/assets/images/Rx/RxJava/AsynchronousAndroidProgramming-SecondEdition/Async.png)

## Concurrency in Android

The Android SDK, as it is based on a subset of Java SDK, derived from the Apache Harmony project, provides access to low-level concurrency constructs such as `java.lang.Thread`, `java.lang.Runnable`, and the `synchronized` and `volatile` keywords.

### Thread

```java
public class MyThread extends Thread {
    public void run() {
        Log.d("Generic", "My Android Thread is running ...");
    }
}
 ```

 ```java
 MyThread myThread = new MyThread();
 myTread.start();
 ```

Other helpful thread-related methods include the following:

* *Thread.currentThread()*: This retrieves the current running instance of the thread
* `Thread.sleep(time)`: This pauses the current thread from execution for the given period of time
* `Thread.getName() and Thread.getId()`: These get the name and TID, respectively so that they can be useful for debugging purposes
* `Thread.isAlive()`: This checks whether the thread is currently running or it has already finished its job
* `Thread.join()`: This blocks the current thread and waits until the accessed thread finishes its execution or dies

### Runnable

```java
package java.lang;
public interface Runnable {
    public abstract void run();
}
```

```java
public class MyRunnable implements Runnable {
    public void run(){
        Log.d("Generic","Running in the Thread " +
        // Do your work here
        ...
    } 
}
```

```java
Thread thread = new Thread(new MyRunnable());
thread.start();
```

## Correctness issues in concurrent programs

```java
int myInt = 2;
...
public class MyThread extends Thread {
    public void run() {
        super.run();
        myInt++; 
    }
}
...
Thread t1 = new MyThread();
Thread t2 = new MyThread();
t1.start();
t2.start();
```

This kind of timing issue is known as a race condition.

这种时序问题就是众所周知的*竞争条件*/*竞态条件*。

To achieve this correctness, we can make use of the synchronized construct to solve the correctness issue on the following piece of code:

我们可以使用`synchronized`构造。

```java
Object lock = new Object();
public class MyThread extends Thread {
    public void run() {
        super.run();
        synchronized(lock) {
            myInt++;
        }
    }
}
```

Another way to create mutually exclusive scopes is to create a method with a synchronized method:

另一种方法是`synchronized`方法。

```java
int myInt = 2;
synchronized void increment() {
    myInt++; 
}
...
public class IncrementThread extends Thread {
    public void run() {
        super.run();
        increment();
    }
}
```

## Liveness issues in concurrent programs

## Thread coordination 线程协调

1. Synchronize access of threads to shared resources or shared memory:

    a. Shared database, files, system services, instance/class variables, or queues
1. Coordinate work and execution within a group of threads:

    a. Parallel execution, pipeline executions, inter-dependent tasks, and so on

```java
while(!readyToProcess) {
     // do nothing .. busy waiting wastes processor time.
}
```

In Java, every object has the `wait()`, `notify()`, and `notifyAll()`methods that provide low-level mechanisms to send thread signals between a group of threads and put a thread in a waiting state until a condition is met.

## Concurrent package constructs

Other Java concurrent constructs provided by java.util.concurrent, which are also available on Android SDK are as follows:


* **Lock objects (java.util.concurrent)**: They implement locking behaviors with a higher level idiom.
* **Executors**: These are high-level APIs to launch and manage a group of thread executions (ThreadPool, and so on).
* **Concurrent collections**: These are the collections where the methods that change the collection are protected from synchronization issues.
* **Synchronizers**: These are high-level constructs that coordinate and control thread execution (Semaphore, Cyclic Barrier, and so on).
* **Atomic variables (java.util.concurrent.atomic)**: These are classes that provide thread-safe operations on single variables. One example of it is AtomicInteger that could be used in our example to solve the correctness issue.

## Executor framework

The Executor framework is another framework available on java.util. concurrent that provides an interface to submit Runnable tasks, decoupling the task submission from the way the task will run:

```java
public interface Executor {
    void execute(Runnable command);
}
```

* **Executors.newCachedThreadPool()**: This is a thread poll that could grow and reuse previously created threads
* **Executors.newFixedThreadPool (nThreads)**: This is a thread pool with a fixed number of threads and a message queue for store work
* **Executors.newSingleThreadPool()**: This is similar to newFixedThreadPool, but with only one working thread

```java
public class MyRunnable implements Runnable {
    public void run() {
        Log.d("Generic", "Running From Thread " + Thread.currentThread().getId());
        logOnConsole("Running From Thread " + Thread.currentThread().getId());
        // Your Long Running Computation Task
        try {
            // Sleeps for 200 ms
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public void startWorking() {
    Executor executor = Executors.newFixedThreadPool(5);
    for ( int i=0; i < 20; i++ ) {
        executor.execute(new MyRunnable());
    }
}
```

## Android primary building blocks

four main building blocks 四大组件

* android.app.Activity
* android.app.Service
* android.content.BroadcastReceiver
* android.content.ContentProvider

The `Activity`, `Service`, and `BroadcastReceiver` are activated explicitly or implicitly over an asynchronous message called Intent.

激活`活动`、`服务`、`广播接收者`要显示或者隐示调用异步**意图**。

## Activity concurrent issues

The most important callbacks on the Activity lifecycle are as follows:

* **onCreate()**: At this state, Activity is not visible, but it is here where all the private Activity resources (views and data) are created. The long and intensive computations should be done asynchronously in order to decrease the time when the users don't get a visual feedback during an Activity transition.
* **onStart()**: This is the callback called when the UI is visible, but not able to interact on the screen. Any lag here could make the user angry as any touch event generated at this stage is going to be missed by the system.
* **onResume()**: This is the callback called when Activity is going to be in the foreground and at an interactable state.
* **onPause()**: This is a callback called when Activity is going to the background and is not visible. Computations should end quickly as the next Activity will not resume until this method ends.
* **onStop()**: This is a callback called when Activity is no longer visible, but can be restarted.
* **onDestroy()**: This is a callback called when the Activity instance is going to be destroyed in the background. All the resources and references that belong to this instance have to be released.

## Manipulating the user interface

> You cannot manipulate the user interface from any thread other than the main thread.

If the system detects this, it will instantly notify the application by throwing `CalledFromWrongThreadException`.

If the developer has access to an Activity instance, the `runOnUiThread` instance method can be used to update the UI from a background thread.

The method accepts a Runnable object like the one used to create an execution task for a thread:

```java
public final void runOnUiThread (Runnable)
```

## Service concurrent issues 服务并发问题

Service, by default, runs in the **main thread** of the application process. It does not create its own thread, so if your Service is going to do any blocking operation, such as downloading an image, play a video, or access a network API, the user should design a strategy to of oad the time of the work from the main thread into another thread.

* **Started services**: This is the service that is started by startService() that can run definitively even if the component that started it was destroyed. A started service does not interact directly with the component that started it.
* **Bound services**: This service exists while at least one Android component is bounded to it by calling bindService(). It provides a two-way (client-server) communication channel for communication between components.

## Started services issues

## Bound services issues

## Service in a separate proces

To implement a service in a different process, we need to use an **inter-process communication (IPC)** technique to send messages between your application and the service.

There are two technologies available on the Android SDK to implement this, as follows:
* **AIDL (Android Interface Definition Language)**: This allows you to define an interface over a set of primitive types. It allows you create multithreaded processing services, but it adds other levels of complexity to your implementation. This is only recommended to advanced programmers.
* **Messenger**: This is a simple interface that creates a queue of work for you in the service side. This executes all the tasks sequentially on single thread managed by a Handler.

## Broadcast receiver concurrent issues 广播接收者并发问题

## Android concurrency constructs Android 并发构造

## Summary

In this chapter, we took a detailed look at the available Android runtimes, Android processes, and thread models.

We then introduced the concurrent issues that we would cope with when we try to implement robust concurrent programs.

Finally, we listed the basic concurrent building blocks available on the SDK to design concurrent programs.

In the next chapter, we'll take a look at some Android-speci c low-level building blocks on which the other concurrency mechanisms are built: Handler, Looper, and LooperThread.

# 第2章 Performing Work with Looper, Handler, and HandlerThread
---

* Understanding Looper
* Understanding Handler
* Sending work to Looper
* Scheduling work with post
* Using Handler to defer work
* Leaking implicit references
* Leaking explicit references
* Updating the UI with Handler
* Canceling pending messages
* Multithreading with Handler and HandlerThread
* Applications of Handler and HandlerThread

## Understanding Looper

> A loop is a group of instructions that are repeated continually until a termination condition is met.

Following this de nition, Android's `Looper` executes on a thread that has a `MessageQueue`, executes a continuous loop waiting for work, and blocks when there is no work pending. When work is submitted to its queue, it dispatches it to the target `Handler` de ned explicitly on the `Message` object.

The Looper sequence on Android follows these steps:

1. Wait until a Message is retrieved from its MessageQueue
2. If logging is enabled, print dispatch information
3. Dispatch the message to the target Handler
4. Recycle the Message
5. Go to step 1

获取主线程 Looper。

```java
Looper mainLooper = Looper.getMainLooper();
```

To set up our own Looper thread, we need to invoke two static methods of Looper— prepare and loop—from within the thread, and they will handle the continuous loop. Here is a simple example:

为了设置我们自己线程的 Looper，我们需要调用 2 个 Looper 的静态方法 `prepare` 和 `loop`。 它们会持有连续循环。

```java
class SimpleLooper extends Thread {
    public void run() {
        // Attach a Looper to the current Thread
        Looper.prepare();
        // Start the message processing
        Looper.loop();
    }
}
```

> `Looper.prepare()` must only be called once from within the same thread; otherwise, a RuntimeException will to be thrown that says only one looper may be created per thread.

## Understanding Handler

```java
public class SimpleLooper extends Thread {
    private Handler myHandler;

    @Override
    public void run() {
        Looper.prepare();
        myHandler  =  new MyHandler();
        Looper.loop();
    }

    public Handler getHandler() {
        return myHandler;
    }
}
```

```java
public class MyHandler extends Handler {
    @Override
    public void handleMessage(Message msg) {
        // Add here your message handling
        // processing
    } 
}
```

![handle](/assets/images/Rx/RxJava/AsynchronousAndroidProgramming-SecondEdition/Handle.png)

```java
public class StackTraceHandler extends Handler {
    @Override
    public void handleMessage(Message msg) {
        // Prints the Stack Trace on the Android Log
        Thread.currentThread().dumpStack();
    }
}
```

> If the current thread does not have a Looper and we try to create a handler over the super constructor, a runtime exception with the message **Can't create handler inside thread that has not called Looper.prepare()** is thrown.

```java
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    ...
    Handler handler = new StackTraceHandler();
    Message msg = handler.obtainMessage();
    handler.sendMessage(msg);
}
```

## Sending work to a Looper

```java
public class StackTraceHandler extends Handler {
    StackTraceHandler(Looper looper){
    super(looper);
}
```

## Scheduling work with post

![runnableComposition](/assets/images/Rx/RxJava/AsynchronousAndroidProgramming-SecondEdition/RunnableComposition.png)

## Using Handler to defer work

## Leaking implicit references

## Leaking explicit references

## Updating the UI with Handler

## Canceling a pending Runnable

## Scheduling work with send

## Cancelling pending messages

## Composition versus inheritance

## Multithreading with Handler and ThreadHandler

![threadPriority](/assets/images/Rx/RxJava/AsynchronousAndroidProgramming-SecondEdition/ThreadPriority.png)

## Looper message dispatching debugging

## Sending messages versus posting runnables

## Applications of Handler and HandlerThread

## Summary

In this chapter, we learned how to use Handler to queue work for the main thread and how to use Looper to build up a queueing infrastructure for our own Thread.

We saw the different ways in which we can de ne work with Handler: arbitrary work de ned at the call site with Runnable or prede ned work implemented in the Handler itself and triggered by message-sending.

In the meantime, we learned how to defer work properly without leaking memory on the way.

We learned how to use Handler in a multithreaded application to pass work and results back and forth between cooperating threads, performing blocking operations on an ordinary background thread and communicating the results back to the main thread to update the user interface.

In the next chapter, we'll start to build responsive applications by applying the AsyncTask instance to execute work in the background using pools of threads and returning progress updates and results to the main thread.
