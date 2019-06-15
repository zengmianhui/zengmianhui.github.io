
---

title: svn之svn server重装系统恢复

date: 2019-02-19 11:02:06 +0800

tags: svn,版本控制工具

categories: svn

---
<a name="bd1bf7e7"></a>
# 转载链接
[windows操作系统重装后恢复svn仓库、tortoisesvn客户端信息、及权限信息的方法——博客园@mni_Ya](http://www.cnblogs.com/painwhy/p/4085873.html)  
<a name="019fa851"></a>
# svn之svn server重装系统恢复
<a name="0585c62f"></a>
## **SVN仓库信息的恢复（使用visual svn作为svn服务器的）**
前提：必须要有原来的仓库文件<br />a.下载与原来同样版本的visual svn或者下载最新的visualsvn 。因为你不知道原来的visualsvn的版本，所以安装旧的版本是无法恢复仓库信息的，提示错误 Expected FS format between '1' and '3'; found format '4'。<br />visual svn下载地址：[https://www.visualsvn.com/server/](https://www.visualsvn.com/server/)<br />b.安装完之后把原来的仓库文件放入新的仓库位置

<!--more-->

<a name="ae5a8d41"></a>
## svn客户端信息的恢复（使用tortoiseSvn作为客户端）
前提必须要有原来的svn文件<br />只要安装和从前tortoiseSvn一个版本的就好，版本信息可以在你的文件中的svn目录中看到，为隐藏目录<br />tortoiseSvn下载地址：[http://tortoisesvn.net/downloads.html](http://tortoisesvn.net/downloads.html)
<a name="41ed8f57"></a>
## svn权限恢复
只要你记得原来的用户名密码可以直接用，要是不记得使用visualsvn建立一个新的用户分配权限，调节成没有权限是不行的<br /> <br />这是笔者重装了一次操作系统，总结出来恢复svn信息的方法，希望对遇到同样问题的人有帮助。


