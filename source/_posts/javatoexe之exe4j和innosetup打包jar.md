---
title: javatoexe之exe4j和innosetup打包jar
date: 2018-01-23 23:09:20
categories:
  - java
  - javatoexe
tags:
  - javatoexe
---

# 下面对几个重要配置进行说明
## exe4j
### APP名和输出路径配置
![image.png](http://upload-images.jianshu.io/upload_images/2641470-5fc0f33e35c8c0f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 选择需要打包的jar
![image.png](http://upload-images.jianshu.io/upload_images/2641470-3a6bff67ad2a0ce7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!--more-->

### 打包jre运行环境
![image.png](http://upload-images.jianshu.io/upload_images/2641470-e76be763e240f538.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 其他配置不作解释，基本够用
## innosetup

>  前面的exe4j就像是生成了一个exe文件 ，然后为了exe配置了一些文件的路径位置，那么innosetup就是打包所有文件方便拷贝

### 打包成安装包
![image.png](http://upload-images.jianshu.io/upload_images/2641470-a61506845f32147b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/2641470-7bf3f427aec191c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### innosetup配置文件
> 图形界面配置完后会生成配置文件，点击comlipe就可以了，配置文件也可随心编写配置
![image.png](http://upload-images.jianshu.io/upload_images/2641470-9adfdab16511fe71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





