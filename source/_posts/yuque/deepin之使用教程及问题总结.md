
---

title: deepin之使用教程及问题总结

urlname: za9qlc

date: 2019-10-17 15:39:34 +0800

tags: [操作系统,deepin,教程]

categories: 操作系统

---




<a name="vp6dp"></a>
# deepin之使用教程及问题总结
<a name="s8e6v"></a>
## 参考链接
[Deepin Linux 重命名挂载磁盘——简书@白帽札记  ](https://www.jianshu.com/p/6efd382094ec)

[Deepin Linux 重命名挂载磁盘——简书@白帽札记 ](1)

<a name="qhenZ"></a>
### 教程
<a name="ry4Qf"></a>
#### 如何重命名磁盘名称
<a name="ZlvFL"></a>
##### 方法1 

1. 首先右键所需磁盘名称，找到“卸载/unmount”，先卸载了磁盘,这要注意是有软件安装在此磁盘中并已经打开，或者在此磁盘中的文件是否已经打开，需要先关闭所有软件和文件<br />

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1571299423165-3f20c34d-5331-4fef-b19f-7a08d2f51189.png#align=left&display=inline&height=281&name=image.png&originHeight=281&originWidth=367&size=24979&status=done&width=367)

1. 右键所需磁盘，点击“重命名/Rename”

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1571299639638-3177d7dc-ab36-400d-97e6-835b5716f670.png#align=left&display=inline&height=253&name=image.png&originHeight=253&originWidth=397&size=26197&status=done&width=397)

3. 输入需要修改的磁盘名称，点击回车，会弹出需要输入管理员密码，点击“确认/comfirm”即可![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1571299968748-f8e60534-ff57-45d4-8785-02fe0285fae5.png#align=left&display=inline&height=350&name=image.png&originHeight=350&originWidth=722&size=50927&status=done&width=722)
<a name="Grnzq"></a>
##### 方法2（未验证）
[Deepin Linux 重命名挂载磁盘——简书@白帽札记  ](https://www.jianshu.com/p/6efd382094ec)

1. 查看当前所有分区

sudo fdisk -l

2. 先卸载要修改名称的分区：

sudo umount /dev/sda5

3. 修改名称:

sudo ntfslabel /dev/sda5 music<br />注：ntfslabel会修改名称后自动重新加载，不用再执行mount命令

<a name="5yOub"></a>
### 问题总结
<a name="rzFLn"></a>
#### 使用su root代码时，出现authentication failure
su命令不能切换root，提示su: Authentication failure，只要你sudo passwd root过一次之后，下次再su的时候只要输入密码就可以成功登录了。<br />![](https://cdn.nlark.com/yuque/0/2019/png/244275/1571301542073-fd5d16aa-39ab-46db-9817-2c8492c1371f.png#align=left&display=inline&height=210&originHeight=210&originWidth=405&size=0&status=done&width=405)



