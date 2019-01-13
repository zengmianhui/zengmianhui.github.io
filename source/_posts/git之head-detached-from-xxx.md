---
title: 'git之head detached from [xxx]'
date: 2017-06-12 22:04:52
categories:
  - git
  - git
tags:
  - git
---

# 参考链接
[《git detached from head》————夜空霓虹](http://blog.csdn.net/zhang_xiaomeng/article/details/52597076)  
[《git checkout 命令详解》————胡涛儿](http://www.cnblogs.com/hutaoer/archive/2013/05/07/git_checkout.html?utm_source=tuicool&utm_medium=referral)  
[《git问题记录--如何从从detached HEAD状态解救出来》————馒头MT](http://www.jianshu.com/p/ae4857d2f868)  
[《git cherry-pick. 如何把已经提交的commit, 从一个分支放到另一个分支》————sg552](http://sg552.iteye.com/blog/1300713)
# git之head detached from [xxx]
## 头部从某提交分离？
近期发现，android studio提交的时候提示`head detached from [xxx]`，不知道是什么意思，百度了之后大概了解了一下，就是不知道是不是之前操作什么，导致我的一次提交没提交到Head指向的分支，这是很危险，意味当前提交不属于任何分支。  
<!-- more -->
> `git checkout master`的时候，提示有5个提交没有链接任何一个分支，同时也给出了解决方式

```
$ git checkout master
Checking out files: 100% (8624/8624), done.
Warning: you are leaving 5 commits behind, not connected to
any of your branches:

  d232605 all
  78c29d8 all
  4eba374 DemoDataBindingLibrary-测试databinding框架
  7c6d192 DemoDataBindingLibrary-测试databinding框架
  15c2a61 EnglishApp-更换评价展示；修复成绩显示不正确的情况

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> d232605

Switched to branch 'master'
M       GeekNews

```
> `git log`查看当前分支历史提交记录，发现并没有那五个提交记录，显然脱离了master分支  

![查看历史提交记录](git之head detached from [xxx]/2017-06-12_154037.png)

## 通过合并分支来解决
> 可以用以下方式来解决
> 1. 创建一个临时分支，将detach出来的提交，合并到临时分支中
> 2. 将临时分支merge到工作分支中。
> 3. 删除临时分支

### 创建临时分支
> 注意此命令并未立即切换到临时分支
```
git branch temp 15c2a61
```
![创建临时分支](git之head detached from [xxx]/2017-06-12_162203.png)  

### 将其他detach的一起合并到temp
```
git checkout temp #切换到temp分支
git cherry-pick [commitid]
```
> 所以合并期间可能会出现合并冲突（cherry-pick为优选的意思，意思是取一个最优的提交来合并，有时候懂英文理解得比较快）

![cherry-pick合并冲突](git之head detached from [xxx]/2017-06-12_183817.png)  

> 上面可以看到提示冲突后，会CHERRY-PICK模式，在这期间无法切换分支等其他操作。会提示某文件冲突，**我们可以打开对应文件，会发现会有以下符号
```
>>>>>>>> HEAD
    HEAD指向的分支代码
========
    某提交的代码
>>>>>>>> 某提交信息commit messages
```
> 箭头和等号是Git用来区别不同分支或者提交的不同内容，通常我们保留最新的分支修改就可以了。然后再通过以下代码完成合并

```
git add 某文件路径

git commit -m "提交信息"

git cherry-pick [commitid] #再使用一次命令合并
```

> 多次使用`git cherry-pick [commitid]`将其他几个提交合并

### 切换到master分支，将temp合并到master分支

```
git checkout master

git merge temp #使用此命令可能也会有冲突
```  

![发生冲突](git之head detached from [xxx]/2017-06-12_181756.png)  

> 发生冲突后，进入了MERGING模式，跟上面的方法一样进入对应文件修改,然后`git add ` 和`git commit`然后`git merge temp`，可以使用`git log`打印提交信息查看是否合并成功

![修改文件内容，保存最新分支信息](git之head detached from [xxx]/2017-06-12_181946.png)  
![两次提交我们的修改](git之head detached from [xxx]/2017-06-12_182814.png)  

