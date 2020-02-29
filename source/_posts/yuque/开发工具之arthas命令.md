
---

title: 开发工具之arthas命令

urlname: tfnv65

date: 2019-07-02 15:25:34 +0800

tags: []

---
<a name="tSiLs"></a>
# 开发工具之arthas命令
<a name="N1m6c"></a>
# 参考链接
[arthas官方文档——arthas](https://alibaba.github.io/arthas/)<br />[arthas官方在线模拟器教程——arths](https://alibaba.github.io/arthas/arthas-tutorials?language=cn)<br />
<a name="shuQ1"></a>
# 概述
> 当你遇到以下类似问题而束手无策时，Arthas可以帮助你解决： <br />
> 1. 这个类从哪个 jar 包加载的？为什么会报各种类相关的 Exception？
> 1. 我改的代码为什么没有执行到？难道是我没 commit？分支搞错了？
> 1. 遇到问题无法在线上 debug，难道只能通过加日志再重新发布吗？
> 1. 线上遇到某个用户的数据处理有问题，但线上同样无法 debug，线下无法重现！
> 1. 是否有一个全局视角来查看系统的运行状况？
> 1. 有什么办法可以监控到JVM的实时运行状态？
> 
Arthas支持JDK 6+，支持Linux/Mac/Winodws，采用命令行交互模式，同时提供丰富的 Tab 自动补全功能，进一步方便进行问题的定位和诊断。

<a name="UxmlN"></a>
## 安装及运行
<a name="hHWYN"></a>
### 安装
windows版本需要从官方下载一个压缩包，解压即可<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1562053000011-6b93ae1e-c3eb-4851-8e60-5a4e0f2800ef.png#align=left&display=inline&height=336&name=%E5%9B%BE%E7%89%87.png&originHeight=336&originWidth=748&size=22442&status=done&width=748)
<a name="uScch"></a>
### 运行
可以使用默认提供的`as.bat`文件，不过需要手动输入JVM进程的pid，也可以采用我下面自己写的，保存为bat批处理文件，只需知道pid，运行选择数字即可<br />

```powershell
:A
java -jar arthas-boot.jar
goto A
pause
```

实际运行效果，下图“org.apache.catalina.startup.bootsrap”是我现在的tomcat，因为只运行了一个JVM，非常好选择，直接输入1进入arthas仪表盘即可。<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1562053490019-4276d91b-15fb-45a5-b981-23772fa31bcf.png#align=left&display=inline&height=429&name=%E5%9B%BE%E7%89%87.png&originHeight=429&originWidth=667&size=6993&status=done&width=667)<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1562053592838-41334dd7-b780-4956-9663-31c1f5795661.png#align=left&display=inline&height=481&name=%E5%9B%BE%E7%89%87.png&originHeight=481&originWidth=731&size=16430&status=done&width=731)

<a name="DorZF"></a>
## 命令行
<a name="MCK8S"></a>
### redefine重新加载class文件到JVM中

```powershell
redefine D:/tomcat/webapps/test/WEB-INF/classes/com/kingzheng/fsjscx/util/JSON.class
```

1. redefine只能重新加载已经存在JVM内存的中class，没有相应的class时，会报错

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1562053771798-c118b453-6e64-4560-a068-eab5d572c1a8.png#align=left&display=inline&height=136&name=%E5%9B%BE%E7%89%87.png&originHeight=136&originWidth=699&size=3512&status=done&width=699)

2. 当已经加载过的类，是有编译错误的，也是无法重新加载的

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1562056224413-a8768fb2-3936-46e9-83a2-602c43218a9f.png#align=left&display=inline&height=82&name=%E5%9B%BE%E7%89%87.png&originHeight=82&originWidth=717&size=2558&status=done&width=717)

