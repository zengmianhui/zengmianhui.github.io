---
title: Android Service总结
date: 2017-04-16 16:49:13
categories:
  - android
  - 四大组件
  - service
tags:
  - android
  - android service
---
# 参考链接
[Service知识总结——某学姐](http://mouxuejie.com/blog/2016-04-16/service-intentservice-analysis/)
# Service

## service生命周期
service有两个生命周期，为什么会有两个生命周期呢，这是根据service的启动方式来进行区分的。
* `Context.startService()`     
    通过该方法启动service，访问者与service之间没有关联，即使访问者退出了，service也仍然运行。以下是打印结果

```
com.app.demoservice I/MyService: onCreate: 开始创建Service
com.app.demoservice I/MyService: onStartCommand: 每次调用startService()方法时，都会回调这个方法
```

* `Context.bindService(Intent service,ServiceConnection conn,int flags)`  
    使用该方法启动的service,访问者与service绑定在一起，访问者退出，service也就终止了。  
![Android Service的两种生命周期](Android Service总结/1.jpg)  

<!--more-->

## service的主要回调方法
* `IBinder onBind(Intent intent)`  
该方法是Service子类必须实现的方法，该方法返回一个IBinder对象，应用程序可通过该对象与Service组件通信。

* `void onCreate()`  
在该Service第一次被创建后会立即回调该方法  

* `void onDestroy`  
在该Service被关闭之前会回调该方法  

* `void onStartCommand(Intent intent,  int flags, int startId)`  
该方法的早期版本是`onStart(Intent intent,int startId)`,每次客户端调用`startService(Intent)`方法启动Service，都会回调该方法  
多次调用startService()，会多次调用onStartCommand()
```
com.app.demoservice I/MyService: onCreate: 开始创建Service
com.app.demoservice I/MyService: onStartCommand: 每次调用startService()方法时，都会回调这个方法
com.app.demoservice I/MyService: onStartCommand: 每次调用startService()方法时，都会回调这个方法
com.app.demoservice I/MyService: onStartCommand: 每次调用startService()方法时，都会回调这个方法
com.app.demoservice I/MyService: onStartCommand: 每次调用startService()方法时，都会回调这个方法
```
* `void onUnBind(Intent intent)`  
当Service与之绑定的客户端都断开连接时，会回调这个方法

## 绑定本地Service并与之通信
使用startService()启动的service，基本不会与访问者有太多的数据交互,所以访问者与service的数据交互最好是，用bindService()去启动这个service。
* `Context.bindService(Intent service,ServiceConnection conn,int flags)`  
    * `Intent service`  
    启动的意图  
    
    * `ServiceConnection conn`  
    ServiceConnection一个接口，该对象用于监听访问者与Service之间的状态，当访问者与Service建立连接时，会回调`onServiceConnected(ComponentName name, IBinder service)`，当Service发生异常或者其他原因中止时，会回调`onServiceDisconnected(ComponentName name)`，但是访问者主动调用unBindService()方法时并回调onServiceDisconnected();

    * `int flag`  
        此参考代表是否自动创建Service
        * `0`  
        指定为0时，不自动创建 
        * `Context.BIND_AUTO_CREATE`  
        指定为这个常量时，表示自动创建Service
### 示例
* MyService.java
```
public class MyService extends Service {
    private static final String TAG = MyService.class.getSimpleName();
    private MyBinder myBinder=new MyBinder();
    //模拟数据
    private int serviceData =0;
    //是否停止对数据处理
    private boolean isStop=false;
    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        Log.i(TAG, "onBind: 开始绑定IBinder");
        return myBinder;
    }
    class MyBinder extends Binder {
        public int  getDataState(){
            return serviceData;
        }
        public void stop(boolean stop){
            isStop=stop;
        }
    }
    @Override
    public void onCreate() {
        super.onCreate();
        Log.i(TAG, "onCreate: 开始创建Service");

        new Thread(new Runnable() {
            @Override
            public void run() {
                while (!isStop){
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (serviceData>30){

                    }
                    serviceData++;
                }
                stopSelf();
            }
        }).start();
    }

    /**
     * onStart()方法是Service的早期方法，onStartCommand()是新的推荐方法
     * @param intent
     * @param flags
     * @param startId
     * @return
     */
    @Override
    public int onStartCommand(Intent intent,  int flags, int startId) {
        Log.i(TAG, "onStartCommand: 每次调用startService()或者 bindService()方法时，都会回调这个方法");
            return super.onStartCommand(intent, flags, startId);
}

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.i(TAG, "onDestroy: Service开始销毁之前会回调这个方法");
    }

    @Override
    public boolean onUnbind(Intent intent) {
        Log.i(TAG, "onUnbind: 当Service与之绑定的客户端都断开连接时，会回调这个方法");
        return super.onUnbind(intent);
    }
}
```

* MainActivity.java
```
//绑定并启动service
findViewById(bindService).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        startService(i);
        bindService(i,MainActivity.this, BIND_AUTO_CREATE);
    }
});
findViewById(R.id.getServiceData).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        int dataState = myBinder.getDataState();
        Toast.makeText(MainActivity.this,"Service中数据为-->"+dataState, Toast.LENGTH_SHORT).show();
    }
});
```
## IntentService
IntentService是继承于Service的，跟Service一样也可以执行耗时任务，IntentService封装了线程和消息循环，IntentService中使用HandlerThread来处理，HandlerThread继承了Thread,内部实现使用了Looper和MessageQueue，用法也很简单。 

* HandlerThread

```
HandlerThread myHandlerThread=new HandlerThread("myHandlerThread");
myHandlerThread.start();
//由于获取了HandlerThread中的Loooper，也就是handleMessage会HandlerThread的线程中执行。
Handler myHandler=new Handler(myHandlerThread.getLooper()){
    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
    }
};
```
### IntentService的源码解析
IntentService封装了HandlerThread，做一个简单分析就可以。

> 这是IntentService中的一个内部，主要负责异步处理，`onHandleIntent((Intent)msg.obj);`是IntentService是抽象方法，也就是提供给我们实现异步处理的方法，onHandleIntent执行完成后会自动停止Service。

```
private final class ServiceHandler extends Handler {
        public ServiceHandler(Looper looper) {
            super(looper);
        }

        @Override
        public void handleMessage(Message msg) {
            onHandleIntent((Intent)msg.obj);
            stopSelf(msg.arg1);
        }
    }
```


> 以下代码可以注意HandlerThread的实例化并start，这是service第一次创建时会立即启动一个线程。

```
@Override
public void onCreate() {
    // TODO: It would be nice to have an option to hold a partial wakelock
    // during processing, and to have a static startService(Context, Intent)
    // method that would launch the service & hand off a wakelock.

    super.onCreate();
    HandlerThread thread = new HandlerThread("IntentService[" + mName + "]");
    thread.start();

    mServiceLooper = thread.getLooper();
    mServiceHandler = new ServiceHandler(mServiceLooper);
}
```

> IntentService的排队处理  
> 我们可以看每次启动Service都会发送一个消息，`mServiceHandler.sendMessage(msg);`，熟悉消息机制的我们知道，多余消息会处于等待状态。

```
//重点这个方法上
@Override
public void onStart(@Nullable Intent intent, int startId) {
    Message msg = mServiceHandler.obtainMessage();
    msg.arg1 = startId;
    msg.obj = intent;
    mServiceHandler.sendMessage(msg);
}

/**
 * You should not override this method for your IntentService. Instead,
 * override {@link #onHandleIntent}, which the system calls when the IntentService
 * receives a start request.
 * @see android.app.Service#onStartCommand
 */
@Override
public int onStartCommand(@Nullable Intent intent, int flags, int startId) {
    onStart(intent, startId);
    return mRedelivery ? START_REDELIVER_INTENT : START_NOT_STICKY;
}
```

## 需要注意的地方
* android5.0以上不允许使用隐式意图的方式启动Service 

* Service是在UI线程中的，所以Service也是不能执行耗时操作的，那么为什么不使用直接在Activity创建线程处理呢？因为如果在Activity退出的情况下，线程执行还未结束，此时所在的进程就变成空进程，系统可能需要内存会优先终止该进程，宿主进程被的终止的话，所在的子线程也会被中止。  