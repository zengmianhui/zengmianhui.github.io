---
title: 理解Android Activity运行模式（understand Android Activity's launch mode）
date: 2017-03-22 23:26:54
categories:
  - android
  - 四大组件
  - activity
tags:
  - android
  - activity launchMode
---
## 文章参考自
[Understand Android Activity's LaunchMode:standard,singleTop,singleTask,and singleInstance](https://inthecheesefactory.com/blog/understand-android-activity-launchmode/en)

[Activity启动模式及Intent Flags与栈的关联分析](http://blog.csdn.net/vipzjyno1/article/details/25463457)

# 理解Android Activity运行模式（understand Android Activity's launch mode）
## Activity's LaunchMode
activity的启动模式，设置了不同的启动模式activity会不同的启动方式，一般使用Intent中的Flag常量来标示如何启动一个Activity
<!--more-->
## understand Tasks and Back Stack(任务队列和回退栈)
### Task
一个Task是由多个Activity组成，一个应用程序可以有多个任务队列，在Android7.0的新特性——多窗口模式，每一个窗口都会创建一个Task

### Back Stack
Task中的多个Activity被安排在Back Stack中
### 可以通过Activity在app中的状态来理解Task和Stack
1、 在Task中，当Activity A启动Activity B时，当前A会调用onPause()-onStop()，A不在栈顶，没有显示在最前，但是A会保存一些基本的状态（例如滚动的位置或者text），Task状态为A-B，如果用户点击navigation back button时,Activity B会调用onPause()-onStop()-onDestroy()对栈顶进行pop（移出并销毁）,此时Activity A回到栈顶，调用onRestart()-onStart()-onResume()，恢复原来的状态

2、当用点击Home Screen时，栈顶Activity会执行onPause()-onStop()，其他Activity之前置于栈下面时，就已经调用了onPause()-onStop(),所以所有activity都看不见了，这个过程，Task进入了后台运行
4、Activity可以在不同时间、不同Task中实例化多次
## type of LaunchMode
### standard
这个模式是默认的Activity的启动模式，每次启动Activity都会创建一个新实例，不会考虑之前实例了多少个这样的Activity

![standard的activity启动时，task的状态](理解Android Activity运行模式（understand Android Activity's launch mode）/diagram_backstack.png)
### singleTop
这个模式下当前activity如果在栈顶，也就是正在前景，如果调用startActivity，启动当前activity，则会取出栈顶的activity。
### SingleTask
这个模式下，如果当需要打一个activity A，这个A已经在当前Task存在了，此模式会pop在A之上的所有Activity，使A处栈顶，处于前景，系统会调用onNewIntent(),而不创建新实例。

如果当前Task为A-B-C-D，则从D-B，B不会被重新创建，而是直接恢复引用，置于foreground
以下是activity B的生命周期打印结果，可见，B是不会被重新创建的
>03-23 13:37:45.258 304-304/com.app.androidbasic I/ActivityB: onRestart: 调用了onRestart
>
>03-23 13:37:45.258 304-304/com.app.androidbasic I/ActivityB: onStart: 调用onstart()
>
>03-23 13:37:45.258 304-304/com.app.androidbasic I/ActivityB: onResume: 调用了onResume

![singleTask](理解Android Activity运行模式（understand Android Activity's launch mode）/2017-03-23_105452.png)

![singleTask](理解Android Activity运行模式（understand Android Activity's launch mode）/singleTask.gif)
### SingleInstance
Task1中A-B-C，当C启动D时，运用此模式下，会将D压入一个新Task中，Task2中D,此时D显示在屏幕上，当D启动C时，模式为standard时，那么会回Task1中，并创建一个新实例C，此时Task1中的结构为A-B-C-C，点击navigation back不会回到D

![singleInstance](理解Android Activity运行模式（understand Android Activity's launch mode）/singleInstance.png)
### launchMode and Flag

Manifest.xml中定义 | 同等于Intent.FLAG_ACTIVITY_*常量
---|---
standard | 默认
singleTop | FLAG_ACTIVITY_SINGLE_TOP
singleTask|singleTop结合FLAG_ACTIVITY_CLEAR_TOP
singleInstance|FLAG_ACTIVITY_NEW_TASK
### 详细解释Intent.FLAG
#### FLAG_ACTIVITY_NEW_TASK
这个flag必须与taskAffinity结合才能使用

#### FLAG_ACTIVITY_CLEAR_TOP
请注意和singleTask的区别，与singleTask的区别，FLAG_ACTIVITY_CLEAR_TOP会销毁启动的目标activity，再重新创建一个实例
如果当前Task为A-B-C-D，则从D-B，B会被销毁重新创建

以下为打印的结果，可以得知，B被重新创建
>03-23 13:23:01.858 19000-19000/com.app.androidbasic I/ActivityB: onDestroy: 调用onDestroy

>03-23 13:23:01.888 19000-19000/com.app.androidbasic I/ActivityB: onCreate: 调用onCreate，获取当前Task的id6

>03-23 13:23:01.888 19000-19000/com.app.androidbasic I/ActivityB: onStart: 调用onstart()

>03-23 13:23:01.888 19000-19000/com.app.androidbasic I/ActivityB: onResume: 调用了onResume

此标志可以与singleTop结合，实现与singleTask一样的效果，但是一般不使用，因为还不如直接使用singleTask
#### FLAG_ACTIVITY_BROUGHT_TO_FRONT
>This flag is not normally set by application code, but set for you by the system as described in the launchMode documentation for the singleTask mode.
这个flag通常不是由我们来定义的，但是设置了singleTask mode系统会自动为我们设置这个标志

#### FLAG_ACTIVITY_CLEAR_TASK
>If set in an Intent passed to Context.startActivity(), this flag will cause any existing task that would be associated with the activity to be cleared before the activity is started. That is, the activity becomes the new root of an otherwise empty task, and any old activities are finished. This can only be used in conjunction with FLAG_ACTIVITY_NEW_TASK.
>
>如果设置了这个flag，将会导致已在的task与之关联的activity被清除，这个flag只能与FLAG_ACTIVITY_NEW_TASK结合使用