---
title: hexo如何为github博客设置一个域名？
date: 2017-01-29 12:09:00
categories:
  - git
  - hexo
tags:
  - 个人主页
---
摘要：
	为我们的github博客设置一个个人域名
<!--more-->
正文：
	我的域名是在腾讯去注册的，那就应该去腾讯云解析域名。
操作步骤：
## 第一步
管理中心-域名管理-点击解析-点击添加记录
![这里写图片描述](http://img.blog.csdn.net/20170129115704677?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170129115714974?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
填写github上个人博客的地址
![这里写图片描述](http://img.blog.csdn.net/20170129115722943?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 第二步
![这里写图片描述](http://img.blog.csdn.net/20170129115904271?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 第三步
在站点的source目录下创建CNAME并添加域名不包含Http://
![这里写图片描述](http://img.blog.csdn.net/20170129120048006?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
发布博客即可

```
$ hexo d -g
```

