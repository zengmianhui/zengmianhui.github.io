
---

title: spring boot之配置数据源时提示无法加载驱动类

urlname: ug2d5n

date: 2019-03-06 22:55:02 +0800

tags: []

---
<a name="ea6f3b87"></a>
# 参考链接
[Difference between Oracle jdbc driver classes?](https://stackoverflow.com/questions/6202653/difference-between-oracle-jdbc-driver-classes/6202721#6202721)[——stackoverflow@bw_üezi](https://stackoverflow.com/questions/6202653/difference-between-oracle-jdbc-driver-classes/6202721#6202721)  

<!--more-->
<a name="af3c8dff"></a>
# spring boot之配置数据源时提示spring boot Cannot load driver class: oracle.jdbc.driver.OracleDriver

1. 环境的maven是自定义了本地仓库位置，但是idea配置maven的设置时却引用的C:\Users\Administrator\.m2\repository，如果自定义了，iDea中也一定要配置好。
1. Idea设置问题，由于我环境的maven是自定义了本地仓库位置，但是idea配置maven的设置时却引用的C:\Users\Administrator\.m2\repository，如果自定义了，iDea中也一定要配置好。

<!--more-->

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1551884347572-64098d3c-b391-4934-9ee4-793fb17d85a6.png#align=left&display=inline&height=649&name=image.png&originHeight=649&originWidth=818&size=43119&status=done&width=818)

3. oracle9i开始就不允许使用__oracle.jdbc.driver.OracleDriver__驱动名,而需要使用****oracle.jdbc.OracleDriver****

 

4. 没有初始化相应的bean，或者没有使用注解@Bean使用于相关数源方法，以下我是之前错误的代码

```java
@ConfigurationProperties("oracle.datasource")
public DataSourceProperties oracleDataSourceProperties(){
   return new DataSourceProperties();
}
@Bean
public DataSource oracleDataSource() {
   DataSourceProperties dataSourceProperties =oracleDataSourceProperties();
   System.out.println("oracle.datasource: {}"+dataSourceProperties.getUrl());
   return dataSourceProperties.initializeDataSourceBuilder().build();
}
@Bean
@Resource
public PlatformTransactionManager oracleTxManager(DataSource oracleDataSource) {
   return new DataSourceTransactionManager(oracleDataSource);
}
```

以上如果仔细看，可以看到oracleDataSourceProperties()这个方法没有使用注解@Bean，导致其一个bean在初始时找不到,我在打印输出时提示“oracle.datasource: {}null”

