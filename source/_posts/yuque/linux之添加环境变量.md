
---

title: linux之添加环境变量

urlname: olpbvb

date: 2019-10-18 20:28:26 +0800

tags: []

---
<a name="KlTLs"></a>
# linux之添加环境变量
<a name="crpYg"></a>
## 参考链接
<a name="6gIdE"></a>
## 添加环境变量
所有环境变量都需要添加在profile这个文件中<br />

```bash
vi /etc/profile
```

在文件插入一行环境变量，例如下面这段环境变量，插入按esc，并输入:wq，即退出并保存 <br />

```bash
export PATH=$PATH:/usr/local/go/bin
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1571401897558-f8593b08-3299-45b4-af05-2bd9106469f2.png#align=left&display=inline&height=330&name=image.png&originHeight=330&originWidth=752&size=68702&status=done&width=752)

<a name="zat4N"></a>
## 刷新环境变量

```bash
source /etc/profile
```


