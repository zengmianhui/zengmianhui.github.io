---
title: android Toolbar的使用
date: 2017-02-09 01:53:47
categories:
  - android
  - Material Design
tags:
  - android
  - Material Design
---
# android Toolbar的使用
## 参考文章
[ToolBar详解（手把手教程）](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1118/2006.html)
[stackoverflow-Android toolbar menu is not showing](http://stackoverflow.com/questions/28317905/android-toolbar-menu-is-not-showing)
[actionBar和Toolbar中如何动态隐藏和修改menu上的菜单](http://blog.csdn.net/chenguang79/article/details/48826199)
## 效果图
![这里写图片描述](http://img.blog.csdn.net/20170209010427047?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<!--more-->
## 配置material design
请确保有依赖以下三项
```
	compile 'com.android.support:appcompat-v7:25.1.1'
    compile 'com.android.support:support-v4:25.1.1'
    compile 'com.android.support:design:25.1.1'
```
## 去除原有的actionBar

```
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <!--增加以下三行，为app去除原有actionBar-->
        <item name="windowActionBar">false</item>
        <item name="android:windowNoTitle">true</item>
        <item name="windowNoTitle">true</item>
    </style>
```

## toolbar代码
res\layout\toolbar.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!--colorPrimary是主题使用的颜色，可以value/color.xml中修改-->
<!--actionBarSize是系统actionBar的高度-->
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    app:menu="@menu/menu_toolbar"
    >

</android.support.v7.widget.Toolbar>
```
一般来说toolbar会应用多个activity，所以我单独作为一个xml来使用，
在activity.xml中只需使用include标签，方便使用

```
	<include
        android:id="@+id/toolbar"
        layout="@layout/toolbar"
        />
```
## 编写menu/menu_toolbar.xml

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    >
    <!--showAsAction如果有足够的空间会显示出来，否则隐藏在工具栏里面-->
    <item
        android:id="@+id/search"
        android:icon="@mipmap/search"
        android:title="搜索"
        app:showAsAction="ifRoom"
        >
    </item>
    <item
        android:id="@+id/write"
        android:icon="@mipmap/write"
        android:title="书写"
        app:showAsAction="ifRoom"
        >
    </item>
    <item
        android:id="@+id/book"
        android:icon="@mipmap/book"
        android:title="书籍"
        app:showAsAction="ifRoom"
        >
    </item>
</menu>
```

## java注意setSupportActionBar和onPrepareOptionsMenu
src\MainActivity.java
```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final TextView hello= (TextView) findViewById(R.id.hello);
        Toolbar toolbar= (Toolbar) findViewById(R.id.toolbar);
        //配置代替actionBar
        setSupportActionBar(toolbar);
        //设置图标
        toolbar.setLogo(getResources().getDrawable(R.mipmap.ic_launcher));
        toolbar.setNavigationIcon(R.mipmap.ic_launcher);
        toolbar.setTitle("setTitle主标题");
        toolbar.setSubtitle("setSubtitle副标题");
        toolbar.setOnMenuItemClickListener(new Toolbar.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                switch (item.getItemId()){
                    case R.id.search:
                        hello.setText("您点击了搜索");
                        break;
                    case R.id.write:
                        hello.setText("您点击了书写");
                        break;
                    case R.id.book:
                        hello.setText("您点击了书籍");
                        break;
                }
                return false;
            }
        });
    }

    @Override
    public boolean onPrepareOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_toolbar,menu);
        return super.onPrepareOptionsMenu(menu);
    }
```
## 遇见的问题
### menu没有展示
出现我第一时间考虑，没有使用menu属性配置。
很显然我是有的

```
<android.support.v7.widget.Toolbar
……
app:menu="@menu/menu_toolbar"
>
</android.support.v7.widget.Toolbar>
```

[stackoverflow-Android toolbar menu is not showing](http://stackoverflow.com/questions/28317905/android-toolbar-menu-is-not-showing)
我在参考了stackoverflow的问题，之后发现是我没有使用
onPrepareOptionsMenu()

```
    @Override
    public boolean onPrepareOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_toolbar,menu);
        return super.onPrepareOptionsMenu(menu);
    }
```
后来，我尝试保留onPrepareOptionsMenu()，把setSupportActionBar()去掉，发现menu同样也会消失不见。
我由此估计，menu是actionbar才有的功能，toolbar只是调用了actionbar功能的部分方法。

所以，才有先用setSupportActionBar()替换actionbar
再onPrepareOptionsMenu();创建menu
### onPrepareOptionsMenu与onCreateOptionsMenu与onOptionsItemSelected的关系
onCreateOptionsMenu():创建菜单，但是从初始开始就只运行一次，所以activity实例化后，无法修改菜单内容
onOptionsItemSelected():对选中的菜单进行操作
onPrepareOptionsMenu()：创建菜单，activity初始化后仍会调用此方法，所以可在方法编写activity初始化后的功能
