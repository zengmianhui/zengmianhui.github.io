
---
title: 语雀之语雀+serverless+travis CI+hexo+github搭建云写作博客
date: 2019-01-27 14:02:59 +0800
tags: [语雀,travis,serverless]
categories: 
	- 其他
	- 语雀
---
# 参考链接
[Hexo 博客终极玩法：云端写作，自动部署——segmentFault@](https://segmentfault.com/a/1190000017797561)[Nero](https://segmentfault.com/u/nerohua)  <br />[如何在github上创建个人博客——图片链接失效了，我后面会再更新](https://iszengmh.github.io/2017/01/27/%E5%A6%82%E4%BD%95%E5%9C%A8github%E4%B8%8A%E5%88%9B%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%9F/)  <br />[手把手教你使用Travis CI自动部署你的Hexo博客到Github上——简书@SmileUsers](https://www.jianshu.com/p/e22c13d85659)  

# 正文
## 注意：语雀触发Webhook调用serverless，我配置失败了
1. 语雀触发Webhook调用serverless，我也不明白为什么失败，语雀更新了文档没有自动触发webhook，所以如果想要完整实现的朋友不用继续往下看，可以到参考链接上看，大神可能已经实现，可以到评论区咨询他。
1. 由于我是完成之后再可能有部分示例不完整

## hexo+github
这是我以之前有写此类教程，这里就不再赘述了，网上教程也很多
## travis CI自动构建部署github博客
> 请看下面这张图，引用[Hexo 博客终极玩法：云端写作，自动部署——segmentFault@](https://segmentfault.com/a/1190000017797561)[Nero](https://segmentfault.com/u/nerohua)  

![](https://cdn.nlark.com/yuque/0/2019/png/244275/1548570430080-765faf89-daf4-47e3-9390-2d5248c8f03c.png#align=left&display=inline&height=262&linkTarget=_blank&originHeight=281&originWidth=800&size=0&width=746)<br />travis的配置其实很简单，虽然我之前一直失败，原因都是语法或者是命令错误
### 首先需要配置一个新仓库，或者一个新分支
我这里选择创建一个新分支**blog**blog**master**travis.yml**blog**blog**blog**master**hexo+github**自动部署
1. **master**分支为输出目录，可以查看我仓库[iszengmh.github.io](https://github.com/iszengmh/iszengmh.github.io)这个仓库![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548571962065-2887ec31-fe23-4b1d-922e-11b0be8d1dff.png#align=left&display=inline&height=594&linkTarget=_blank&name=image.png&originHeight=594&originWidth=930&size=75111&width=930)
1. **blog**分支为输出目录，可以查看我仓库[iszengmh.github.io](https://github.com/iszengmh/iszengmh.github.io)这个仓库

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548572128842-d7f15a4f-ab97-451e-ace8-066621ca9ce6.png#align=left&display=inline&height=557&linkTarget=_blank&name=image.png&originHeight=557&originWidth=933&size=82045&width=933)
### 在源码分目录下创建.travis.yml， travis后台会跟踪blog分支，并读取.travis.yml中的命令执行
至于如何将blog的代码更换为源码，其实只需将blog分支克隆下来，删除原文件，再将源码提交，并push到远程分支即可，**需要先在源码分目录下创建.travis.yml**blog**.travis.yml**中的命令执行

```yaml
 #设置语言
language: node_js  
# 指定需要sudo权限
sudo: required
#设置相应的版本
node_js: 
  - 10.15.0
# 指定缓存模块，可选。缓存可加快编译速度。
cache:
    directories:
        - node_modules    
before_install:
  - npm install -g hexo-cli
#安装hexo及插件
install:
  - npm install   
  - npm install hexo-deployer-git --save
  - npm i -g yuque-hexo
# yuque-hexo clean 清除语雀文章,并清除“yuque-hexo”的json文件“yuque.json”
# yuque-hexo sync 同步语雀的文章，并创建json文件“yuque.json”
# hexo clean 清理文章
# hexo generate 重新发布文章
script:
  - yuque-hexo clean
  - yuque-hexo sync
  - hexo clean
  - hexo generate

# iszengmh修改成自己的github用户名
# iszengmh@qq.com修改成自己的GitHub邮箱
# GH_token就是在travis中设置的token，等下会告诉大家怎么配置
# GH_REF 是下面仓库地址
after_script:
  - cd ./public
  - git init
  - git config user.name "iszengmh"   
  - git config user.email "iszengmh@qq.com"   
  - git add .
  - git commit -m "update by Travis-CI"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master 
 #只监测这个分支，一有动静就开始构建
branches:
  only:
  - blog 
# GH_REF 仓库地址
env:
    global:
        - GH_REF: github.com/iszengmh/iszengmh.github.io.git
```
### 修改_config.yml中的发布设置
修改仓库地址，增加gh_token
```yaml

#发布设置
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  ## 修改仓库地址，增加gh_token
  repo: https://gh_token@github.com/iszengmh/iszengmh.github.io.git
  branch: master
  message: 我的博客
#头像
avatar: /blog_head.jpg
```

* push到远程origin的blog分支，一定要注意push远程分支blog，我之前一直提交错误

```bash
git push origin blog
```

### 配置travis后台同步,跟踪
[travis网站](https://travis-ci.org/)
* 去github官网先配置一个token，并复制token，用于其他用户作提交

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548574333248-0cc50f8e-5045-4139-8eaf-3d086ed930cf.png#align=left&display=inline&height=378&linkTarget=_blank&name=image.png&originHeight=378&originWidth=1022&size=41927&width=1022)
* 勾选同步，并进入setting 

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548574028361-9bea90c6-b1e0-4dbd-8e2a-ce197f6c70c7.png#align=left&display=inline&height=577&linkTarget=_blank&name=image.png&originHeight=577&originWidth=1180&size=66727&width=1180)
* 配置变更GH_TOKEN，value那里请输入github的token

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548574251748-8dfe579d-601a-434f-a138-0f154b6cb74c.png#align=left&display=inline&height=231&linkTarget=_blank&name=image.png&originHeight=231&originWidth=1060&size=19010&width=1060)

### travis 配置完璧，接下来可以通过将源码提交到blog，或者到travis的后台点击“restart build”自动构建 
![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548574498837-6fd3d668-101f-468e-8723-db0edcbd33ba.png#align=left&display=inline&height=323&linkTarget=_blank&name=image.png&originHeight=323&originWidth=1026&size=32717&width=1026)
## 语雀配置Webhook和serverless（此步骤失败了，语雀没有触发webhook）
### 先需要腾讯云无服务云函数
腾讯云或者阿里云都可以，我是选择腾讯云，在控制台新建函数<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548580798367-54082aeb-2989-47c5-9583-e6cf107fcbdb.png#align=left&display=inline&height=488&linkTarget=_blank&name=image.png&originHeight=488&originWidth=681&size=27779&width=681)

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548580946060-3a407941-963e-4e78-b090-150a8959217e.png#align=left&display=inline&height=503&linkTarget=_blank&name=image.png&originHeight=503&originWidth=897&size=31517&width=897)
### 输入函数代码
此函数来自[Hexo 博客终极玩法：云端写作，自动部署——segmentFault@](https://segmentfault.com/a/1190000017797561)[Nero](https://segmentfault.com/u/nerohua)  

```php
<?php
function main_handler($event, $context) {
    // 解析语雀post的数据
    $update_title = '';
    if($event->body){
        $yuque_data= json_decode($event->body);
        $update_title .= $yuque_data->data->title;
    }
    // default params
    $repos = 'xxxx';  // 你的仓库id 或 slug
    $token = 'xxxxxx'; // 你的登录token
    $message = date("Y/m/d").':yuque update:'.$update_title;
    $branch = 'master';
    // post params
    $queryString = $event->queryString;
    $q_token = $queryString->token ? $queryString->token : $token;
    $q_repos = $queryString->repos ? $queryString->repos : $repos;
    $q_message = $queryString->message ? $queryString->message : $message;
    $q_branch = $queryString->branch ? $queryString->branch : 'master';
    echo($q_token);
    echo('===');
    echo ($q_repos);
    echo ('===');
    echo ($q_message);
    echo ('===');
    echo ($q_branch);
    echo ('===');
    //request travis ci
    $res_info = triggerTravisCI($q_repos, $q_token, $q_message, $q_branch);

    $res_code = 0;
    $res_message = '未知';
    if($res_info['http_code']){
        $res_code = $res_info['http_code'];
        switch($res_info['http_code']){
            case 200:
            case 202:
                $res_message = 'success';
            break;
            default:
                $res_message = 'faild';
            break;
        }
    }
    $res = array(
        'status'=>$res_code,
        'message'=>$res_message
    );
    return $res;
}

/*
* @description  travis api , trigger a build
* @param $repos string 仓库ID、slug
* @param $token string 登录验证token
* @param $message string 触发信息
* @param $branch string 分支
* @return $info array 回包信息
*/
function triggerTravisCI ($repos, $token, $message='yuque update', $branch='master') {
    //初始化
    $curl = curl_init();
    //设置抓取的url
    curl_setopt($curl, CURLOPT_URL, 'https://api.travis-ci.org/repo/'.$repos.'/requests');
    //设置获取的信息以文件流的形式返回，而不是直接输出。
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    //设置post方式提交
    curl_setopt($curl, CURLOPT_CUSTOMREQUEST, "POST");
    //设置post数据
    $post_data = json_encode(array(
        "request"=> array(
            "message"=>$message,
            "branch"=>$branch
        )
    ));
    $header = array(
      'Content-Type: application/json',
      'Travis-API-Version: 3',
      'Authorization:token '.$token,
      'Content-Length:' . strlen($post_data)
    );
    curl_setopt($curl, CURLOPT_HTTPHEADER, $header);
    curl_setopt($curl, CURLOPT_POSTFIELDS, $post_data);
    //执行命令
    $data = curl_exec($curl);
    $info = curl_getinfo($curl);
    //关闭URL请求
    curl_close($curl);
    return $info;
}
?>
```
### 查看repo的id和token
* travis登录token，在travis-ci.org 中设置界面获取：

![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548581610483-75a884f9-2810-4176-b739-655c803c4121.png#align=left&display=inline&height=409&linkTarget=_blank&name=image.png&originHeight=409&originWidth=1236&size=46342&width=1236)
* 获取travis的仓库ID

在后台进入对应在仓库，travis会请求仓库地址，所以直接找到相似链接，链接路径名就仓库ID，路径像这个https://api.travis-ci.org/repo/<你的仓库ID><br />参考链接中说需要请求API，但是我操作了半天不知道为什么一直提示方法不可用，所以投机取巧找这个方法<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548585058267-ef4c79df-3e21-47df-b5d5-9c3166dd3774.png#align=left&display=inline&height=508&linkTarget=_blank&name=image.png&originHeight=508&originWidth=1146&size=179654&width=1146)

### 在语雀中新建知识库，然后设置开发者
最好先看一下[语雀开发文档](https://www.yuque.com/yuque/developer)<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/244275/1548581236420-c369b9fe-501f-4ede-bcc1-098d55efb2f0.png#align=left&display=inline&height=544&linkTarget=_blank&name=image.png&originHeight=544&originWidth=1148&size=49627&width=1148)

其中“所有更新触发”是指，有文档新增或者变更时，自动触发，而“仅主动推送更新触发”是指在勾选“文档有较大更新时，推送给关注的人”时，才会触发Webhook，具体请看文档[WebHook 说明](https://www.yuque.com/yuque/developer/doc-webhook#wooqpi)<br />无服务函数（请到对应云平台去复制，并加入参数）示例：

```http
https://service-<…………>.ap-guangzhou.apigateway.myqcloud.com/release/main_handler?repos=<your travis's repo ID>&token=<your travis token>&message=语雀自动提交&branch=blog
```


## yuque-hexo开源项目用于同步语雀
### 需要在.travis.yml中配置如下，如果已经copy上文的配置，则无需再配置

```
script:
  - yuque-hexo clean
  - yuque-hexo sync
  - hexo clean
  - hexo generate
```
### 配置package.json
以下注释，一定要删除，json文件不能使用注释，这里只作说明，[开源项目地址](https://github.com/x-cold/yuque-hexo)
> login————这里是你语雀的个人主页链接的路径名，例如[https://www.yuque.com/iszengmh/](https://www.yuque.com/iszengmh/p)
> repo————这里是你语雀的个人主页知识库的路径名，例如[https://www.yuque.com/iszengmh/personalblog](https://www.yuque.com/iszengmh/p)
> mdNameFormat 生成的 Markdown 文件的文件名，可以选择 "title" 或者 "slug"，默认 "title"，slug 是语雀的永久链接名，一般是几个随机字母。
> postPath 存放从语雀下载的 Markdown 文件的文件夹，除了 Hexo ，理论上可以支持其他支持 Front-matter 的 Markdown 静态博客


```json
  //………………………………
  },//注意补上逗号哦，不然后会报错
  "yuqueConfig": {
    "baseUrl": "https://www.yuque.com/api/v2",
    "login": "iszengmh",
    "repo": "personalblog",
    "mdNameFormat": "title",
    "postPath": "source/_posts/yuque"
  },
  "scripts": {
    "clean": "hexo clean",
    "clean:yuque": "DEBUG=yuque-hexo.* yuque-hexo clean",
    "deploy": "hexo deploy",
    "publish": "npm run clean && npm run deploy",
    "dev": "hexo s",
    "sync": "DEBUG=yuque-hexo.* yuque-hexo sync",
    "reset": "npm run clean:yuque && npm run sync"
  }
```

### 将修改的文件全部提交github的blog分支

```bash
git add .
git commmit -m "修改"
git push -u origin blog
```

由于travis自动构建会自动发布到github的master分支
## 手动触发自动构建
由于我的语雀配置没有自动触发travis更新，所以我自己去travis后台点击“restart build”可以同步并自动构建，这样也是可以的，本篇文章就是这样同步过去的
## 以上就是全部配置，如果你语雀可以触发webhook，请告诉怎么搞，搞死人

