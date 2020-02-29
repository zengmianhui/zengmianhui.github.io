
---

title: oracle之重装系统后恢复

urlname: ivm3hq

date: 2019-02-19 23:12:33 +0800

tags: [oracle,数据库]

categories: oracle

---


<a name="ea6f3b87"></a>
# 参考链接
[Oracle数据库冷备份与恢复（救命稻草）——博客园@Hellow world](https://www.cnblogs.com/arxive/p/9437152.html?tdsourcetag=s_pctim_aiomsg)  （这篇文章参考测试了很久，很遗憾没有成功）<br />[重装系统后ORACLE的恢复——CSDN@haiross](https://blog.csdn.net/haiross/article/details/18550523)  （注：用户能够登录了，但是在navicat中双击用户查看表时报错了，错误忘记了好像是“sql查询错误[][][][][]”，没有成功，但是有了启发，能登录说明这个方法可能是可行的）<br />[Windows Server 2012上安装.NET Framework 3.5——CSDN@lvmenglong888](https://blog.csdn.net/sunny_lv/article/details/73603360)<br />[Oracle监听服务无法启动——博客园@Leis](https://www.cnblogs.com/leis/p/5525844.html)  （注：试过问题二改注册表没有成功）<br />[Oracle监听服务无法启动——ITPUB博客@chenoracle](http://blog.itpub.net/29785807/viewspace-2131834/)  （注：试过没有成功，但是有了启发，觉得可能是监听配置有问题）<br />[ORA-12514: TNS:监听程序当前无法识别连接描述符中请求的服务——CSDN@麦田](https://blog.csdn.net/a921122/article/details/51878992)  （注：没有成功）<br />[关于Win7 64位下：Navicat无法连接64位Oracle 11gR2：Cannot load OCI DLL 87 解决方法——CSDN@a921122](https://blog.csdn.net/a921122/article/details/51878992)  （注：后面发现不是oracle问题，可能是oracle配置变更了navicat无法访问）<br />[处理错误：ORA-27101: shared memory realm does not exist 解决方](https://www.cnblogs.com/loveling-0239/p/6547409.html)[案](https://www.cnblogs.com/loveling-0239/p/6547409.html)[——博客园@AlgorithmInit](https://www.cnblogs.com/loveling-0239/p/6547409.html)  （注：成功）<br />[ORA-27101: shared memory realm does not exist 错误的处理——博客园@lpioneer](https://blog.csdn.net/lpioneer/article/details/6109933)（注：这篇看过不太一样，但是有试过没有成功） 

<a name="c83d32c2"></a>
# oracle之重装系统后恢复
<a name="59683fbc"></a>
## 简述
oracle恢复我也是模模糊糊的，毕竟我对oracle不是很熟悉，不一定保证能成功，如果想要百分百成功的可以不用看下去。<br /><!--more-->
<a name="a96b396f"></a>
## 具体操作分析

1. 第一次我是采用“[Oracle数据库冷备份与恢复（救命稻草）——博客园@Hellow world](https://www.cnblogs.com/arxive/p/9437152.html?tdsourcetag=s_pctim_aiomsg)  ”中冷恢复法，测试了很久没有成功
1. 第二次我是采用“[重装系统后ORACLE的恢复——CSDN@haiross](https://blog.csdn.net/haiross/article/details/18550523)”用户能够登录了，但是在navicat中双击用户查看表时报错了，错误忘记了好像是“sql查询错误[][][][][]”，没有成功，但是有了启发，能登录说明这个方法可能是可行的，我也有了怀疑，可能是重装的oralce配置跟之前系统的还是有区别，然后仔细思考，**安装oracle的时候一直提示“.net framework 3.5无法安装”**
1. 在服务器上安装**.net framework 3.5**，并将oracle卸载了，注册表也删除干净，重新启动
1. **重新安装oracle，创建与旧oracle一模一样的实例，停掉所有服务，将旧oracle安装目录整个替换为新oracle，直接重新启动服务器，启动完成后查看服务，**发现只有监听程序没有启动，尝试启动后失败提示“启动服务失败，无法找到对应文件”
1. 参考文件“[Oracle监听服务无法启动——ITPUB博客@chenoracle](http://blog.itpub.net/29785807/viewspace-2131834/)”后怀疑1521端口被占用，遂使用“net Configuration Assistant”删除1521监听，重新创建一个1522新监听，测试没有成功，但是测试连接ip:1521/orcl时居然成功旧数据也可以查询，但是我已经删除1521的监听程序，我发现其他实例连接不行，仔细查看发现居然三个监听服务，“OracleOraDb11g_home1TNSListener”、“OracleTNSListener”、“OracleTNSListenerLISTNER2”；经测试发现在我创建“OracleTNSListenerLISTNER2”时，oracle自动帮我创建了“OracleTNSListener”，这个明显是1521端口监听程序，仔细思考1521可以那么“OracleTNSListener”一定可以使用，遂删除“OracleTNSListenerLISTNER2”监听程序
1. 在通过sqlplus尝试连接其他实例jzcc，报错“ORA-27101: shared memory realm does not exist”，参考“[处理错误：ORA-27101: shared memory realm does not exist 解决方](https://www.cnblogs.com/loveling-0239/p/6547409.html)[案](https://www.cnblogs.com/loveling-0239/p/6547409.html)[——博客园@AlgorithmInit](https://www.cnblogs.com/loveling-0239/p/6547409.html)  ”，测试成功，原来恢复后，实例服务虽然打启动了，但是实例数据库没有打开，需要用sysdba登录 ，调用startup打开数据库实例。

![](https://cdn.nlark.com/yuque/0/2019/png/244275/1550592701794-be5b7d15-cc1a-45dc-ac6a-e255ccb64f73.png#align=left&display=inline&height=449&originHeight=449&originWidth=679&size=0&status=done&width=679)<br />![](https://cdn.nlark.com/yuque/0/2019/png/244275/1550592738223-a093d680-6243-483f-8532-811d71fcb822.png#align=left&display=inline&height=316&originHeight=316&originWidth=613&size=0&status=done&width=613)
<a name="25f9c7fa"></a>
## 总结

1. 需要保存跟之前一样的环境，一样配置，即操作系统、计算机名、oracle版本（桌面版或者是企业版必须一致）、windows的配置(.net framework 3.5)
1. 重新安装oracle，创建与旧oracle一模一样的实例，停掉所有服务，将旧oracle安装目录整个替换为新oracle，直接重新启动服务器。
1. 此时你可能会发现，只有监听服务没有启动，重新配置监听，使用“net Configuration Assistant”删除1521监听，重新建立1521监听
1. 查看oracle实例服务是否启动，已经启动，进入cmd管理员

```powershell
set oracle_sid=sid
sqlplus / as sysdba
startup
```

以下图片中代码与上面相同

![](https://cdn.nlark.com/yuque/0/2019/png/244275/1550592738223-a093d680-6243-483f-8532-811d71fcb822.png)

