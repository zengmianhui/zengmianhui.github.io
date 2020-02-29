
---

title: oracle之技术经验

urlname: yd4suv

date: 2019-02-14 12:05:59 +0800

tags: []

---
<a name="f66bd8f4"></a>
# ****O****ra****cle****经验****
 
<a name="e9a9187c"></a>
## ****申明****
本文大部分资料来自《深入理解Oracle 12c数据库管理》，但是也有自己的个人观点，大家也去看这本书
<a name="e05dce83"></a>
## ****简介****
	Oracle数据库已经是当今世界技术前沿了，因为它优点突出 <br />有以下优点：<br />(1) 拥有其他数据库系统所没有的表空间概念；<br />(2) 拥有真正的等级锁功能<br />(3) 拥有多版本数据功能，读写操作不会相互等待（我觉得是非常的好特性）<br />(4) 拥有更快的处理速度和更高的安全性；<br />(5) 拥有丰富的数据字典，易于DBA判断数据库的各种情况；<br />(6) 拥有非常简单明了的备份与恢复原理<br />(7) Oracle数据库可以启动到多个阶段，DBA可以在不同的情况下，通过启动到特定的阶段解决一些特殊问题<br />(8) Oracle可以跨越多种软、硬平台。

<a name="362eced1"></a>
## ****Oracle****安装和创建****(****由于本文作者觉得linux太花费时间，故只有这部分讲解到linux****)****
Oracle安装一般有两种，一种是图形界面的安装，另一种是无界面安装。建议是无界面，因为图形界面在宽带不足情况下，可能出现加载远程界面慢的问题，而且不能自动化。无界面可以依靠应答文件来安装。
<a name="712f5efa"></a>
### ****了解OFA标准****
OFA标准是指oracle的目录结构和文件名，然而大部分DBA（database manager数据库管理员简称DBA）都在一定程度上自定义了，以适应于不同的环境。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117219355-d3c0e3a0-41c9-45c4-ae0c-a89fe13bccb3.png#align=left&display=inline&height=507&name=image.png&originHeight=507&originWidth=835&size=295404&status=done&width=835)<br /><!--more-->
<a name="119c5730"></a>
### ****库的高速缓存和数据字典的高速缓存****
 
<a name="a55e5de5"></a>
#### ****库的高速缓存****
是用来存放你实际表的数据块的，如表TAB_A里实际存放的若干条数据记录，一般都存放在用户的表空间里。<br /> <br /> 
<a name="773d69a4"></a>
#### ****数据字典的高速缓存****
用来存放表的定义，如表TAB_A，有几个字段，每个字段的类型、长度，表空间等，这类信息在你建表后会存放在系统表里，都是在SYSTEM表空间下，ORACLE运行时，这些信息被装入数据字典高速缓存里。<br /> 
<a name="7f04d897"></a>
#### ****数据字典的意思是****
简单的说就和我们小学用的词典的目录一样  要查询个表的数据 首先要确认这个词典（数据库）中有这个词语（表）  吧<br /> 
<a name="2655d132"></a>
### ****安装oracle****
 
<a name="061f72da"></a>
#### ****创建对应的权限的O****S****用户组****
我们需要linux上创建一些OS用户组，安装完oracle之后就可以为linuxOS用户组分配的相应的数据库操作权限，正常来说OS用户组（注：用户和用户组是不一样的）的创建是属于系统管理员（SA）的工作，但是大部分情况没有SA。<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117248391-cbfc23d8-2f3c-4bb5-b862-65300cfb607d.png#align=left&display=inline&height=488&name=image.png&originHeight=488&originWidth=822&size=254798&status=done&width=822)<br />不必根据一字不差照搬组名，可以根据不一样环境来配置。<br /> 
<a name="657ad920"></a>
##### ****运行linux命令，创建组****
我们只按照简单的功能来分组就好了，oinstall负责安装和卸载权限，dba具有完全操作权限，oper只具有数据库操作权限（包含一些删除表，创建表，修改等待权限 ）<br />groupadd oinstall<br />groupadd dba<br />groupadd oper<br /> 
<a name="ccd2979a"></a>
###### ****查看创建的OS组****
cat /etc/group<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117306714-eef5438b-f88e-43d4-9ecb-0d43990fe65f.png#align=left&display=inline&height=386&name=image.png&originHeight=386&originWidth=281&size=12073&status=done&width=281)<br />1000、1001、1002是我们组的ID
<a name="87c9fadb"></a>
##### ****创建用户并分配组****
useradd -u 500 -g oinstall -G dba , oper oracle<br />将组ID设置500(其他同事可能需要人执行相同的组ID来执行所有安装)<br />创建主属组为oinstall，创建副属组为dba,oper<br />-g 和-G，分别是分配主属组和附属组的意思。
<a name="d82e410e"></a>
###### ****查看用户信息****
cat /etc/passwd<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117322832-e84ea737-28ec-450b-9087-7f6f9c5272ea.png#align=left&display=inline&height=385&name=image.png&originHeight=385&originWidth=827&size=30460&status=done&width=827)<br /> 
<a name="e7d20343"></a>
###### ****删除修改用户，或者用户组****
修改删除用户组：groupmod、groupdel<br />修改删除用户：usermod、userdel<br />以上命令需要使用系统管理员登录
<a name="11c5905d"></a>
#### ****查看linux环境信息****
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117345109-376aba41-14b7-46f6-9e38-8141c9886cfb.png#align=left&display=inline&height=401&name=image.png&originHeight=401&originWidth=860&size=45732&status=done&width=860)以下为我本人系统的信息<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117364504-14c00655-46d0-4c60-a248-84661ea1cb04.png#align=left&display=inline&height=452&name=image.png&originHeight=452&originWidth=666&size=29951&status=done&width=666)<br />grep MemTotal  /proc/meminfo<br />free –t<br />df –h /tmp<br />df –h<br />cat /proc/version<br />uname –r<br /> <br />rpm –q <package name> 查询是否已经安装必须的软件包
<a name="fdd1ae5b"></a>
## ****管理数据库****
 
