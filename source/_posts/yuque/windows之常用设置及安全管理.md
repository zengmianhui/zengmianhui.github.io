
---

title: windows之常用设置及安全管理

urlname: ich5xw

date: 2019-07-13 22:02:13 +0800

tags: []

---
<a name="T0vEc"></a>
# 参考链接
[Windows Server 2008 R2远程协助选项灰色——百度经验@☆落墨云烟★](https://jingyan.baidu.com/article/f3e34a12a75dd5f5ea653570.html)<br />[Windows Srever 2008 R2 服务器远程桌面连接，仅允许运行使用网络级别身份证的远程桌面计算机连接失败——百度@](https://zhidao.baidu.com/question/1388826106841180660.html#best-answer-2987455643)[匿名用户](https://zhidao.baidu.com/question/1388826106841180660.html#best-answer-2987455643)<br />[打开允许远程后仍然无法远程连接——百度经验@小明仔sweety](https://jingyan.baidu.com/article/380abd0a1a14631d91192c4a.html)
<a name="wTlYJ"></a>
# windows之常用设置及安全管理
<a name="SYxPo"></a>
## [Windows Server 2008 R2允许远程协助选项灰色](https://jingyan.baidu.com/article/f3e34a12a75dd5f5ea653570.html)
<a name="CWEa2"></a>
### 方法一
在“服务器管理器——功能”右键选择“添加功能”，勾选“远程协助”，点击“下一步”，等待安装完成即可<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563026746618-4c1ba175-f1cb-471a-8400-b6cfc0f722ab.png#align=left&display=inline&height=580&name=%E5%9B%BE%E7%89%87.png&originHeight=580&originWidth=478&size=28044&status=done&width=478)<br />效果如下<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563026893036-0fce47bb-6323-4c38-8b97-b8d6768e8188.png#align=left&display=inline&height=464&name=%E5%9B%BE%E7%89%87.png&originHeight=464&originWidth=407&size=10124&status=done&width=407)
<a name="nojxo"></a>
### 方法2
“控制面板——启用或者关闭windows功能”，一直点击“下一步”直到“功能”，选择“远程协助”，点击“安装”，等待时间即可。

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563027106865-2db77639-753f-402d-9164-f66a049bb375.png#align=left&display=inline&height=573&name=%E5%9B%BE%E7%89%87.png&originHeight=573&originWidth=802&size=35054&status=done&width=802)
<a name="FSwqt"></a>
## [“仅允许运行带网络级身份的远程桌面的计算机连接”按钮灰色](https://zhidao.baidu.com/question/1388826106841180660.html#best-answer-2987455643)
[Windows Srever 2008 R2 服务器远程桌面连接，仅允许运行使用网络级别身份证的远程桌面计算机连接失败——百度@](https://zhidao.baidu.com/question/1388826106841180660.html#best-answer-2987455643)[匿名用户](https://zhidao.baidu.com/question/1388826106841180660.html#best-answer-2987455643)<br />按win+R，输入“gpedit.msc”，进入“计算机配置”——“管理模版"——"Windows组件"——“远程桌面服务”——“远程桌面会话主机”——“安全”

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563086761118-59643044-22ab-4ca5-ba53-317db8c57dfd.png#align=left&display=inline&height=550&name=%E5%9B%BE%E7%89%87.png&originHeight=550&originWidth=762&size=22320&status=done&width=762)<br />![](https://cdn.nlark.com/yuque/0/2019/png/244275/1563086789327-a4c66720-b99b-4de9-8283-7a9d3fbed02d.png#align=left&display=inline&height=493&originHeight=493&originWidth=426&status=done&width=426)
<a name="qacmY"></a>
## 如何设置远程桌面连接
<a name="yhzKb"></a>
### 允许远程协助
![](https://cdn.nlark.com/yuque/0/2019/png/244275/1563026893036-0fce47bb-6323-4c38-8b97-b8d6768e8188.png#align=left&display=inline&height=464&originHeight=464&originWidth=407&status=done&width=407)
<a name="yN1Ob"></a>
### 组策略中设置“允许用户使用远程桌面服务进行远程连接”
按win+R，输入“gpedit.msc”，进入“计算机配置”——“管理模版"——"Windows组件"——“远程桌面服务”——“远程桌面会话主机”——“连接”<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563087481412-9d9a71a0-2736-43d4-9bdc-cfc4681fb0af.png#align=left&display=inline&height=551&name=%E5%9B%BE%E7%89%87.png&originHeight=551&originWidth=672&size=20931&status=done&width=672)
<a name="6zqv0"></a>
### 设置“[仅允许运行带网络级身份的远程桌面的计算机连接](https://zhidao.baidu.com/question/1388826106841180660.html#best-answer-2987455643)”
![](https://cdn.nlark.com/yuque/0/2019/png/244275/1563086789327-a4c66720-b99b-4de9-8283-7a9d3fbed02d.png#align=left&display=inline&height=493&originHeight=493&originWidth=426&status=done&width=426)
<a name="i9fJO"></a>
### 设置可远程的用户
注：无论是否提示“administrator已经有访问权”，都需要添加一次用户。<br />方法：“控制面板”——“系统”——“远程设置”——“选择用户”——点击“添加”——点击“高级”——点击“立即查找”

<a name="6PLfm"></a>
### 确保远程服务正在运行
[打开允许远程后仍然无法远程连接——百度经验@小明仔sweety](https://jingyan.baidu.com/article/380abd0a1a14631d91192c4a.html)<br />win+R输入“services.msc”，查看以下服务是否已启动，未启动的需要启动，并设置为“自动”

Remote Desktop Configuration、Remote Desktop Services、Remote Desktop Services UserMode Port Redirector<br />![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563088538032-947955e5-6038-41ff-aeaa-b6d01cde362f.png#align=left&display=inline&height=229&name=%E5%9B%BE%E7%89%87.png&originHeight=229&originWidth=661&size=16714&status=done&width=661)
<a name="UIjkP"></a>
### [设置凭据分配](https://jingyan.baidu.com/article/380abd0a1a14631d91192c4a.html)
[打开允许远程后仍然无法远程连接——百度经验@小明仔sweety](https://jingyan.baidu.com/article/380abd0a1a14631d91192c4a.html)<br />1、win+R输入“gpedit.msc”<br />2、点击“计算机配置”—“管理模板”—“系统”—“凭据分配”。双击右边窗口的“允许分配保存的凭据用于仅NTLM 服务器身份验证”。<br />3、在弹出的窗口中选中“已启用”，再单击“显示”，在弹出的窗口中，输入“TERMSRV/*”。（确保TERMSRV 为大写，在对话框右侧能找到这个‘单词’）<br />4、管理员运行“cmd”，输入“gpupdate /force”刷新组策略，或者重启计算机

![图片.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1563088180346-0a89e16d-329a-4372-88dc-892a517e56cd.png#align=left&display=inline&height=491&name=%E5%9B%BE%E7%89%87.png&originHeight=491&originWidth=1074&size=30075&status=done&width=1074)

<a name="KVlb2"></a>
## 文章还在不停更新中



