---
title: javascript之模块化
date: 2017-05-10 11:54:22
categories:
  - javascript
  - 模块化
tags:
  - javascript模块化
---
# 参考链接
[《Javascript模块化编程（一）：模块的写法》—— 阮一峰](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)
# javascript之模块化
## 说明
&emsp;&emsp;所谓的模块化，是换功能去划分模块，现在的网页越来越像桌面应用程序，随着团队协作、进度管理、单元测试等等开发模式，网页开发变得越来越复杂，引入的javascript文件越来越多。  
&emsp;&emsp;模块化是软件工程的方法，开发者只需开发自己的核心模块，其他模块只需通过引入的方式即可（jquery、vue.js也是module）
<!-- more -->
## 原始模块写法

```
var v1;
function f1(){
    
}
function f2(){
    
}
```
&emsp;&emsp;上面的简单放在一起就是一个module了，需要调用直接f1()和f2()调用就可以了。  
&emsp;&emsp;但是这样的写法会污染全局变量，而且
变量名也有可能与其他模块冲突，而且模块之间看不出直接的关系。

## 对象写法
```
var module=new Object{
    _count:0,
    f1:function(){
        
    },
    f2:function(){
        
    }
}
```
&emsp;&emsp;像上面的写法，只需module.f1()即可调用函数，但是缺点是可以直接访问，对象内的私有变量，内部状态可以被外部直接编写。
```
module._count=5;
```

##  立即执行函数写法(javascript模块化的基本写法)
> 这样的写法，外部无法直接获取内部变量

```
var module=(function(){
    var _count=0;
    var f1=function(){
        return _count;
    };
    var f2=function(){
        _count++;
    };
    return {f1:f1,f2:f2};
})();
//外部无法直接获取内部变量
　console.info(module1._count); //undefined
```
## 放大模式(augmentation)
如果一个模块很大，需要分成几个小模块，或者需要一个模块继承另一个模块
```
var module=(function(mod){
    mod.m3=function(){
        
    }
    
})(module);
```
module新增一个m3，并返回给新的module。
## 宽放大模式（Loose augmentation）
&emsp;&emsp;在开发中，大部分模块可能是直接网上获取的，所以我们无法保证哪一个预先加载完，也就是有可能会加载到一个module空对象，所以需要使用宽放大模式。
```
var module=(function(mod){
    mod.m3=function(){
        
    }
    
})(window.module||{});
```
&emsp;&emsp;与"放大模式"相比，＂宽放大模式＂就是"立即执行函数"的参数可以是空对象。

## 模块内输入全局变量
&emsp;&emsp;模块内部最好不要直接引用全局变量
```
　var module1 = (function ($, YAHOO) {
　　　　//...
　})(jQuery, YAHOO);
```
&emsp;&emsp;上面的module1模块需要使用jQuery库和YUI库，就把这两个库（其实是两个模块）当作参数输入module1。这样做除了保证模块的独立性，还使得模块之间的依赖关系变得明显。