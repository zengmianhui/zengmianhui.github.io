---
title: hexo之如何强制访问https
date: 2018-11-03 23:53:19
categories:
  - git
  - hexo
tags:
  - 个人主页
---

之前一直不知道为什么部署到github的主页点分类或者链接会无法访问，原来hexo部署生成的html访问的都是`http`协议的，github关闭了http的访问

<!--more-->

只需要将以下代码加入主题文件`_layout.swig`文件中底部即可,位置是`E:\blog\themes\next\layout`，`E:\blog\`是我hexo的位置

```
  <!-- 强制使用https访问 -->
  <script type="text/javascript">
    (function() {
		//这里是你的域名，发布后记得清一下缓存F12看一下有没有代码有没有加上去
    var host = "iszengmh.github.io";
    if ((host == window.location.host) && (window.location.protocol != "https:"))
        window.location.protocol = "https";
    })(); 
</script>
```
![1.png](hexo之如何强制访问https/1.png)
![1.png](hexo之如何强制访问https/2.png)

