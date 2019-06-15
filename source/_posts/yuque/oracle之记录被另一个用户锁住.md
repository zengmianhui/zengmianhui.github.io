
---

title: oracle之记录被另一个用户锁住

date: 2019-03-25 11:58:05 +0800

tags: oracle,数据库

categories: oracle

---


<a name="ea6f3b87"></a>
# 参考链接
[oracle "记录被另一个用户锁定"——博客园@流年煮雪](https://www.cnblogs.com/angusbao/p/7521430.html)

<!--more-->
<a name="ef2bb941"></a>
# oracle之记录被另一个用户锁住

```powershell
SQL> select object_id,session_id,locked_mode from v$locked_object;

 OBJECT_ID SESSION_ID LOCKED_MODE

---------- ---------- -----------

    74605         11           3

SQL> select t2.username,t2.sid,t2.serial#,t2.logon_time from v$locked_object t1,

v$session t2 where t1.session_id=t2.sid order by t2.logon_time;

USERNAME                        SID        SERIAL#    LOGON_TIME

------------------------------ ---------- ---------- --------------

FSET                                11       2430     25-3月 -19

SQL>  alter system kill session '11,2430' ;

系统已更改。

SQL> select object_id,session_id,locked_mode from v$locked_object;

未选定行
```


