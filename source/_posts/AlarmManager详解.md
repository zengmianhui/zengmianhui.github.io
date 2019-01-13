---
title: AlarmManager详解
date: 2017-04-09 21:48:25
categories:
  - android
  - android各种manager
tags:
  - android
  - android各种manager
---
# 参考链接
[Android闹钟设置的解决方案——HanWen](http://www.jianshu.com/p/1f919c6eeff6)  

[Android AlarmManager 定时会 “失效” 的问题——开源中国社区的问答](http://www.oschina.net/question/261246_140677) 

[保证Android后台不被杀死的几种方法——不吃早饭好不好](http://www.cnblogs.com/dixonyy/p/5163880.html)  

[Android中运用AlarmManager需注意设置进程属性——choujs](http://www.myexception.cn/android/917358.html)  

[AlarmManager 怎样在进程被干掉的情况下触发回调——百度知道](https://zhidao.baidu.com/question/1178427619022141179.html)
# AlarmManager详解
AlarmManager通常用途是用来开发手机闹钟，但是AlarmManager的用处并只是这个。AlarmManager其实是一个全局定时器，它可以在指定时间或指定周期启动其他组件（`Activity`、`Service`、`BroadcastReceiver`）
## 主要遇到一些问题
* 华为手机上kill应用后，无法唤醒Alarm  
    参考了一些社区问答，大部分原因都是手机为Alarm设置了**统一的唤醒时间**
* 不能精确启动闹钟服务  
    API19以上是无法精确时间的，可以调用`setExact()`和`setWindow()`
* 华为手机上休眠无法启动闹钟服务

<!--more-->

##  主要方法
### set(int type,long triggerAtTime,PendingIntent operation)
设置指定triggerAtTime时间启动由operation参数指定组件。其中第一个参数指定定时服务的类型，该参数可接受如下值。
* `ElAPSED_REALTIME`：  
指定从现在开始过了一定时间后启动operation所对应的组件。

* `ELASPED_REALTIME_WAKEUP`：  
指定从现在开始时间过了一定时间operation所对应的组件。即使系统处于休眠状态也会执行也会执行operation所对应的组件。

* `RTC`：  
指定当系统调用System.currentTimeMillis()方法的返回值与triggerAtTime相等时启动operation所对应的组件。

* `RTC_WAKEUP`:指定当系统调用System.currentTimeMillis()方法的返回值与triggerAtTime相等时启动operation对应的组件，即使系统休眠状态也会执行operation所对应的组件。

### setInexactRepeating(int type,long triggerAtTime,long interval,PendingIntent operation)
设置非精确的周期性任务。例如，我们设置Alarm每个小时启动一次，但是系统不一定总在每个小时的开始启动Alarm服务。

### setRepeating(int type,long triggerAtTime,long interval,PendingIntent operation)
设置一个周期性执行的定时服务。
### cancel(PendingIntent operation)
取消AlarmManager的定时服务。
### 注意
* API 19(Android4.4)开始，AlarmManager的机制是非准确激发的，操作系统会偏移(shift)闹钟来最小化唤醒和电池消耗。不过AlarManager新增了如下两个方法来支持精确激发。
    * `setExact(int type long triggerAtMillis,PendingIntent operation)`
    设置闹钟闹钟将在精确的时间被激发。

    * `setWindow(int type,long windowStartMillis,long windowLengthMillis,PendingIntent operation)`
    设置闹钟将在精确的时间段内被激发。
* 很显示API19以后无法使用`setInexactRepeating()`和`setRepeating()`，也就是无法设置重复闹钟，**唯一解决的方式，也只有启动闹钟的时候再设置一次闹钟，也就变相地实现了重复闹钟了。**
* API19以下使用`setExact()`和`setWindow()`将会报没有匹配的方法  
```
java.lang.NoSuchMethodError: android.app.AlarmManager.setExact  
```

解决办法是判断SDK版本，根据SDK版本来定义不同的方法。
```
int sdkVersion = Integer.valueOf(Build.VERSION.SDK_INT);
```
## 使用AlarmManager做demo示例
### 使用AlarmManager
### 闹钟服务
代码结构：  
main/com.app.demo  
└──MainActivity.java——>用户设置闹钟时间  
└──AlarmActivity.java——>闹钟启动时，将启动的Activity  
└──AudioPlayer.java———>封装的，用来播放音频  

> MainActivity.java

```
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        findViewById(R.id.setDate).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Calendar currentTime = Calendar.getInstance();
                new TimePickerDialog(MainActivity.this, new TimePickerDialog.OnTimeSetListener() {

                    @RequiresApi(api = Build.VERSION_CODES.KITKAT)
                    @Override
                    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
                        Intent i=new Intent(MainActivity.this,AlarmActivity.class);
                        //创建PendingIntent对象
                        PendingIntent pi=PendingIntent.getActivity(MainActivity.this,0,i,0);
                        Calendar c=Calendar.getInstance();
                        c.setTimeInMillis(System.currentTimeMillis());
                        
                        //Calendar.HOUR这是12小时制因为无论你的TimePickerDialog设置的是12还是24，hourOfDay默认获取的是24小时制的  
                        //根据用户的选择的时间来设置Calendar对象 c.set(Calendar.HOUR_OF_DAY,hourOfDay);
                        c.set(Calendar.MINUTE,minute);
                        //获取AlarmManager
                        AlarmManager am= (AlarmManager) getSystemService(ALARM_SERVICE);
                        Log.i(TAG, "onTimeSet: "+SystemInfoUtil.getSDKVersionNumber());
                        if (SystemInfoUtil.getSDKVersionNumber()>=19){
                        //API19以上使用
                            am.setExact(AlarmManager.RTC_WAKEUP,c.getTimeInMillis(),pi);
                        }else {
                            am.set(AlarmManager.RTC_WAKEUP,c.getTimeInMillis(),pi);
                        }
                        Toast.makeText(MainActivity.this,"设置闹钟成功", Toast.LENGTH_LONG).show();
                    }
                },currentTime.get(Calendar.HOUR_OF_DAY),currentTime.get(Calendar.MINUTE),false).show();
            }
        });
    }
```

> AlarmActivity.java

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_alarm);
    // 加载音乐
    try {
//            String path = RingtoneManager.getActualDefaultRingtoneUri(this, RingtoneManager.TYPE_RINGTONE).getPath();
        AssetFileDescriptor openFd=getAssets().openFd("music.mp3");
        AudioPlayer.getInstance().play(openFd);
        AudioPlayer.getInstance().setLooping(true);
    } catch (IOException e) {
        e.printStackTrace();
    }
    new AlertDialog.Builder(this)
            .setTitle("闹钟")
            .setTitle("时间到！！！！！ ")
            .setPositiveButton("确定", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    AudioPlayer.getInstance().stop();
                }
            })
            .create().show();
}

```