<a name="dfc119cb"></a>
### ****S****ysdba数据库账号****
这个账号可拥有除了关闭数据库以外的所有操作权限<br />as sysdba作为系统管理员登录
<a name="6aee7bf5"></a>
### ****第一次操作数据库****
startup onmount 启动后台进程并分配内存，此命令执行后，sql*plus会读取ORACLE_HOME/dbs中的初始化文件，会使后台进程和内存区域初始化，这样你就拥有了oracle的实例，但是还没有数据库<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117384566-27b7d3ce-0863-4a79-ac04-4ec4bfc56b98.png#align=left&display=inline&height=228&name=image.png&originHeight=228&originWidth=603&size=11983&status=done&width=603)<br />oracle实例是指后台进程和内存区域，oracle数据库是指磁盘上的物理文件（数据文件、控制文件、联机重做日志文件）
<a name="187ec332"></a>
### ****表空间****
<a name="fcf86c6f"></a>
#### ****查询TEMP临时表空间****
select*from database_properties where property_name='DEFAULT_TEMP_TABLESPACE';<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117405819-4e8691dc-3239-445c-b58e-e77a25fa45f4.png#align=left&display=inline&height=202&name=image.png&originHeight=202&originWidth=620&size=7894&status=done&width=620)
<a name="5d4d5693"></a>
#### ****USER表空间****
select*from database_properties<br />where roperty_name='DEFAULT_PERMANENT_TABLESPACE';<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117417926-a850da9d-5066-484f-9f27-e1aedcadea79.png#align=left&display=inline&height=172&name=image.png&originHeight=172&originWidth=632&size=7080&status=done&width=632)
<a name="00caab79"></a>
### ****连接标识****
<a name="8d258c38"></a>
#### ****Oracle 的OCI Driver 和 Thin Driver的区别****
有以下两种标识方式：<br /> <br />jdbc:oracle:oci8:@shdb<br /> <br />1）从使用上来说，oci必须在客户机上安装oracle客户端或才能连接，而thin就不需要，因此从使用上来讲thin还是更加方便，这也是thin比较常见的原因。 <br />2）原理上来看，thin是纯java实现tcp/ip的c/s通讯；而oci方式,客户端通过native java method调用c library访问服务端，而这个c library就是oci(oracle called interface)，因此这个oci总是需要随着oracle客户端安装（从oracle10.1.0开始，单独提供OCI Instant Client，不用再完整的安装client） <br />3）它们分别是不同的驱动类别，oci是二类驱动， thin是四类驱动，但它们在功能上并无差异。<br /> 
<a name="3a0f1856"></a>
### ****查询实例名（SID）****
sqlplus / as sysdba<br />show parameter instance_name<br /> 
<a name="f858894c"></a>
## ****配置用户和数据库对象****
<a name="f8876e5d"></a>
## ****创建和维护大型数据库对象、分区和索引****
<a name="d2adec7f"></a>
## ****备份和恢复****
<a name="01a62dbc"></a>
## ****数据库自动化及常见问题解决方法****
<a name="9d2f273a"></a>
## ****管理窗口和可插入数据库****
<a name="d579eb7d"></a>
## ****Oracle 导入导出及连接****
<a name="ea6f3b87"></a>
### ****参考链接****
[Oracle数据库导入导出命令总结](http://blog.itpub.net/21614165/viewspace-766937/)<br />[sqlplus连接远程数据库](http://blog.csdn.net/wildin/article/details/5850252)<br />[ORACLE的impdp和expdp命令](http://www.cnblogs.com/wanghongyun/p/6307652.html)<br />[oracle expdp——红黑联盟](https://www.2cto.com/database/201304/203709.html)
<a name="02babed4"></a>
### ****exp 和imp导入导出****
<a name="f77e0e7c"></a>
#### ****导出命令 (exp)****
****格式：****

```
exp [用户名]/[密码]@[主机ip]:[端口号]/[SID/service] file=d:\zhpt.dmp full=n
```

file是导出路径full=n,表示是否导出主机数据库上全部用户，n表示否，y表示是<br />exp abc/abc@183.233.179.165:1521/orcl file=d:\zhpt.dmp full=y<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117435502-1497acdc-62e6-4f06-bcb2-86eb61c72693.png#align=left&display=inline&height=34&name=image.png&originHeight=34&originWidth=625&size=2889&status=done&width=625)<br />如果密码出特殊符号，使用`"""`包裹，如果其他地址有特殊符号，需要用`\`转义，需要指定用户导出可以使用`owner`

```powershell
exp grz/"""g2011*)"""@19.129.180.19:1521/oracle file=gr20190410.dmp owner in \(\'gr\',\'jzs'\) full=n
```

<a name="606fd7a7"></a>
#### ****导入数据库（imp）****

```powershell
# full 表示是否导出全部数据，一定要设置
# log 输出日志文件# fromuser 从哪一个用户导入
# touser 导入到哪个用户
# ignore=y buffer=100000000; 修改缓冲区大小，有时sql语句过长，会造成缓冲区空间不足
imp username/pwd@orcl file=d:\zhpt.dmp log=C:\data\logname.log full=y
# 或者
imp username/pwd@orcl file=E:\20171108.dmp fromuser=username touser= username log=D:\webBackend\kingzheng\住房保障系统\fszfbz201711191635.log full=n
# 或者
imp username/pwd@orcl file=d:\zhpt.dmp log=C:\data\logname.log full=y ignore=y buffer=100000000;
```
 
<a name="69a49bc6"></a>
### ****expdp和impdp创建数据泵导入导出**** ****
<a name="f0c6d9cb"></a>
#### ****需要先创建数据泵****
数据泵，说白了就是指定一个目录给oracle，但是oracle不会帮你创建的，需要自己先实际地创建

```sql
#  查看所有数据泵地址
select * from dba_directories;# 创建数据泵，数据泵地址即为你的导出导入地址文件地址
create directory myname as 'D:\temp\数据泵地址';# 授予权限 sshe这个用户可读可写
grant read,write on directory dpdata1 to sshe;

sql>--可以使用以下语句查看目录操作权限
sql>  SELECT privilege, directory_name, DIRECTORY_PATH FROM user_tab_privs t, all_directories d WHERE t.table_name(+) = d.directory_name ORDER BY 2, 1; 
```

**注意：** 数据泵地址以及文件dmp需要自己创建
<a name="837cb31c"></a>
#### ****导出数据（expdp）****
这种数据泵效率非常高，但是使用这种数据泵导出的数据，****一般情况下只在本机导出****，需要用impdp导入

```
rem my_dir是数据泵名称

rem exclude table:"in(表名,列名2，……)"不导出某些表

expdp test/test@orcl directory=my_dir dumpfile=my.dmp exclude=table:\"in \(\'DEPT\',\'EMP\'\)\" SCHEMAS=FSJSCX
```

<a name="Impdp"></a>
#### ****Impdp****
跟expdp的语法格式差不多

```
 impdp test/test@orcl DIRECTORY=my_dir  DUMPFILE=my.dmp SCHEMAS=test logfile=%logfile%
```
 
<a name="d303267a"></a>
##### ****问题：****
<a name="29398c59"></a>
###### ****这些对象由 FSZJZ 导出, 而不是当前用户****
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117805403-46bf4a8c-9f04-4d1a-9def-916cf1fd0a66.png#align=left&display=inline&height=134&name=image.png&originHeight=134&originWidth=517&size=10915&status=done&width=517)<br />导出是哪个用户，导入时用户也要相同，需要自己再创建一个用户<br /> 
<a name="2e846665"></a>
###### ****只有管理员用户，才可以导入****
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550117815821-b03a30e3-3157-4e16-8e5d-4cfb4766f698.png#align=left&display=inline&height=80&name=image.png&originHeight=80&originWidth=412&size=4702&status=done&width=412) 
<a name="5b692eb8"></a>
###### ****ora-28759 无法打开文件****
以下这两句可能在不同的操作系统，支持不同，不太清楚，我服务器，两个都是sever2008，但是只有一个报这个错误，这个报错确实跟用户连接有关系，****最好是采用second****<br /> 
<a name="88e73bd4"></a>
### ****sqlplus 远程连接数据库****
<a name="ce1ec2ce"></a>
#### ****远程连接****

```
命令：sqlplus 用户名/密码@ip地址[:端口]/service_name [as sysdba]

示例：sqlplus sys/pwd@ip:1521/test as sysdba
```

<a name="0dfbe902"></a>
#### ****常用命令****

```
SQL>set colsep' ';　　　　 //-域输出分隔符

SQL>set echo off;　　　　 //显示start启动的脚本中的每个sql命令，缺省为on

SQL> set echo on              //设置运行命令是是否显示语句

SQL> set feedback on;       //设置显示“已选择XX行”

SQL>set feedback off;　    //回显本次sql命令处理的记录条数，缺省为on

SQL>set heading off;　　 //输出域标题，缺省为on

SQL>set pagesize 0;　　    //输出每页行数，缺省为24,为了避免分页，可设定为0。

SQL>set linesize 80;　　   //输出一行字符个数，缺省为80

SQL>set numwidth 12;　    //输出number类型域长度，缺省为10

SQL>set termout off;　　   //显示脚本中的命令的执行结果，缺省为on

SQL>set trimout on;　　　//去除标准输出每行的拖尾空格，缺省为off

SQL>set trimspool on;　　//去除重定向（spool）输出每行的拖尾空格，缺省为off

SQL>set serveroutput on; //设置允许显示输出类似dbms_output

SQL> set timing on;          //设置显示“已用时间：XXXX”

SQL> set autotrace on;    //设置允许对执行的sql进行分析

SQL> set verify off;           //可以关闭和打开提示确认信息old 1和new 1的显示.
```

导出结果到文本：<br />spool<spool_flat_file><br />例如：spool d:\Spool_flatquery.txt<br />这样，SQL*Plus将把所有的输出以及在屏幕上的命令等都指定给该文件。<br />执行查询输出。此时，系统并没有把结果保存到文件中，而是保存到缓冲区中。<br />查询结束后，关闭文件即可。命令格式为：spool off。
<a name="df0cbcf7"></a>
### ****oracle之jdbc连接oracle****
<a name="b5f7449a"></a>
#### ****使用sid方式：****

```
jdbc:oracle:thin:@host:port:SID 

Example: jdbc:oracle:thin:@localhost:1521:orcl 
```

<a name="e0db0567"></a>
#### ****使用服务名方式****
使用服务名的方式，这种格式是Oracle 推荐的格式，因为对于集群来说，每个节点的SID 是不一样的，但是SERVICE_NAME 确可以包含所有节点。

```
jdbc:oracle:thin:@//host:port/service_name

Example:jdbc:oracle:thin:@//localhost:1521/orcl.city.com
```

<a name="859540a4"></a>
#### ****使用TNSName ****
使用[TNSName ]()， 要实现这种连接方式首先要建立tnsnames.ora文件，然后通过System.setProperty指明这个文件路径。再通过上面URL中的@符号指定文件中的要使用到的资源。<br />这种格式我现在水平几乎没见过，对于我来说用得到这种的情况并不多吧。当然既然是通过配置文件来读取指定资源肯定也可以直接将资源拿出来放在URL中，直接放在URL中的URL模版是下面这样的（tnsnames.ora这个文件中放的就是@符号后面的那一段代码，当然用文件的好处就是可以配置多个，便于管理）：

```
jdbc:oracle:thin:@TNSName 

Example: jdbc:oracle:thin:@TNS_ALIAS_NAME

jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL= TCP)(HOST=hostA)(PORT= 1522))(ADDRESS=(PROTOCOL=TCP)(HOST=your host)(PORT=1521)))(SOURCE_ROUTE=yes)(CONNECT_DATA=(SERVICE_NAME=your service_name)))
```

<a name="efbc27b9"></a>
## ****Oracle obj（plsql中解释为对象）****
<a name="019aaceb"></a>
#### ****Function 函数****
 
<a name="d283ab83"></a>
#### ****Procedure 存储过程****
<a name="ea6f3b87-1"></a>
##### ****参考链接****
[Oracle创建存储过程、创建函数、创建包——博客园@helong](https://www.cnblogs.com/helong/articles/2093807.html) <br />[ORACLE执行存储过程权限不足——CSDN@He之涅槃](https://blog.csdn.net/u010109335/article/details/60577055)<br /> 
<a name="e3127cc1"></a>
##### ****格式****

```
create or replace procedure procedure_name(Name in out type, Name in out type, ...) isbegin

  end procedure_name;
```

<a name="1a63ac23"></a>
##### ****示例****

```sql
--自动创建表格，并update数据

--dbms_output.put_line()需要先在command（命令行界面）“set serverout on ”打开输出

create or replace procedure update_qylxid_of_null_for_rygx

Authid Current_User

is

  v_date varchar2(8);--定义日期变量

  v_sql varchar2(2000);--定义动态sql

  v_tablename varchar2(20);--定义动态表名

  begin

   select to_char(sysdate,'yyyymmdd') into v_date from dual;--取日期变量

   v_tablename := 'T_'||v_date;--为动态表命名

   v_sql := 'create table '||v_tablename||'as select*from t_qy';--为动态sql赋值

   dbms_output.put_line(v_sql);--打印sql语句

   execute immediate v_sql;--执行动态sql

   v_sql:='update t_qy t set t.LXID=(select LXID from t_qy_qy lx where lx.bh=t.bh and lx.LX =t.dm) where  t.lxid is null';

   dbms_output.put_line(v_sql);--打印sql语句

   execute immediate v_sql;--执行动态sql

end update_qylxid_of_null_for_rygx;
```

<a name="50d52dd9"></a>
##### ****常见问题****
<a name="1fc98647"></a>
###### ****ORACLE执行存储过程权限不足****
[ORACLE执行存储过程权限不足——CSDN@He之涅槃](https://blog.csdn.net/u010109335/article/details/60577055)

```
--需要增加Authid Current_User

--AUTHID DEFINER （定义者权限）：指编译存储对象的所有者。也是默认权限模式。

--AUTHID CURRENT_USER（调用者权限）：指拥有当前会话权限的模式，这可能和当前登录用户相同或不同(alter session set current_schema 可以改变调用者Schema)create or replace PROCEDURE 存储过程名称

Authid Current_User

IS 

BEGIN

 

……；

END;

 
```

<a name="618d4aaf"></a>
#### ****Database link 数据库链接****
即在需要在两个不同的数据库中连接表或者查询数据时，可创建数据库链接
<a name="481feccf"></a>
##### ****如何使用****

```
--user_tables 是DBLINK_test所链接的用户的表

select * from user_tables@DBLINK_test;

--链接可以方便于多个数据库用户关联查询数据，非常方便,mytable 是你当前登录用户的表

select * from user_tables@DBLINK_test t,mytable t2 where t2.id=t.id;
```

<a name="ea6f3b87-2"></a>
##### ****参考链接****
[oracle中的database link如何使用——百度经验@wangzhiqing999](#best-answer-746405041)
<a name="345cd1b6"></a>
##### ****oracle sql创建****

```

-- Drop existing old  database link --DBLINK_test是database link的名称drop database link DBLINK_test;-- Create new database link -- other_db 为用户名 pwd为密码create database link DBLINK_test

  connect to other_db IDENTIFIED BY  pwd

  using '(DESCRIPTION =

(ADDRESS_LIST =

(ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))

)

(CONNECT_DATA =

(SERVICE_NAME = orcl)

)

)';--查询 database link select * from dba_db_links;
```

如果创建全局dblink，必须使用systm或sys用户，在database前加public

```
create  public  database link DBLINK_test

  connect to other_db IDENTIFIED BY  pwd

  using '(DESCRIPTION =

(ADDRESS_LIST =

(ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))

)

(CONNECT_DATA =

(SERVICE_NAME = orcl)

)

)';
```

<a name="5f005e3c"></a>
##### ****通过plsql创建****
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118003862-eccf0b6e-7132-4106-aa23-e2b350ce0b5b.png#align=left&display=inline&height=408&name=image.png&originHeight=408&originWidth=327&size=22765&status=done&width=327) <br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118072979-b08dec8d-fd6a-4d8f-99cc-50fbc1e4e75c.png#align=left&display=inline&height=376&name=image.png&originHeight=376&originWidth=971&size=30163&status=done&width=971) <br /> 
<a name="146704a5"></a>
## ****常用函数及sql实例****
<a name="ea6f3b87-3"></a>
### ****参考链接****
[Oracle中replace函数的使用](http://www.cnblogs.com/harvey888/p/5957656.html)<br />[Oracle round函数是什么意思?怎么运用?](https://zhidao.baidu.com/question/14756835.html)<br />[oracle的nvl](https://zhidao.baidu.com/question/552089573.html)<br />[Oracle 中 decode 函数用法](http://www.cnblogs.com/vinsonLu/p/3512526.html)<br />[[oracle] to_date() 与 to_char() 日期和字符串转换](http://www.cnblogs.com/gaojing/archive/2008/11/07/1328657.html)<br />[Oracle的Cast的用法](http://blog.csdn.net/ziwen00/article/details/8685858) <br />[Oracle 大小写转换函数——博客园@Twang](https://www.cnblogs.com/wangfuyou/p/6605166.html)
<a name="ca48b68b"></a>
### ****常用函数****
<a name="2a18f8a8"></a>
#### ****判断是否为数字****

```
# 注意只能判断纯数字，不带小数,判断带小数方式请查看下文“常用sql”创建函数

SELECT nvl2(translate('123','/1234567890','/'),'CHAR','NUMBER')   

FROM   dual ;
```

<a name="da5a6765"></a>
#### ****add_months()日期增加，以月为单位****

```
add_months(sysdate,12)--增加一年

add_months(sysdate,-12)--减去一年

sysdate+1 --加一天
```

<a name="c79a4a1b"></a>
#### ****to_date()****

```
to_date("要转换的字符串","转换的格式")

to_date(t.access_date,'yyyy-mm-dd hh24:mi:ss')--2005-12-25 13:25:59

TO_DATE('17-DEC-1980', 'DD-MON-YYYY','NLS_DATE_LANGUAGE=American')--日期语言
```

<a name="b0a57818"></a>
#### ****replace替换字符****

```
replace(原字段，'原字段旧内容','原字段新内容')--替换字符串
```

<a name="fecd35a6"></a>
#### ****round四舍五入****

```
`round(number)``round(number, decimal_places )`

 

number ---需要四舍五入的数字

decimal_places ---从哪里开始四舍五入，此参数是下标，预设为0

 

select round(123.456, 0) from dual;     --- 123 
```

<a name="2510b668"></a>
#### ****nvl如果为空返回新值****
 
```
nvl(字段名，'新的返回的值')

如果提供的字段的值为空，则将返回这个新值，注意：只是返回了这个值，并不是update到表中

 

 nvl(name,'小明')---name为空，返回小明
```

<a name="d623ee97"></a>
#### ****decode逻辑判断简化****

```
decode(条件,值1,返回值1,值2,返回值2,...值n,返回值n,缺省值)

 

该函数的含义如下：IF 条件=值1 THEN

　　　　RETURN(翻译值1)

ELSIF 条件=值2 THEN

　　　　RETURN(翻译值2)

　　　　......

ELSIF 条件=值n THEN

　　　　RETURN(翻译值n)ELSE

　　　　RETURN(缺省值)

END IF

decode(字段或字段的运算，值1，值2，值3）

 

该函数的含义如下：

 这个函数运行的结果是，当字段或字段的运算的值等于值1时，该函数返回值2，否则返回值3

 当然值1，值2，值3也可以是表达式，这个函数使得某些sql语句简单了许多
 注意：值2和值3的数据类型必须一致
```



<a name="4e7db3ce"></a>
#### ****sys_guid()生成唯一32位字符串****

```
sys_guid()
```

<a name="774534e1"></a>
#### ****CAST(expr AS type_name) 数值类型转换****

```
--例

cast(R.MONTH as int)--将月份转换为整型类型
```

<a name="5b106349"></a>
#### ****大小写转换****

```
select UPPER('Test') as u from dual; --转大写

select LOWER('Test') as l from dual;--转小写
```

<a name="7db7c5f3"></a>
#### ****截取字符串****

```
--截取身份证出生日期

to_date(substr('XXXXXXXXXXXXXXXXX',7,8),'YYYYMMDD')
```

<a name="425d8a49"></a>
#### ****删除左右字符、添加左右字符****

```
ltrim(原字符,'需要删除的字符')--删除左边字符

rtrim(原字符,'需要删除的字符')--删除右边字符

LPAD(原字符,'需要添加的字符') --添加字符在左边

RPAD(原字符,'需要添加的字符') --添加字符在右边--例

ltrim('abcdefg','abc')--删除左边abc，输出defg

ltrim('abqwert','abc')--删除左边ab，输出qwert

 

 

 

 

 
```

<a name="349ddcfc"></a>
### ****Sql实例****
<a name="89f6884e"></a>
#### ****判断数字****（****创建函数****）****

```
create or replace function isNumber(p in varchar2)return number

is

result number;begin

result := to_number(p);return 1;

exceptionwhen VALUE_ERROR then return 0;end;
```

导出表结构

```
SELECT B.TABLE_NAME     AS "表名",

       C.COMMENTS       AS "表说明",

       B.COLUMN_ID      AS "字段序号",

       B.COLUMN_NAME    AS "字段名",

       B.DATA_TYPE      AS "字段数据类型",

       B.DATA_LENGTH    AS "数据长度",

       B.DATA_PRECISION AS "整数位",

       B.DATA_SCALE     AS "小数位",

       A.COMMENTS       AS "字段说明"

  FROM ALL_COL_COMMENTS A, ALL_TAB_COLUMNS B, ALL_TAB_COMMENTS C

WHERE A.TABLE_NAME IN (SELECT U.TABLE_NAME FROM USER_ALL_TABLES U)

   AND A.OWNER = B.OWNER

   AND A.TABLE_NAME = B.TABLE_NAME

   AND A.COLUMN_NAME = B.COLUMN_NAME

   AND C.TABLE_NAME = A.TABLE_NAME

   AND C.OWNER = A.OWNER

   AND A.OWNER = 'PYE'ORDER BY A.TABLE_NAME, B.COLUMN_ID;
```

<a name="abcdb7f1"></a>
#### ****修改不符合的时间，修改年份和月份**
-

```
-更新有/的时间、有两个/的日期、月份为1位数的，改为两位数select  (substr(t.stime,1,5)||'0'||substr(t.stime,6,length(t.stime))),substr(t.stime,1,5)||'0'||substr(t.stime,6,length(t.stime)),(length(substr(t.stime,0,7))-length(replace(substr(t.stime,0,7),'/',''))),t.stime,t.*,t.rowid From t_test_cc_all_b20181212 t --where substr(t.stime,length(t.stime),length(t.stime)-1)='-'WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'/','')))>=2 and  (length(substr(t.stime,0,7))-length(replace(substr(t.stime,0,7),'/','')))=2

update  t_test_cc_all_b20181212 t set t.stime=(substr(t.stime,1,5)||'0'||substr(t.stime,6,length(t.stime))) --where (length(t.stime)-length(replace(t.stime,'-',''))) =1WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'/','')))>=2 and  (length(substr(t.stime,0,7))-length(replace(substr(t.stime,0,7),'/','')))=2

--更新有/的时间、有两个/的日期、年份为1位数的，改为两位数select (substr(t.stime,1,length(t.stime)-1)||'0'|| substr(t.stime,length(t.stime),1)), t.stime,t.*,t.rowid From t_test_cc_all_b20181212 t --where substr(t.stime,length(t.stime),length(t.stime)-1)='-'WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'/','')))>=2 and   (length(substr(t.stime,length(t.stime)-1,2))-length(replace(substr(t.stime,length(t.stime)-1,2),'/','')))=1

update  t_test_cc_all_b20181212 t set t.stime=(substr(t.stime,1,length(t.stime)-1)||'0'|| substr(t.stime,length(t.stime),1)) --where (length(t.stime)-length(replace(t.stime,'-',''))) =1WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'/','')))>=2 and   (length(substr(t.stime,length(t.stime)-1,2))-length(replace(substr(t.stime,length(t.stime)-1,2),'/','')))=1

 

--更新有/的时间、有两个-的日期、月份为1位数的，改为两位数select  (substr(t.stime,1,5)||'0'||substr(t.stime,6,length(t.stime))),t.stime,t.*,t.rowid From t_test_cc_all_b20181212 t --where substr(t.stime,length(t.stime),length(t.stime)-1)='-'WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'-','')))>=2 and  (length(substr(t.stime,0,7))-length(replace(substr(t.stime,0,7),'-','')))=2

update  t_test_cc_all_b20181212 t set t.stime=(substr(t.stime,1,5)||'0'||substr(t.stime,6,length(t.stime))) --where (length(t.stime)-length(replace(t.stime,'-',''))) =1WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'-','')))>=2 and  (length(substr(t.stime,0,7))-length(replace(substr(t.stime,0,7),'-','')))=2

--更新有-的时间、有两个-的日期、年份为1位数的，改为两位数select (substr(t.stime,1,length(t.stime)-1)||'0'|| substr(t.stime,length(t.stime),1)), t.stime,t.*,t.rowid From t_test_cc_all_b20181212 t --where substr(t.stime,length(t.stime),length(t.stime)-1)='-'WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'-','')))>=2 and   (length(substr(t.stime,length(t.stime)-1,2))-length(replace(substr(t.stime,length(t.stime)-1,2),'-','')))=1

update  t_test_cc_all_b20181212 t set t.stime=(substr(t.stime,1,length(t.stime)-1)||'0'|| substr(t.stime,length(t.stime),1)) --where (length(t.stime)-length(replace(t.stime,'-',''))) =1WHERE length(t.stime)<10 and  (length(t.stime)-length(replace(t.stime,'-','')))>=2 and   (length(substr(t.stime,length(t.stime)-1,2))-length(replace(substr(t.stime,length(t.stime)-1,2),'-','')))=1

 

 
```

<a name="37a3a350"></a>
#### 查看所有表空间及容量

```
SELECT DBF.TABLESPACE_NAME,
       DBF.TOTALSPACE "总量(M)",
       DBF.TOTALBLOCKS AS 总块数,
       DBF.TOTALSPACE-DFS.FREESPACE "使用量(M)",
       DBF.TOTALBLOCKS-DFS.FREEBLOCKS AS 使用块数,      
       DFS.FREESPACE "剩余总量(M)",
       DFS.FREEBLOCKS "剩余块数",
       (DFS.FREESPACE / DBF.TOTALSPACE) * 100 "空闲比例"
  FROM (SELECT T.TABLESPACE_NAME,
               SUM(T.BYTES) / 1024 / 1024 TOTALSPACE,
               SUM(T.BLOCKS) TOTALBLOCKS
          FROM DBA_DATA_FILES T
         GROUP BY T.TABLESPACE_NAME) DBF,
       (SELECT TT.TABLESPACE_NAME,
               SUM(TT.BYTES) / 1024 / 1024 FREESPACE,
               SUM(TT.BLOCKS) FREEBLOCKS
          FROM DBA_FREE_SPACE TT
         GROUP BY TT.TABLESPACE_NAME) DFS
 WHERE TRIM(DBF.TABLESPACE_NAME) = TRIM(DFS.TABLESPACE_NAME);
```

<a name="c60f6138"></a>
#### oracle表空间不足时处理
表空间数据文件最大是32G，也就是说扩容最大为32G 
<a name="e7a0542d"></a>
##### 参考链接： 
[oracle 11g 导入数据库，表空间超过32G的解决办法——CSDN@冷静cc](https://blog.csdn.net/love_java_cc/article/details/52857363)  <br />[oracle 表空间不足解决办法大全——百度经验@javababy5](https://jingyan.baidu.com/article/48b37f8d6ca1eb1a646488dc.html)

<a name="b7cc6308"></a>
##### 第一，可能表空间还未达到最大扩容内存，但未设置自动扩容

```
--修改数据文件内存50m为当前数据文件的内存大小
alter database datafile 'D:\ORACLE\PRODUCT\ORADATA\TEST\USERS01.DBF' resize 50m;
--增加数据文件自动扩容功能,每次扩容为50m，最大不会超过32G
alterdatabase datafile 'D:\ORACLE\PRODUCT\ORADATA\TEST\USERS01.DBF' autoextend onnext 50m maxsize 32767m; 
```
<a name="e3470cfa"></a>
##### 第二，表空间数据文件已经达到32G，则可以通过增加数据文件方式

```
--USERS是你的表空间名，H:\IDE\oracle\oradata\orcl\USERS02.dbf可以改为你的任意地址，最好放在一起方便，
--每次扩容50m，最大32G
alter tablespace USERS  
add datafile 'H:\IDE\oracle\oradata\orcl\USERS02.dbf' size 50m 
autoextend on next 50m maxsize 32767m;
```


<a name="5dc99f6e"></a>
## ****问题****
<a name="122f6443"></a>
### ****oracle之违反唯一约束条件****
出现这个原因，除了自己手动新增ID的情况外，还有就是引用自己创建的sequance，导入新表数据后，没有将新的sequance导入进来，可以将新sequance导入进来，也可以自动手动修改<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118277133-fbbfbc99-569b-4a1e-ad5a-cf655ef6f786.png#align=left&display=inline&height=199&name=image.png&originHeight=199&originWidth=822&size=21888&status=done&width=822) <br /> 
<a name="e2d3e6c0"></a>
### ****修改字符集****
<a name="ea6f3b87-4"></a>
#### ****参考链接****
[如何改oracle AL16UTF16为AL32UTF8——百度知道](https://link.jianshu.com/?t=https://zhidao.baidu.com/question/134444813.html)<br />[建库时AL16UTF16字符集怎么设置？——出处: ITPUB论坛－中国最专业的IT技术社区](https://link.jianshu.com/?t=http://www.itpub.net/thread-505857-1-1.html%23pid3728655)<br />****操作：****

```
Microsoft Windows [版本 6.1.7601]

版权所有 (c) 2009 Microsoft Corporation。保留所有权利。

 

C:\Users\Administrator>sqlplus / as sysdba

 

SQL*Plus: Release 11.2.0.1.0 Production on 星期四 1月 11 12:00:49 2018

 

Copyright (c) 1982, 2010, Oracle.  All rights reserved.

 

 

连接到:

Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production

With the Partitioning, OLAP, Data Mining and Real Application Testing options

 

SQL> shutdown immediate

数据库已经关闭。

已经卸载数据库。

ORACLE 例程已经关闭。

SQL> startup mount

ORACLE 例程已经启动。

 

Total System Global Area 3290345472 bytes

Fixed Size                  2180224 bytes

Variable Size            2164263808 bytes

Database Buffers         1107296256 bytes

Redo Buffers               16605184 bytes

数据库装载完毕。

SQL> ALTER SYSTEM ENABLE RESTRICTED SESSION;

 

系统已更改。

 

SQL> ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;

 

系统已更改。

 

SQL> ALTER SYSTEM SET AQ_TM_PROCESSES=0;

 

系统已更改。

 

SQL> ALTER DATABASE OPEN;

 

数据库已更改。

 

SQL> ALTER DATABASE CHARACTER SET AL32UTF8;

ALTER DATABASE CHARACTER SET AL32UTF8

*

第 1 行出现错误:

ORA-12712: 新字符集必须为旧字符集的超集

 

 

SQL> ALTER DATABASE CHARACTER SET AL16UTF16;

ALTER DATABASE CHARACTER SET AL16UTF16

*

第 1 行出现错误:

ORA-12712: 新字符集必须为旧字符集的超集# ALTER DATABASE national CHARACTER SET AL16UTF16;

 

SQL> ALTER DATABASE character set INTERNAL_USE AL32UTF8;

 

数据库已更改。

 

SQL> SHUTDOWN IMMEDIATE;

数据库已经关闭。

已经卸载数据库。

ORACLE 例程已经关闭。

SQL> STARTUP

ORACLE 例程已经启动。

 

Total System Global Area 3290345472 bytes

Fixed Size                  2180224 bytes

Variable Size            2164263808 bytes

Database Buffers         1107296256 bytes

Redo Buffers               16605184 bytes

数据库装载完毕。

数据库已经打开。

SQL>
```

ALTER DATABASE character set INTERNAL_USE AL32UTF8;<br />INTERNAL_USE有点像是强制修改，其他用户角色可能会报错
<a name="bf52b411"></a>
#### ****其他问题****
<a name="3e2e31b2"></a>
##### ****AL16UTF16不能作为character set****
AL16UTF16 不能用做数据库的character set，只能用做national character set 。<br />character set必须是single byte 7-bit ASCII或是单字节EBCDIC的子集，因此fixed width的多字节字符集(AL16UTF16)就不能做为character set。<br /> <br />你可以用如下这样用的：

```
CHARACTER SET US7ASCII NATIONAL CHARACTER SET AL16UTF16

或是

CHARACTER SET zhs16cgb231280  NATIONAL CHARACTER SET AL16UTF16

 

 

 

 
```

<a name="46b308ed"></a>
### ****如何修改服务名service_name****
<a name="bd1bf7e7"></a>
##### ****转载链接****
[如何修改 service_name](https://link.jianshu.com/?t=https://www.2cto.com/kf/201311/259856.html)
<a name="d48898d5"></a>
##### ****例：****
<a name="64162c1b"></a>
###### ****service_name原有环境：****

```
sid： mynewdb

global_name：mynewdb

service_names： MYNEWDB

db_domain  ：

db_name：mynewdb
```

<a name="f0740534"></a>
###### ****需要修改如下：****

```
global_name：mynewdb

service_names： test

db_domain  ：

db_name：mynewdb
```

<a name="568a025c"></a>
##### ****方法：****
服务器端：<br />alter system set service_names='test';<br />这里采用静态注册，同时还要修改下 listener.ora

```
SID_LIST_LISTENER =

  (SID_LIST =

    (SID_DESC =

      (SID_NAME = PLSExtProc)

      (ORACLE_HOME =/u01/app/oracle/product/11.2.0/dbhome_1)

      (PROGRAM = extproc)

    )

        (SID_DESC=

        (GLOBAL_DBNAME = mynewdb)

        (ORALCE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)

        (SID_NAME = mynewdb)

        )

        (SID_DESC=

        (GLOBAL_DBNAME = test)  -------这个是需要添加

        (ORALCE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)

        (SID_NAME = mynewdb)    ------这个还是原来的实例名

        )

  )
```

cmd下执行命令lsnrctl reload<br />查看监听状态lsnrctl status<br />L

```
SNRCTL>

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=10.80.11.202)(PORT=1521)))

STATUS of the LISTENER

------------------------

Alias                     LISTENER

Version                   TNSLSNR for [Linux](https://www.2cto.com/os/linux/): Version 11.2.0.1.0 - Production

Start Date                21-NOV-2013 00:09:35

Uptime                    0 days 20 hr. 30 min. 55 sec

Trace Level               off

Security                  ON: Local OS Authentication

SNMP                      OFF

Listener Parameter File   /u01/app/oracle/product/11.2.0/dbhome_1/network/admin/listener.ora

Listener Log File         /u01/app/oracle/diag/tnslsnr/oracle11g/listener/alert/log.xml

Listening Endpoints Summary...

  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=10.80.11.202)(PORT=1521)))

Services Summary...

Service "PLSExtProc" has 1 instance(s).

  Instance "PLSExtProc", status UNKNOWN, has 1 handler(s) for this service...

Service "mynewdb" has 1 instance(s).

  Instance "mynewdb", status UNKNOWN, has 1 handler(s) for this service...

Service "test" has 1 instance(s).

  Instance "mynewdb", status UNKNOWN, has 1 handler(s) for this service...

The command completed successfully
```

可以看到新的 Service "test" 已经可以使用了<br />客户端配置：<br />net manager 配置 服务名为 test ，ip为数据库服务器主机ip，相应端口。<br />测试连接：

```
SQL>  conn sys/oracle@test as sysdba

已连接。

SQL>
```

当然不使用静态注册，动态注册也可以<br /> 
<a name="3158f60d"></a>
### ****警告日志文件****
不知道日志文件在哪的，可以使用这个命令<br />select value from v$diag_info where name='Diag Trace';<br />以下是我的输出地址

```
SQL> select value from v$diag_info where name='Diag Trace';

 

VALUE

--------------------------------------------------------------------------------

D:\FLYINGCLOUD\diag\rdbms\odb\odb\trace

 

 
```

<a name="9e2e81b7"></a>
## ****开发工具配置及问题****
<a name="Plsql"></a>
### ****Plsql****
<a name="d672874c"></a>
#### ****plsql设置可显示的查询记录条数****
tools->prifereces->window types->sql window->records per page<br />查询所有记录<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118425257-e4d6330a-e5f6-4e7e-932c-fb25cf372fd2.png#align=left&display=inline&height=591&name=image.png&originHeight=591&originWidth=564&size=92527&status=done&width=564) 
<a name="5c094ab1"></a>
#### ****plsql如何查询sql执行计划****
[怎么使用plsql查看执行计划](https://link.jianshu.com/?t=https://jingyan.baidu.com/article/ab69b270bffc2e2ca7189fee.html)<br />执行计划可以用计划sql执行的性能<br />选中需要执行的sql语句，然后按F5，或者直接点击"执行计划"
<a name="e33957e7"></a>
#### ****PLSQL工具如何远程连接数据库****
<a name="ea6f3b87-5"></a>
##### ****参考链接****
[如何配置pl/sql 连接远程oracle服务器——百度知道](https://link.jianshu.com/?t=https://zhidao.baidu.com/question/333852172.html)
<a name="d7ecf9b4"></a>
##### ****方法1：****
找到oracle的安装目录。如：C:\oracle\product\10.2.0\db_1\network\ADMIN<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118449158-f157887f-799b-4b90-a947-2218b90f25ac.png#align=left&display=inline&height=30&name=image.png&originHeight=30&originWidth=311&size=2681&status=done&width=311) <br />添加如下内容<br />其中中文部分是需要修改的部分，除第一个“本地实例名”外，其他需要跟远程数据库管理员咨询，本地实例名就是方便自己识别数据库的一个名字，可以自定义。

```
本地实例名 =

  (DESCRIPTION =

    (ADDRESS = (PROTOCOL = TCP)(HOST = 远程数据库IP地址)(PORT = 远程服务器端口号))

    (CONNECT_DATA =

      (SERVER = DEDICATED)

      (SERVICE_NAME = 远程数据库服务名)

    )

  )
```

然后打开pl/sql就能看到自己创建的链接，如图：<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118496776-446fbbd8-7f17-4ec9-be0b-219d248b2910.png#align=left&display=inline&height=204&name=image.png&originHeight=204&originWidth=392&size=17502&status=done&width=392) <br />方法2：<br /> <br /> 
<a name="5d64018c"></a>
##### ****方法2：****
<a name="ef35ed63"></a>
###### ****格式：****
ip:端口/sid
<a name="614fc656"></a>
###### ****示例：****
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1550118525263-c7c6ad21-d3fb-4563-9154-f8bc23c3f375.png#align=left&display=inline&height=209&name=image.png&originHeight=209&originWidth=393&size=10049&status=done&width=393) 

