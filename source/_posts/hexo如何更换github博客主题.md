---
title: hexo如何更换github博客主题
date: 2017-01-28 01:33:40
categories:
  - git
  - hexo
tags:
  - 个人主页
---

摘要：
	hexo如何更换github博客主题
<!--more-->
正文：
	[TOC]
# hexo更换github博客主题

[next主题官网](http://theme-next.iissnan.com/getting-started.html)
首先当然是下载主题，可以直接使用命令的形式，用git clone下来，也可以直接下载压缩包，然后解压在站点目录下的theme目录下。

```
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
编辑站点文件_config.yml

```
theme: next
```
这样子就可以了,启动hexo站点，直接验证主题
```
hexo s --debug
```

