---
title: 运行我的第一个python程序
date: 2017-02-06 00:31:20
categories:
  - python
tags:
  - python
---

# 运行我的第一个python程序
## python简介（python是一门跨平台的编程语言）
创始人Guido van Rossum
创始人是Guido van Rossum荷兰人，google工程师，1989年圣诞节，龟叔为了打发无聊的圣诞节所开发的python语言。
<!--more-->
### python适合的领域
1、web网站和各种网络服务。
2、系统工具和脚本。
3、作为“胶水”语言把其他语言开发模块包装起来方便使用
### python不适合的领域
1、	贴近硬件的代码（首选C）【由于python是一门高级语言，所以不适合贴近硬件开发】
2、	移动开发（IOS/Android有各自的开发语言（objC,Swift/java））
3、	游戏开发（C/C++）【由于游戏开发需要有高速度的渲染，python运行速度缓慢，所以不适合游戏开发】
### python的实际应用
国外：Youtube
国内：豆瓣、搜狐邮箱
开源云计算平台
### python与其他语言的对比
 ![这里写图片描述](http://img.blog.csdn.net/20170206002619216?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
python的优点是代码量少，代码简单，但是缺点是运行速度缓慢，针对网络和硬盘都很缓慢。
python的代码无法加密，虽然无法加密，但是由于现代是属于互联网时代，只要源代码不被查看到，就不会泄漏，就像豆瓣是不会被看到它实际的程序程序代码。

## python的安装与运行
### python的版本
python有两个版本，为什么要介绍版本呢，因为，python两个版本的程序两者无法兼容。
python2.7版本的程序无法在3.3版本上运行，同样3.3的程序也无法在2.7的平台上运行
### python的安装
1、从官网下载安装包
 ![这里写图片描述](http://img.blog.csdn.net/20170206002638669?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
2、安装非常简单，直接打开安装包安装就可以了，所以不附图
3、增加环境变量
增加一个环境变量，就可以直接使用CMD编写python程序了
 ![这里写图片描述](http://img.blog.csdn.net/20170206002649841?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
直接使用命令提示符
![这里写图片描述](http://img.blog.csdn.net/20170206002707080?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
### 第一个python程序（Hello World!）
直接使用cmd编写程序有一个缺点，就是下次还得再输入一次程序，才可以。
所以我们将python程序用“.py”的后缀名文件保存下来
 ![这里写图片描述](http://img.blog.csdn.net/20170206002720826?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
使用cmd进入py文件的目录下，使用代码“python demo.py”运行python程序 
![这里写图片描述](http://img.blog.csdn.net/20170206002727534?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)