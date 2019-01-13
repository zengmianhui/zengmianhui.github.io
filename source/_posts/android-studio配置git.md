---
title: android studio配置git
date: 2017-01-27 17:01:10
categories:
  - android
  - android studio
tags:
  - android
  - android studio
  - git
---

摘要:
	如今开发时代处于团队合作的时代，团队合作的工具是必不可少的，像git就是团队合作比较好的工具，这篇告诉大家如何在android studio中配置git版本控制工具，方便开发。
<!-- more -->
正文：
部分教程来自[全面介绍Android Studio中Git 的使用（一）](http://blog.csdn.net/gao_chun/article/details/49817229/)

当前文章目录：

[TOC]
## 首先当然是先创建一个本地仓库
首先在一个目录准备作为仓库的目录下，右键“git bash here”
 
 ![这里写图片描述](http://img.blog.csdn.net/20170123012152124?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 ![这里写图片描述](http://img.blog.csdn.net/20170123012239765?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
这样本地仓库创建完成了
##配置远程仓库（提交的时候要注意，仓库至少要有一个文件，git不允许仓库为空）
首先要知道“commit”是提交到本地仓库，“push”是提交到远程仓库，所以光有本地仓库是不能团队合作的，我要将本地仓库和远程仓库进行关联
关联远程仓库，一个重要的条件就是本地git的“SSH”要和远程仓库的“SSH”相匹配。
我首先要配置创建一个SSH在本地（在C盘本地用户目录里面）
### 1、先配置git用户（不作截图）
$ git config --global user.name "xuhaiyan"
$ git config --global user.email "haiyan.xu.vip@gmail.com"
###2、创建SSH，创建目录在C盘当前用户下

ssh-keygen -t rsa -C “zengmianhui”
双引号内为电脑登录时的用户名
 ![这里写图片描述](http://img.blog.csdn.net/20170123012354325?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
 ![这里写图片描述](http://img.blog.csdn.net/20170123014626433?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170123012425579?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 3、	查看SSH代码，并将其设置到远程仓库（这里示例github）
 ![这里写图片描述](http://img.blog.csdn.net/20170123012436748?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 ![这里写图片描述](http://img.blog.csdn.net/20170123012528858?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
### 4、	将本地仓库关联到远程仓库
git remote add origin [url]
 ![这里写图片描述](http://img.blog.csdn.net/20170123012548454?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 在Settings中选择Version Control 并配置Git，不多说，上个图
 ![这里写图片描述](http://img.blog.csdn.net/20170123012559015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
配置完后我们点下路径后的Test按钮，若出现该Success提示框则表明配置成功：
 ![这里写图片描述](http://img.blog.csdn.net/20170123012634579?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
配置好后我们点击Version Control的时候，在右侧会提示该项目所采用的版本控制工具
 ![这里写图片描述](http://img.blog.csdn.net/20170123012659220?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
若发现该路径为灰色，需要选中，点击"+"
 ![这里写图片描述](http://img.blog.csdn.net/20170123012731236?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
因为我们是在一个已有的项目上创建的仓库，所以配置完后需要为项目指定版本控制工具，也可以在最开始Create项目的时候直接指定仓库路径。

## 提交代码
起初，我们项目所有文件颜色，都是 [白色：正常文件的颜色]
 ![这里写图片描述](http://img.blog.csdn.net/20170123012757502?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
但是当我们为项目指定仓库路径后，所有文件颜色的颜色变了 [红色：指定仓库路径后，未Add的文件]
 ![这里写图片描述](http://img.blog.csdn.net/20170123012811659?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
切换为Project视图，对项目右键，Add文件。（在创建仓库的时候.gitignore文件已默认生成，可以修改添加需要ignore的文件）
 ![这里写图片描述](http://img.blog.csdn.net/20170123012834488?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Add成功后，我们在看看文件的颜色为 [绿色：已Add，但未commit的文件]
 ![这里写图片描述](http://img.blog.csdn.net/20170123012855174?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
也可查看Log日志
 ![这里写图片描述](http://img.blog.csdn.net/20170123012920552?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Commit文件可通过 项目右键--> Git --> Commit Directory...  也可点击 工具栏上的两个按钮。
 ![这里写图片描述](http://img.blog.csdn.net/20170123012947721?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
填写提交信息，然后可先Commit 然后再 Push 或者直接选择 Commit And Push ...
 ![这里写图片描述](http://img.blog.csdn.net/20170123013016053?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
当我们对某个类进行修改后，会发现文件颜色又变了 [墨蓝色：提交成功后修改过的文件]，到此就将项目上传至仓库了，可以通过Studio中的Version Control一目了然的查看提交Log。

 ![这里写图片描述](http://img.blog.csdn.net/20170123013030897?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
关于如何下拉代码，我们可以点击工具栏上的“CVS↓”![这里写图片描述](http://img.blog.csdn.net/20170123013206754?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 按钮，然后在在弹出框中选择 Merge 合并。
 
![这里写图片描述](http://img.blog.csdn.net/20170123013226629?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)