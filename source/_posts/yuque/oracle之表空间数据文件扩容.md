
---

title: oracle之表空间数据文件扩容

date: 2019-05-24 16:37:39 +0800

tags: oracle,表空间

categories: oracle

---



<a name="xLiK6"></a>
# 参考链接
<a name="8DpMF"></a>
## 查询表空间使用情况

```sql

----查询表空间使用情况---     
    
SELECT UPPER(F.TABLESPACE_NAME) "表空间名",     
D.TOT_GROOTTE_MB "表空间大小(M)",     
D.TOT_GROOTTE_MB - F.TOTAL_BYTES "已使用空间(M)",     
TO_CHAR(ROUND((D.TOT_GROOTTE_MB - F.TOTAL_BYTES) / D.TOT_GROOTTE_MB * 100,2),'990.99') "使用比",     
F.TOTAL_BYTES "空闲空间(M)",     
F.MAX_BYTES "最大块(M)"    
FROM (SELECT TABLESPACE_NAME,     
ROUND(SUM(BYTES) / (1024 * 1024), 2) TOTAL_BYTES,     
ROUND(MAX(BYTES) / (1024 * 1024), 2) MAX_BYTES     
FROM SYS.DBA_FREE_SPACE     
GROUP BY TABLESPACE_NAME) F,     
(SELECT DD.TABLESPACE_NAME,     
ROUND(SUM(DD.BYTES) / (1024 * 1024), 2) TOT_GROOTTE_MB     
FROM SYS.DBA_DATA_FILES DD     
GROUP BY DD.TABLESPACE_NAME) D     
WHERE D.TABLESPACE_NAME = F.TABLESPACE_NAME     
ORDER BY 4 DESC;  

```
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1558687529242-c9957964-97f0-4623-a24f-36fbb5504edb.png#align=left&display=inline&height=191&name=image.png&originHeight=191&originWidth=614&size=13664&status=done&width=614)

```sql

--查看表空间是否具有自动扩展的能力     
SELECT T.TABLESPACE_NAME,D.FILE_NAME,     
D.AUTOEXTENSIBLE,D.BYTES,D.MAXBYTES,D.STATUS     
FROM DBA_TABLESPACES T,DBA_DATA_FILES D     
WHERE T.TABLESPACE_NAME =D.TABLESPACE_NAME     
 ORDER BY TABLESPACE_NAME,FILE_NAME; 
```
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1558687557539-d579743f-cc38-4f5c-927b-b24816a50bef.png#align=left&display=inline&height=213&name=image.png&originHeight=213&originWidth=866&size=24491&status=done&width=866)

<a name="woEPM"></a>
## 表空间扩容
通常默认的表空间都一个数据文件
<a name="xQFMD"></a>
### 给表空间增加数据文件
```sql

ALTER TABLESPACE app_data ADD DATAFILE  
'D:\ORACLE\PRODUCT\10.2.0\ORADATA\EDWTEST\APP03.DBF' SIZE 50M;  
```
<a name="0s5nB"></a>
### 新增数据文件，并且允许数据文件自动增长

```sql
ALTER TABLESPACE app_data ADD DATAFILE
'D:\ORACLE\PRODUCT\10.2.0\ORADATA\EDWTEST\APP04.DBF' SIZE 50M
AUTOEXTEND ON NEXT 5M MAXSIZE 31G;
```
<a name="Bbhkr"></a>
### 允许已存在的数据文件自动增长
```sql

ALTER DATABASE DATAFILE 'D:\ORACLE\PRODUCT\10.2.0\ORADATA\EDWTEST\APP03.DBF'  
AUTOEXTEND ON NEXT 5M MAXSIZE 100M;
```

<a name="Oe2uv"></a>
### 手工改变已存在数据文件的大小

```sql
ALTER DATABASE DATAFILE 'D:\ORACLE\PRODUCT\10.2.0\ORADATA\EDWTEST\APP02.DBF'  
RESIZE 100M;
```

<a name="HuT2f"></a>
# oracle之表空间数据文件扩容

