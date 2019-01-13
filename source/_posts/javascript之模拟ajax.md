---
title: javascript之模拟ajax
date: 2017-06-10 23:09:22
categories:
  - javascript
  - ajax
tags:
  - javascript ajax
---

# javascript之模拟ajax
## 原javascript的异步请求方法
很多人可能一直用jquery封装好的ajax异步请求，可能不知道原生javascript是怎么异步请求的，javascript是用`XMLHttpRequest`来创建异步对象的，以下是简单的请求方法。
```
    var ajax = new XMLHttpRequest();
    ajax.open('post','http://www.baidu.com');
    //因为是post请求所以必须加上setRequestHeader(get请求可以忽略)
    ajax.setRequestHeader('Content-type','text/plain;charset=UTF-8');
    //请求所带的参数
    ajax.send("username=123456");
    //绑定onreadystatechange事件
    ajax.onreadystatechange=function(){
    //判断如果请求的状态是否成功
    	if (ajax.readyState==4&&ajax.status==200) {
    		    console.log("请求的结果是"+ajax.responseText);
		}
    };
```
<!--more-->
## 简单封装，便于理解
当然是我没有看到jquery是如何写的ajax，只是在自己理解范围写了这个ajax。

> ajax.js

```
//requestParameter为一个JSON对象
var ajax=function(requestParameter){
	var requestObj = new XMLHttpRequest();
	var requestMethod=requestParameter.type.toLowerCase();
		switch(requestMethod){
			case "get":
				break;
			case "post":
				break;
			case "options":
				break;
			case "head":
				break;
			case "put":
				break;
			case "detele":
				break;
			case "trace":
				break;
			default:
				throw("ajax:type don't null");
				break;

		}
	//	请求地址及请求方法设置
	if (requestParameter.url) {
		requestObj.open(requestMethod,requestParameter.url);	
	}else{
		throw("ajax:url don't null");
	}
	//请求头
	if (requestParameter.head) {
		requestObj.setRequestHeader('Content-type',requestParameter.head);
	}
	//请求参数，data为一个JSON对象并发送
	if (requestParameter.data) {
		var dataParameter="";
		for(var o in requestParameter.data){
			dataParameter+=o+"="+requestParameter.data[o]+"&"
		}
		dataParameter=dataParameter.substring(0,dataParameter.length-1)
		requestObj.send(dataParameter);
	}
	//结果返回
	requestObj.onreadystatechange=function(){
		var result={
			status:requestObj.status,
			result:requestObj.responseText
		};
		if (requestObj.readyState==4&&requestObj.status==200&&requestParameter.success) {
			 requestParameter.success(result);
		}else if (requestObj.readyState==4&&requestParameter.error) {
			requestParameter.error(result);
		}
		
	}
	
	return requestObj;
	
};
```
## 简单调用
```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>测试</title>	
	<script src="ajax.js" type="text/javascript"></script>
</head>
<body>
    <!-- login('abc','123456') -->
    <input type="submit" value="登录" onclick="testLogin()"/><br>
</body>
<script type="text/javascript">
//登录
function testLogin(username,pass){
    ajax({
    	url:"http://www.server.com",
    	type:"post",
    	data:{
    		username:username,
    		pass:pass
    	},
    	success:function(obj){
    		console.log("index: 成功了，状态代码是"+obj.status+"返回结果"+obj.result);
    	},
    	error:function(obj){
    		console.log("index: 失败了，状态代码是"+obj.status+"返回结果"+obj.result);
    	}
    });
}
</script>
</html>
```

> 如果有觉得写不好，请在下方评论一起讨论