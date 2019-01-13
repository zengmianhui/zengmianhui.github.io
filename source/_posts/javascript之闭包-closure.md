---
title: javascript之闭包(closure)
date: 2017-05-05 23:22:24
categories:
  - javascript
tags:
  - javascript闭包
---


# 参考链接
[javascript深入理解js闭包](http://www.jb51.net/article/24101.htm)

# javascript之闭包(closure)
## 说明
&emsp;&emsp;闭包就是拥有许多变量和绑定这些变量环境的表达式，因此这些变量也是该表达式的一部分。  
&emsp;&emsp;这个解释估计会有许多人懵了不懂，我们暂且理解为，闭包是一个函数表达式，表达式绑定了一些变量的环境，然后看下面的代码。
```
function f1(){
    var n=999;
    function f2(){
        console.log(n);
    }
    return f2;//返回内部函数供外部调用。
}
var result=f1();//创建函数表达式，result引用了f2函数，绑定变量n的环境。
result();//输出999
```
<!--more-->
## 变量的作用域
&emsp;&emsp;javascript的变量分为两种，全局变量和局部变量，全局变量无法直接引用局部变量，局部变量可以直接引用全局变量，这就是作用域。闭包就是为了解决全局变量无法引用局部变量的问题。


```
/**
*直接引用全局变量
*/
var n=999;
function f1(){
　alert(n);//引用全局变量
}
f1(); // 999
/**
*无法直接引用局部变量
*/
function f2(){
　var abc=111;
}
　alert(abc); // error
```

> 闭包获取局部变量

```
function f1(){
    var n=999;
    function f2(){
        console.log(n);
    }
    return f2;//返回内部函数供外部调用。
}
var result=f1();
result();//输出999
```
## 闭包需要注意的问题
&emsp;&emsp;闭包会将局部变量长期驻留在内存中，很可能会造成内存泄漏。这和java的内存引用很相似，长生命周期的变量引用短生命周期的变量时，会促使垃圾回收器无法回收短生命周期的变量。
```
function f1(){
　var n=999;
　nAdd=function(){n+=1}//没有使用var修饰，实为申明全局变量。
　function f2(){
　　　alert(n);
　}
　return f2;
}
var result=f1();
result(); // 999
nAdd();
result(); // 1000，再次调用也可以获取到增后n的值，所以n没有被回收
```

## 我们可以这么玩
```
function f1(){
    var n=0;
    function f2(){
        n++;
        function f3(){
            n++;
            function f4(){
                n++;
                console.log(n);
            }
            return f4;
        }
        return f3;
    }
    return f2;
}
//第一种调用方式
f1()()()();// 输出 3
//第二种调用方式
var result=f1()()();
result();//输出 3
```

> 多连括号方式调用，即为将父函数返回的子函数立即调用。

```
(function(){
    var n=0;
    console.log(n);
})();//输出 0
//也可以带有函数名
(function f1(){
    var n=0;
    console.log(n);
})();//输出 0
```
> 这个叫做 申明函数并立即执行。