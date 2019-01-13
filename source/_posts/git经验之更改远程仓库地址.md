---
title: git经验之更改远程仓库地址
date: 2017-04-18 21:28:07
categories:
  - git
  - git
tags:
  - git
---
# 参考链接
[git修改远程仓库地址——赖忠标](http://www.cnblogs.com/lazb/articles/5597878.html)  

# git修改远程仓库地址的三种方法
## 直接命令修改

> git remote set-url origin  [url]

<!--more-->
## 命令，先删除后设新地址

> git remote rm origin  
git remote add origin [url]

## 直接修改配置文件
文件位置：git/config  
`config`
```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[gui]
	wmstate = normal
	geometry = 841x483+225+101 189 218
[remote "origin"]
	url = git@github.com:zengmianhui/android_project.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master

```