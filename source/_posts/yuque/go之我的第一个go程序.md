
---

title: go之我的第一个go程序

urlname: pnyr7b

date: 2019-10-18 22:04:26 +0800

tags: []

---
<a name="xubnw"></a>
# go之我的第一个go程序
<a name="nuArP"></a>
## 参考链接
[Getting Started——Go](https://golang.google.cn/doc/install?download=go1.13.3.linux-amd64.tar.gz#install)
<a name="phTK8"></a>
## 下载go
我使用的是deepin linux，所以本文是在linux下编写的，首先在go官网下载压缩包，并解压到“/usr/local”路径下，解压后是路径是“/usr/local/go”<br />

```bash
tar -C /usr/local -xzf go1.13.3.linux-amd64.tar.gz
```

<a name="XOdUG"></a>
## 环境变量
使用以下命令行修改profile文件，在profile文件中插入“export PATH=$PATH:/usr/local/go/bin”<br />
```bash
vi /etc/profile
```
插入请输入i，移动光标，到指定位置输入“export PATH=$PATH:/usr/local/go/bin”，按Esc，并输入“:wq”保存并退出即可。

```bash
export PATH=$PATH:/usr/local/go/bin
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1571409177442-c508f544-0c88-4b1a-afd8-d90e71a77a65.png#align=left&display=inline&height=332&name=image.png&originHeight=332&originWidth=745&size=51136&status=done&width=745)
<a name="GuXuN"></a>
## 刷新环境变量

```bash
source /etc/proflie
```

<a name="r0GYO"></a>
## demo
在任意一个文件夹中创建文件名为hello.go，并输入以下文件内容<br />

```go
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}
```
在当前路径下打开终端，输入以下代码编译go程序,＂$HOME/go/src/hello＂是我存放文件的路径。编译完成后会发现多了一个hello的可执行文件。<br />

```bash
cd $HOME/go/src/hello
go build
```
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1571409673843-af2fafea-36a7-4b4f-adef-9a0d1ea2fe7e.png#align=left&display=inline&height=126&name=image.png&originHeight=126&originWidth=667&size=13603&status=done&width=667)<br />“./<可执行程序>”,表示运行该程序，输入以下命令行运行go程序。

```bash
./hello
```
hello, world

