---
title: 正则表达式之环视(lookaround)
date: 2017-04-21 15:06:48
categories:
  - regex
tags:
  - regex
---
# 参考链接
[正则基础之——环视(Lookaround)](http://www.cnblogs.com/kernel0815/p/3375249.html)  

<!--more-->

# 环视(lookaround)
## 说明
环视是由《精通正则表达式》命名的，其他资料也有不同的叫法。  
环视，大体分为顺序和逆序环视，以下是一个表格，表示了环视四种组合。

环视四种表示|  哪一侧的表达式| 是否匹配| 环视分类
---|---|---|---
?<=|左侧 | 是|逆序
?<!|左侧 | 否|逆序
?=|右侧 | 是|顺序
?!|右侧| 否|顺序

## 示例
```
//?=右侧匹配
String str="abbb";
Pattern p = Pattern.compile("a(?=bbb)");//a后面紧跟bbb
Matcher matcher = p.matcher(str);
System.out.println(matcher.find());//返回true
//?!右侧不匹配
Pattern p1 = Pattern.compile("a(?!bbb)");//a后面不紧跟bbb
Matcher matcher1 = p1.matcher(str);
System.out.println(matcher1.find());//返回false
```