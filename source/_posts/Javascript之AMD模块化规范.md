---
title: Javascript之AMD模块化规范
date: 2017-05-10 11:54:39
categories:
  - javascript
  - 模块化
tags:
  - javascript模块化
---
# 参考链接
[《Javascript模块化编程（二）：AMD规范》——阮一峰](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)
# Javascript之AMD模块化规范
## 说明
&emsp;&emsp;现今通行的模块化规范有三种AMD、CMD和Commonjs（AMD和CMD是Commonjs衍生出来的模块化，Commonjs应该也是CMD）
<!-- more -->
## 服务器环境（CommonJs）
&emsp;&emsp;2009年，当nodejs被美国程序员Ryan Dah开发出来后，javascript也可以作为服务器端语言，开发服务器功能，但是为了解决服务器端的复杂性，开发Commonjs来使javascript模块化操作。

> 假定有一个数学模块math.js
```
 var math=require("math");
```

> 然后就可以调用它模块方法了

```
 var math=require("math");
 math.add(2,3);//得到5
```

## 浏览器环境（AMD）

```
 var math=require("math");
 math.add(2,3);//得到5
```
&emsp;&emsp;上面这段代码是Commonjs的模块化，但是很明显有一个缺点，可以看出需要先加载math这个模块，获取再调用add()，这样可能会浏览器出现卡死的情况，因为同步执行，js没有执行完无法继续加载页面，所以就使用到**异步加载模块**，AMD就是异步加载模块规范，待模块加载完成后，自动回调方法。`require([module], callback);`
```
 require(['math'],function(math){
     math.add(2,3);
 });
```
> 这样的话，就算模块未加载完成也不会造成页面假死

> 目前使用AMD规范是流行库，require.js和curl.js，异步模块化益于浏览器端
