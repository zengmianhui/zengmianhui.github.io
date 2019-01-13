---
title: oracle之partition by与group by的区别
date: 2018-01-10 22:58:39
categories:
  - oralce
  - sql
tags:
  - oralce
  - sql
---
# 正文
## 前言
> 先来看两个表

* 主表(webcheck——存储网站记录)

|  webcheckid   | website       | 
| :-------------:|:-------------:| 
| 1 | www.baidu.com   | 
| 2 | www.google.com |
| 3 | www.soso.com |

* 副表(webrecord——访问网站记录表)

| webrecordid   |webcheckid | access_state  | access_date|
| :-------------:|:-------------:|:-------------:| :-------------:| 
| 1         | 1  |  成功  | 2018-01-10 18:03:49  |
| 2         | 1| 成功  | 2018-01-09 18:03:49  |
| 3         | 2| 失败 | 2018-01-11 18:03:49  |
> 我想要关联副表，**查询每个网站最新访问记录**，即使**网站没有访问记录，也要能查询出来**
> 
<!--more-->

* 预计实现效果为

| webcheckid|website|webrecordid| access_state| access_date|
| :-----:|:----------:|:---------:| :---------:|  :---------:| 
| 1     | www.baidu.com | 1  |  成功  | 2018-01-10 18:03:49  |
| 2     | www.google.com | 3| 失败  | 2018-01-11 18:03:49  |
| 3     | www.soso.com | |  |  |
```
--以下是错误的sql，唯一难住我的是就是关联查询最新记录了，之前想当然地用group by来分组获取最新记录，但是会报错，group by的字段必须与查询的字段相对应，除非是聚合函数包裹着
select a.WEBCHECKID,a.WEBSITE,b.RECORDID,b.access_state,b.access_date from TFSWXTS_WEBCHECK a left join TFSWXTS_WEBRECORD b on a.WEBCHECKID=b.WEBCHECKID 
group by b.WEBCHECKID,b.access_date having b.access_state=max(b.access_state) 
--报错不是group by 的表达式      
```
> 改为使用以下语句
```
row_number() over(partition by b.webcheckid order by b.access_date desc) 

--partition by 按字段分组
--order by 分组，按组内排序，desc即最新在第一条
--row_number() 按分组内的行个数，生成行号；1,2,3，……
```
> 推出新的sql

```
--行号作为新字段,并用一个新select包裹，获取第一行数据，即为新记录
select * from ( 
         select a.WEBCHECKID,a.WEBSITE,b.RECORDID,b.access_state,b.access_date,  
         
         row_number() over(partition by b.webcheckid order by b.access_date desc) as ran
          from TFSWXTS_WEBCHECK a left join TFSWXTS_WEBRECORD b on a.WEBCHECKID=b.WEBCHECKID 
        ) where recordid is  null or ran=1
```

> -- `recordid is  null `是为了防止当前webrecord表里面没有数据，行号数据为混乱，行号此时就无法作为判断条件

* 例如当soso和google在webrecord没有访问记录时，错误实现效果为，soso这一数据没有显示出来

| webcheckid|website|webrecordid| access_state| access_date|ran|
| :---:|:---:|:----:| :----:|  :------:| :-------:| 
| 1     | www.baidu.com | 1  |  成功  | 2018-01-10 18:03:49  | 1  |
| 2     | www.google.com | |   |  |  1 |

* 去外部条件，我们来看看效果，可以看到soso的行号为2，明显不正常，故需要增加 `recordid is  null `，并需要`or`条件

| webcheckid|website|webrecordid| access_state| access_date|ran|
| :---:|:---:|:----:| :----:|  :------:| :-------:| 
| 1     | www.baidu.com | 1  |  成功  | 2018-01-10 18:03:49  | 1  |
| 1     | www.baidu.com | 1  |  成功  | 2018-01-09 18:03:49  | 2  |
| 2     | www.google.com | |   |  |  1 |
| 1     | www.soso.com |   |    |   | 2  |


# 成功实现效果为

| webcheckid|website|webrecordid| access_state| access_date|ran|
| :---:|:---:|:----:| :----:|  :------:| :-------:| 
| 1     | www.baidu.com | 1  |  成功  | 2018-01-10 18:03:49  | 1  |
| 2     | www.google.com | 3|  失败 | 2018-01-11 18:03:49 |  1 |
| 3 | www.soso.com |   |    |   | 2 |
