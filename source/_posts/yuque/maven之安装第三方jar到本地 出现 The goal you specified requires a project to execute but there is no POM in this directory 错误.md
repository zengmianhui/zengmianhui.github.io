
---

title: maven之安装第三方jar到本地 出现 The goal you specified requires a project to execute but there is no POM in this directory 错误

date: 2019-03-05 23:19:33 +0800

tags: maven,oracle

categories: maven

---


<a name="bd1bf7e7"></a>
# 转载链接

[https://www.cnblogs.com/demonrain/p/5674091.html——博客园@demonrain](https://www.cnblogs.com/demonrain/p/5674091.html)

<a name="a1db7c12"></a>
# maven之安装第三方jar到本地 出现 The goal you specified requires a project to execute but there is no POM in this directory 错误

```powershell
mvn install:install-file "-Dfile=cobra.jar" "-DgroupId=com.cobra" "-DartifactId=cobra" "-Dversion=0.98.4" "-Dpackaging=jar" "-DgeneratePom=true"
```


