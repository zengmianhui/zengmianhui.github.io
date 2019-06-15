
---

title: mysql之绿色版

date: 2019-01-28 03:26:51 +0800

tags: mysql,数据库

categories: mysql

---


<a name="ea6f3b87"></a>
# 参考链接
[MySQL 5.7.20绿色版安装详细图文教程](https://www.jb51.net/article/129367.htm?utm_medium=referral)  <br />[mysql安装常见问题（系统找不到指定的文件、发生系统错误 1067 进程意外终止）——CSDN@MikanMu](https://blog.csdn.net/mhmyqn/article/details/17043921)  

<a name="58378f0d"></a>
# 正文
<a name="6b2ff274"></a>
## 下载mysql解压版
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548617933737-b535ebc1-0570-447e-aad6-b4be74e0ad46.png#align=left&display=inline&height=488&name=image.png&originHeight=488&originWidth=1026&size=91231&status=done&width=1026)

<!--more-->

<a name="bd872cff"></a>
## 解压mysql，并在根目录创建my.ini

```shell
[client]
port=3306
default-character-set=utf8
[mysqld] 
# 设置MYSQL安装目录 
basedir=D:\Program Files (x86)\mysql-5.7.20
# 设置MYSQL数据目录 
datadir=D:\Program Files (x86)\mysql-5.7.20\data
port=3306
character_set_server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER
#开启查询缓存
explicit_defaults_for_timestamp=true
skip-grant-tables
```
<a name="914c1a08"></a>
## 创建mysql服务
以管理员身份证运行cmd，输入以下命令，创建mysql服务，并启动

```shell
cd D:\Program Files (x86)\mysql-5.7.20\bin
mysqld -install
net start mysql
```
<a name="7fc88aee"></a>
## 修改密码
初次系统是默认用户是root，默认没有密码，以管理员身份证运行cmd，输入以下命令

```
mysql

```


