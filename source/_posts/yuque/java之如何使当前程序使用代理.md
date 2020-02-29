
---

title: java之如何使当前程序使用代理

urlname: xg2mli

date: 2019-05-05 12:13:10 +0800

tags: []

---
<a name="VKpaC"></a>
# 参考链接

[java中设置网络代理——简书@jijs](https://www.jianshu.com/p/9e1abe05314d) 

<a name="9rrtR"></a>
# java之如何使当前程序使用代理
<a name="jldiB"></a>
## 概述
<a name="cSPtQ"></a>
## java中支持的代理
java中支持 HTTP代理、HTTPS代理、Socket代理、FTP代理 。

<!--more-->
<a name="emjLU"></a>
## 程序中编写程序使用代理

```java
//使用http协议
System.setProperty("http.proxyHost", "192.168.10.130");
System.setProperty("http.proxyPort", "808");
System.setProperty("http.nonProxyHosts", "192.168.3.249 | 192.168.3.100");
//https代理
System.setProperty("https.proxyHost", "192.168.10.130");
System.setProperty("https.proxyPort", "808");
System.setProperty("https.nonProxyHosts", "192.168.3.249 | 192.168.3.100");
//ftp代理
System.setProperty("ftp.proxyHost", "192.168.10.130");
System.setProperty("ftp.proxyPort", "808");
System.setProperty("ftp.nonProxyHosts", "192.168.3.249 | 192.168.3.100");
//使用socks协议连接
Properties prop = System.getProperties();
prop.put("proxySet", true);
prop.setProperty("socksProxyHost", "143.168.16.188");
prop.setProperty("socksProxyPort", "1780");
//注意：socks无法使用忽略IP配置
//prop.setProperty("nonProxyHosts", "192.168.1.82");//192.168.1.82 | 192.168.3.100

```

<a name="lasfA"></a>
## jvm变量中设置代理

```powershell
# 在系统启动时，使用-D项来设置代理。
# http代理
java -Dhttp.ProxyHost=143.168.16.188 -Dhttp.ProxyPort=1780 com.test.mydemo
# https代理
java -Dhttps.ProxyHost=143.168.16.188 -Dhttps.ProxyPort=1780 com.test.mydemo
# ftp代理
java -Dftp.ProxyHost=143.168.16.188 -Dftp.ProxyPort=1780 com.test.mydemo
# socks代理
java -Dsocks.ProxyHost=143.168.16.188 -Dsocks.ProxyPort=1780 com.test.mydemo
```


