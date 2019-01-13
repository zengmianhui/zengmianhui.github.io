---
title: broadcast与broadcast receiver
date: 2017-04-08 22:57:07
categories:
  - android
  - 四大组件
  - broadcast
tags:
  - android
  - android broadcast
---
# broadcast与broadcast receiver
我们需要注意，广播与广播接收者的区别
## 广播接收者的注册
一般面试官会问，广播的注册方式有几种，最好能解释一下，广播注册指的是广播接收者的注册。

### 静态注册
**也就是在AndroidManifests.xml中进行注册**
```
<receiver android:name=".MyReceiver">
    <intent-filter android:priority="20">
        <action android:name="org.crazyit.action.CRAZY_BROADCAST"/>
    </intent-filter>
</receiver>
```
这种方法的优点，在于**应用退出后也能接收广播**
### 动态注册  
动态注册是在程序运行中注册，**只能在应用运行时进行广播接收。**

>Intent registerReceiver (BroadcastReceiver receiver, IntentFilter filter)  注册广播接收者

>Intent registerReceiver (BroadcastReceiver receiver, IntentFilter filter,  String broadcastPermission,  Handler scheduler)  
broadcastPermission：命名一个权限，使广播必须持有这个权限，才能被这个Receiver接收  
scheduler：handler识别线程将接收intent，如果为空，则将在main thread中接收intent

>unregisterReceiver(BroadcastReceiver)取消广播接收者

<!--more-->
## 普通广播和有序广播  
>Context.sendBroadcast(Intent)：发送普通广播  

>Context.sendOrderedBroadcast(Intent):发送有序广播  

### 普通广播(Normal Broadcast)
普通广播是完全异步的，它可以在同一时刻被所有接收者接收到，但是缺点是无法将处理结果传递给下一个接收者，并且无法中断broadcast的传播。  

### 有序广播(Ordered Broadcast)
有序广播，故名思意，就是有顺序的广播，Ordered Broadcast的接收者将**按预先声明的优先级依次接收Broadcast**。比如，A>B>C，Broadcast会优先传给A，再到B，最后到C。优先级别可以使用`android:priority=""`申明在AndroidManifests.xml中，`优先级在-1000~1000之间，`例：  
```
<receiver android:name=".MyReceiver">
    <intent-filter android:priority="20">
        <action android:name="org.crazyit.action.CRAZY_BROADCAST"/>
    </intent-filter>
</receiver>
```
也可以在src中调用`IntentFilter.setPriority()`设置优先级别。  
**Ordered Broadcast的高优先级别的Receiver，可以将处理结果传递给一位Receiver，或者可以中止Broadcast的传播。** 
>BroadcastReceiver.abortBroadcast()中止广播  

>BroadcastReceiver.setResultExtras(Bundle)：将处理结果存入Broadcast，然后传递给下一个Receiver

>Bundle bundle=getResultExtras(true)可以获取上一个Receiver存入的数据

## 疑问
### Receiver的onReceive()是在子线程中吗？
onReceive()是主线程中运行的,最大支持耗时是10秒，超过可能会触发ANR，在onReceive()中添加了睡眠方法，然后用另一个按钮触发事件，可以看出，onReceive()方法执行完成后才触发了另一个按钮的事件。  
![发送广播后测试是否在主线程](broadcast与broadcast receiver/01.gif)  
虽然也有可能是线程锁的，但是打印结果是在同一线程内
```
04-08 14:23:45.071 2306-2306/com.app.demobroadcast I/MainActivity: onCreate: 获取当前进程名com.app.demobroadcast
04-08 14:23:45.071 2306-2306/com.app.demobroadcast I/MainActivity: onCreate: 获取当前线程名main
04-08 14:23:45.071 2306-2306/com.app.demobroadcast I/MyReceiver: onReceive: 获取当前进程名com.app.demobroadcast
04-08 14:23:45.071 2306-2306/com.app.demobroadcast I/MyReceiver: onReceive: 获取当前线程名main
```
### Broadcast是同步的？
Broadcast是完全异步的，要区别Broadcast与BroadcastReceiver，Broadcast的发送过程是异步，BroadcastReceiver是接收Broadcast的是同步执行的，在main线程中。
### Broadcast是启动activity？
如果你直接使用以下代码肯定会报错
```
    @Override
    public void onReceive(Context context, Intent intent) {
        Intent i=new Intent(context,MainActivity.class);
        context.startActivity(i);
    }
```

```
 E/AndroidRuntime: FATAL EXCEPTION: main
     java.lang.RuntimeException: Unable to start receiver com.app.demobroadcast.MyReceiver: android.util.AndroidRuntimeException: Calling startActivity() from outside of an Activity  context requires the FLAG_ACTIVITY_NEW_TASK flag. Is this really what you want?
         
         ···········
         
      Caused by: android.util.AndroidRuntimeException: Calling startActivity() from outside of an Activity  context requires the FLAG_ACTIVITY_NEW_TASK flag. Is this really what you want?
         
         ···········

```

报错原因是Activity是存储在Task中的，应用启动的时候会创建一个Task存储MainActivity，在MainActivity启动一个Activity时，Activity会依附于MainActivity的Task中，但是在BroadcastReceiver中context不是MainActivity，所以无法获取一个Task，解决方法是使用FLAG_ACTIVITY_NEW_TASK，这属于Activity的启动模式范畴，之前写过《[理解Android Activity运行模式](https://zengmianhui.github.io/2017/03/22/理解Android-Activity运行模式（understand-Android-Activity-s-launch-mode）/)》。


```
Intent i=new Intent(context,MainActivity.class);
        i.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        context.startActivity(i);
```

