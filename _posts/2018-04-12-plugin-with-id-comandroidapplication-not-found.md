---
layout: post
title: "Plugin with id 'com.android.application' not found."
author: iosdevlog
date: 2018-04-12 16:29:13 +0800
description: ""
category: android
tags: []
---

从 github 上面下载的 Android 项目，里面没有 `gradlew` 这个文件，也就是说没有使用 *gradle wrapper*，所有我们要使用 `gradle` 命令运行。

```bash
$ gradle tasks
...
> Task :tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Android tasks
-------------
androidDependencies - Displays the Android dependencies of the project.
signingReport - Displays the signing info for each variant.
sourceSets - Prints out all the source sets defined in this project.

Build tasks
-----------
assemble - Assembles all variants of all applications and secondary packages.
assembleAndroidTest - Assembles all the Test applications.
assembleDebug - Assembles all Debug builds.
assembleRelease - Assembles all Release builds.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
clean - Deletes the build directory.
cleanBuildCache - Deletes the build cache directory.
compileDebugAndroidTestSources
compileDebugSources
compileDebugUnitTestSources
compileReleaseSources
compileReleaseUnitTestSources
mockableAndroidJar - Creates a version of android.jar that's suitable for unit tests.

Build Setup tasks
-----------------
init - Initializes a new Gradle build.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'MP3Player3_2_4'.
components - Displays the components produced by root project 'MP3Player3_2_4'. [incubating]
dependencies - Displays all dependencies declared in root project 'MP3Player3_2_4'.
dependencyInsight - Displays the insight into a specific dependency in root project 'MP3Player3_2_4'.
dependentComponents - Displays the dependent components of components in root project 'MP3Player3_2_4'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'MP3Player3_2_4'. [incubating]
projects - Displays the sub-projects of root project 'MP3Player3_2_4'.
properties - Displays the properties of root project 'MP3Player3_2_4'.
tasks - Displays the tasks runnable from root project 'MP3Player3_2_4'.

Install tasks
-------------
installDebug - Installs the Debug build.
installDebugAndroidTest - Installs the android (on device) tests for the Debug build.
uninstallAll - Uninstall all applications.
uninstallDebug - Uninstalls the Debug build.
uninstallDebugAndroidTest - Uninstalls the android (on device) tests for the Debug build.
uninstallRelease - Uninstalls the Release build.

Verification tasks
------------------
check - Runs all checks.
connectedAndroidTest - Installs and runs instrumentation tests for all flavors on connected devices.
connectedCheck - Runs all device checks on currently connected devices.
connectedDebugAndroidTest - Installs and runs the tests for debug on connected devices.
deviceAndroidTest - Installs and runs instrumentation tests using all Device Providers.
deviceCheck - Runs all device checks using Device Providers and Test Servers.
lint - Runs lint on all variants.
lintDebug - Runs lint on the Debug build.
lintRelease - Runs lint on the Release build.
lintVitalRelease - Runs lint on just the fatal issues in the release build.
test - Run unit tests for all variants.
testDebugUnitTest - Run unit tests for the debug build.
testReleaseUnitTest - Run unit tests for the release build.
...
```

 可能项目里面只有根目录下面有 `build.gradle` 文件，导致

```
Plugin with id 'com.android.application' not found.
```

需要在 `build.gradle` 文件下面添加

```
apply plugin: 'com.android.application'

// 添加在第1行下面
buildscript{
    repositories{
        jcenter()
        google()
    }

    dependencies{
        classpath 'com.android.tools.build:gradle:3.1.1'
    }
}
...
```

最后可以使用

```bash
$ gradle installDebug
```

安装运行。