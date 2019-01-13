---
title: android Material Design 底部导航栏
date: 2017-02-08 03:22:38
categories:
  - android
  - Material Design
tags:
  - android
  - Material Design
---

# android Material Design 底部导航栏
## 实现效果
![这里写图片描述](http://img.blog.csdn.net/20170208030222983?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<!--more-->
## 配置gradle依赖

```
compile 'com.android.support:appcompat-v7:25.1.0'
//注意：V4包一定要加，material design是依赖V4包的
//不加，某些类可能报“ClassNotFoundException”
compile 'com.android.support:support-v4:25.1.0'
compile 'com.android.support:design:25.1.0'
```
## 使用BottomNavigationView

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.kangda.app.MainActivity">
    <!--一定要加入V4包和Material Design包的依赖-->
    <!--itemIconTint设置图标的颜色-->
    <!--itemTextColor设置文本标题的颜色-->
    <android.support.design.widget.BottomNavigationView
        android:id="@+id/nav_bottom"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:layout_alignParentBottom="true"
        app:itemIconTint="#767676"
        app:itemTextColor="#767676"
        app:menu="@menu/nav_bottom_item"
        > </android.support.design.widget.BottomNavigationView>
</RelativeLayout>

```
## res/menu/nav_bottom_item.xml

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    >
    <!--showAsAction如果有剩余空间就显示出来，否则隐藏-->
    <item android:id="@+id/menu_pk"
        android:enabled="true"
        app:showAsAction="ifRoom"
        android:title="@string/menu_title_pk"
        android:icon="@mipmap/menu_pk" />
    <item android:id="@+id/menu_home"
        android:enabled="true"
        app:showAsAction="ifRoom"
        android:title="@string/menu_title_home"
        android:icon="@mipmap/menu_home_press" />
    <item android:id="@+id/menu_mine"
        android:enabled="true"
        app:showAsAction="ifRoom"
        android:title="@string/menu_title_mine"
        android:icon="@mipmap/menu_mine" />
</menu>
```
## src/MainActivity.java
@BindView这注解依赖ButterKnife，这个不必理会
```
@BindView(R.id.nav_bottom)
    protected BottomNavigationView navBottom;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ButterKnife.bind(this);
        navBottom.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch (item.getItemId()){
                    case R.id.menu_pk:
                        Toast.makeText(MainActivity.this, "PK", Toast.LENGTH_SHORT).show();
                        break;
                    case R.id.menu_home:
                        Toast.makeText(MainActivity.this, "主页", Toast.LENGTH_SHORT).show();
                        break;
                    case R.id.menu_mine:
                        Toast.makeText(MainActivity.this, "我的", Toast.LENGTH_SHORT).show();
                        break;
                }
                return false;
            }
        });
    }
```

## 注意
 

> 盲目照抄android官方文档带来的问题

> 以下是官方文档抄过来的，有点问题
> 问题在这一句代码
> xmlns:design="http://schema.android.com/apk/res/android.support.design"
> 正确的代码是
> xmlns:app="http://schemas.android.com/apk/res-auto"

```
layout resource file:
 <android.support.design.widget.BottomNavigationView
     xmlns:android="http://schemas.android.com/apk/res/android"
     xmlns:design="http://schema.android.com/apk/res/android.support.design"
     android:id="@+id/navigation"
     android:layout_width="match_parent"
     android:layout_height="56dp"
     android:layout_gravity="start"
     design:menu="@menu/my_navigation_items" />

 res/menu/my_navigation_items.xml:
 <menu xmlns:android="http://schemas.android.com/apk/res/android">
     <item android:id="@+id/action_search"
          android:title="@string/menu_search"
          android:icon="@drawable/ic_search" />
     <item android:id="@+id/action_settings"
          android:title="@string/menu_settings"
          android:icon="@drawable/ic_add" />
     <item android:id="@+id/action_navigation"
          android:title="@string/menu_navigation"
          android:icon="@drawable/ic_action_navigation_menu" />
 </menu>
```
这个代码不会报错，但是XML布局预览无任何效果，暂时不知道是什么原因。
## 问题总结
> viewpager的滚动监听和bottomNavigationView有关系么，为什么没有viewpager的滚动监听会影响到bottomNavigationView，使bottomNavigationView只有点击事件，没有选中改变按钮颜色呢，难道是viewpager的滚动监听的配置增加了焦点，好像也不对，如果没有焦点，bottomNavigationView也就不能点击