---
title: How to build blog with Hexo on Windows
date: 2017-12-29T23:29:00+08:00
lastmod: 2017-12-29T23:29:00+08:00

tags: 
    - 教程

# url: "post/build-blog-with-hexo.html"
---

## 第一次搭Blog的经历

16年暑假，用Wordpress和阿里云学生机搭了一个Blog，配环境、弄数据库、修改主题搞了半天，总算是弄好了。
结果后面数据库不知道因为啥原因出了点问题，也懒得修，就直接弃了。

下图为原Blog的遗照(它现在还苟延残喘在我本地的WampServer上)

<!--more-->

![Wordpress](http://p1u1vh70v.bkt.clouddn.com/wordpress.png)

## 开始接触Hexo

后来在学校Vidar Team的群中看到了学长学姐们的一些Blog，知道了还有`Hexo`这种东西。

然后查了查百度，还知道了`Hugo`、`Jekyll`等Blog生成工具。

但最后还是选择了第一眼见到的`Hexo`

`Hexo`的好处是不需要服务器，你可以直接将网页部署在你的Github上，缺点的话就是它只能生成静态的网页了

## 搭建博客前的本地工作

### 安装Git

[官网下载安装Git](https://git-scm.com/downloads) **记得选择适合自己系统的版本下载喔OVO**

### 安装Node.js

[Node.js](https://nodejs.org/en/)是一个JavaScript 运行环境，用来生成静态页面的。

笔者这里选了`8.9.3 LTS`的版本下载

安装时记得勾选`ADD TO PATH`选项

### （题外话）editor的选择

都<del>8012年</del>了,你还在用记事本写代码吗？

一个好的editor可是能大大提高自己的生产<del>摸鱼</del>效率啊。

比如：`Sublime Text3`、`Visual Studio Code`、`Notepad++`、`Atom`等。

笔者目前使用的是`Visual Studio Code`

### 安装并部署Hexo

`win+R`运行cmd，使用npm安装Hexo
```Bash
$ npm install -g hexo-cli
```

确认安装成功后
```Bash
$ cd <博客文件夹存放的目录>
$ hexo init <博客文件夹名>
$ cd <博客文件夹名>
$ npm install
```

以笔者的操作为例就是
```Bash
$ cd C:\\Users\\a9657\\Documents
$ hexo init Blog
$ cd Blog
$ npm install
```

然后，Hexo会自动为你生成你网站所需要的全部文件。此时，我们已经在本地搭建好Blog了。

继续在Blog目录下执行如下命令，可生成静态页面
```Bash
$ hexo generate (可简写为hexo g)
```

启动本地服务，可进行预览调试，命令：
```Bash
$ hexo server (可简写为hexo s)
```

然后在浏览器输入 http://localhost:4000 可以预览当前博客

如果无法访问，表示4000端口被占用，命令：
```Bash
$ hexo s -p 5000
```

这里我们切换成5000端口，浏览器输入 http://localhost:5000 访问即可。

## 将博客部署至Github上

### 配置Github

在Github上简历与你用户名对应的Repository，名字必须为`Your_User_Name.github.io`，比如我的用户名是Ericwww，那我的仓库名字就是`Ericwww.github.io` 【但我不确定是否会区分大小写】

然后我们来修改`_config.yml`文件来建立关联

打开`_config.yml`后，拖到最下面，改成像这样字的，注意：**`: 后面要有空格`**
```
deploy:
  type: git
  repository: https://github.com/Ericwww/Ericwww.github.io.git (仓库地址填你自己的)
  branch: master
```

修改完后，在命令行中输入：

```Bash
$ hexo deploy (可简写为hexo d)
```

然后你就可以通过`用户名.github.io`来访问你的博客啦！

### 美化博客

Hexo有大量的主题可以让您挑选，如：

> [Cover](https://github.com/daisygao/hexo-themes-cover)
> [TKL](https://github.com/SuperKieran/TKL)
> [Tinnypp](https://github.com/levonlin/Tinnypp)
> [Writing](https://github.com/yunlzheng/hexo-themes-writing)
> [NexT](https://github.com/iissnan/hexo-theme-next)
> [Yeele](https://github.com/MOxFIVE/hexo-theme-yelee)

笔者的博客使用的是[Yilia](https://github.com/litten/hexo-theme-yilia)

