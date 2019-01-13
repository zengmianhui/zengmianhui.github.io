---
title: Android中传递对象的三种方法
date: 2017-04-30 18:29:33
categories:
  - android
  - 通讯
  - 对象传递
tags:
  - android
  - 对象传递
---
# 参考链接
[《Android中传递对象的三种方法》——Malinkang](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0104/2256.html)  
[《Android Serializable与Parcelable原理与区别》——love_world_](http://blog.csdn.net/androiddevelop/article/details/22108843)  
[《探索Android中的Parcel机制（上）》——文斌](http://blog.csdn.net/caowenbin/article/details/6532217)
# Android中传递对象
`Serializable`，`Parcelable`和`转为JSON字符串`，JSON就不解析了大家都懂，Serializable是jdk提供的方法，最熟悉的序列化，Parcelable是android.os包里面的,也是序列化。两种都是用于支持序列化、反序列化话操作，两者最大的区别在于存储媒介的不同，**Serializable使用IO读写存储在硬盘上**，而**Parcelable是直接在内存中读写**，很明显内存的读写速度通常大于IO读写，所以在Android中通常优先选择Parcelable。
<!--more-->
## Serializable
> 通过让实体类实现Serializable，这个也不需要太多解释，只是一个标志而已，表示可以序列化。

```
public class Bean implements Serializable {
    private int id;
    private String name;

    public int getId() {
        return id;
    }
//略·····················

```
> 调用Intent.putExtra(String name, Serializable value)，可以看到value是Serializable。
```
Bean b=new Bean();
b.setId(1);
b.setName("小明");
Intent intent=new Intent(this,TargetActivity.class);
intent.putExtra("serializable",b);//在Bundle中存放序列化对象
startActivity(intent);
```
>在目标Activity中调用getIntent().getSerializableExtra("serializable");
```
Bean b= (Bean) getIntent().getSerializableExtra("serializable");
Log.i(TAG, "onCreate: "+b.getName());
```

打印出数据
```
04-30 07:30:47.764 1974-1974/com.app.demodeliverobject I/TargetActivity: onCreate: 小明
```

## Parcelable
### Parcelable机制原理
> 以下引用CSDN博客专家文斌解析，不敢说全懂

1. 整个读写全是在内存中进行，主要是通过malloc()、realloc()、memcpy()等内存操作进行，所以效率比JAVA序列化中使用外部存储器会高很多；
2. 读写时是4字节对齐的，可以看到#define PAD_SIZE(s) (((s)+3)&~3)这句宏定义就是在做这件事情；
3. 如果预分配的空间不够时newSize = ((mDataSize+len)*3)/2;会一次多分配50%；
4. 对于普通数据，使用的是mData内存地址，对于IBinder类型的数据以及FileDescriptor使用的是mObjects内存地址。后者是通过flatten\_binder()和unflatten_binder()实现的，目的是反序列化时读出的对象就是原对象而不用重新new一个新对象。

### 示例
> ParcelableBean实现Parcelable，表示可序列化
```
public class ParcelableBean implements Parcelable {
    private int id;
    private int age;
    private String name;

    protected ParcelableBean(Parcel in) {
        id = in.readInt();
        age = in.readInt();
        name = in.readString();
    }

    /**
     * 将数据存入Parcel容器中
     * Flatten this object in to a Parcel.
     * @param dest
     * @param flags
     */
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeInt(id);
        dest.writeInt(age);
        dest.writeString(name);
    }

    @Override
    public int describeContents() {
        return 0;
    }

    public static final Creator<ParcelableBean> CREATOR = new Creator<ParcelableBean>() {
        @Override
        public ParcelableBean createFromParcel(Parcel in) {
            return new ParcelableBean(in);
        }

        @Override
        public ParcelableBean[] newArray(int size) {
            return new ParcelableBean[size];
        }
    };
//get and set略 ·························
}
```
> 传入Parcel.obtain()，并通过Intent.putExtra(String name, Parcelable value)保存Parcelable对象
```
ParcelableBean b=new ParcelableBean(Parcel.obtain());
b.setId(1);
b.setAge(14);
b.setName("小明");
Intent intent=new Intent(this,TargetActivity.class);
intent.putExtra("parcelable",b);
startActivity(intent);
```

> 在目标Activity中调用getParcelableExtra("parcelable");
```
ParcelableBean b= (ParcelableBean) getIntent().getParcelableExtra("parcelable");
Log.i(TAG, "onCreate: ID号"+b.getId());
Log.i(TAG, "onCreate: 年龄"+b.getAge());
Log.i(TAG, "onCreate: 名字"+b.getName());
```

>以下为打印结果

```
04-30 08:42:49.874 30499-30499/com.app.demodeliverobject I/TargetActivity: onCreate: ID号1
04-30 08:42:49.874 30499-30499/com.app.demodeliverobject I/TargetActivity: onCreate: 年龄14
04-30 08:42:49.874 30499-30499/com.app.demodeliverobject I/TargetActivity: onCreate: 名字小明
```
## 三种数据传输速度的比较
![参考链接中的图片](Android中传递对象的三种方法/1.png)