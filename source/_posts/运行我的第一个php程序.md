---
title: 运行我的第一个php程序
date: 2017-02-05 02:11:21
categories:
  - php
tags:
  - php
---
## php的简介
### 什么是php？
php是一种超文本的标记语言，或者叫做开源脚本言。它是一种运行在服务器上的程序语言，语法简单，开发速度快，像WordPress博客、facebook社交平台都采用了php的技术。
<!--more-->
## php的简介
### 什么是php？
php是一种超文本的标记语言，或者叫做开源脚本言。它是一种运行在服务器上的程序语言，语法简单，开发速度快，像WordPress博客、facebook社交平台都采用了php的技术。
### 什么是php文件
1.	PHP 文件能够包含文本、HTML、CSS 以及 PHP 代码
2.	PHP 代码在服务器上执行，而结果以纯文本返回浏览器
3.	PHP 文件的后缀是 ".php"
### php能够做什么？
1.	PHP 能够生成动态页面内容
2.	PHP 能够创建、打开、读取、写入、删除以及关闭服务器上的文件
3.	PHP 能够接收表单数据
4.	PHP 能够发送并取回 cookies
5.	PHP 能够添加、删除、修改数据库中的数据
6.	PHP 能够限制用户访问网站中的某些页面
7.	PHP 能够对数据进行加密
### 为什么使用php？
1.	PHP 运行于各种平台（Windows, Linux, Unix, Mac OS X 等等）
2.	PHP 兼容几乎所有服务器（Apache, IIS 等等）
3.	PHP 支持多种数据库
4.	PHP 是免费的。请从官方 PHP 资源下载：www.php.net
5.	PHP 易于学习，并可高效地运行在服务器端
## 搭建开发环境（第一个程序）

使用PHPstudy作为php的服务器作调试运行
此安装程序集成了php所需的所有开发工具，
Apache+Nginx+LightTPD+PHP+MySQL+phpMyAdmin+Zend Optimizer+Zend Loader
直接下载phpstudy的压缩包进行解压安装即可，运行phpstudy
![这里写图片描述](http://img.blog.csdn.net/20170205015043138?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 运行一个php程序
<?php
	echo "Hello World!This is  my first php program!"
?>
使用.php后缀的文件保存在“phpstudy—WWW”目录下，“WWW”目录是phpstudy服务运行的目标目录
![这里写图片描述](http://img.blog.csdn.net/20170205015056653?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTEyNzQ2MjQ5OTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)