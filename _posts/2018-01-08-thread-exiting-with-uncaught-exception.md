---
layout: post
title: "thread exiting with uncaught exception"
author: iosdevlog
date: 2018-01-08 18:05:24 +0800
description: ""
category: 
tags: []
---

Sometimes，you may not catch the Exception with Try-Catch(eh,you may google the reason ).But here I will suggest you one way may solve your case.The Thread class has a function setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh) to process uncaught exception.

* Make your Activity to implement UncaughtExceptionHandler

```java
public class MainActivity extends Activity implements UncaughtExceptionHandler {
    ....
}
```

* Let your Activity override the uncaughtException

```java
public void uncaughtException(Thread arg0, Throwable arg1) {

        //here to process the uncaught exception

}
```

* In your Activity's OnCreate

Just Call Thread.setDefaultUncaughtExceptionHandler(this); to let it works.

I hope this can do some help .



<https://stackoverflow.com/questions/33603849/thread-exiting-with-uncaught-exception-android>
