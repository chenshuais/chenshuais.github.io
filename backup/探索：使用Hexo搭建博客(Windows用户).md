# 前言
我是在一个安卓开发群里了解到这种搭建博客的方法，群友发了两个链接，链接上的教程说是五分钟搭建自己的博客，可我经过了一个多小时的探索才完成了这个博客。自己又搜索些资料，改了改主题，才最终弄成现在这个样子。

先贴出群友发的两个链接：
> [http://www.jianshu.com/p/4eaddcbe4d12](http://www.jianshu.com/p/4eaddcbe4d12)
> 
> [http://opiece.me/2015/04/09/hexo-guide/](http://opiece.me/2015/04/09/hexo-guide/)

# 正文

## 搭建前的准备
* 要有一个github账号 ( 没有的可以先去注册一下 → [github](https://github.com/) )
* 安装 [Git](http://pan.baidu.com/s/1cwumrc) ( 下载完后一直下一步就可以，记下安装目录，方便后面使用 )
* 安装 [Nodejs](https://nodejs.org/en/) (下载完后也是一直下一步即可)
* 安装hexo
    
    这里就需要Git的安装目录了，在这个目录下找到一个叫Git Bash.vbs的文件，打开如下
    `Gmeek-html<img src="https://blog-image.ian2018.cn/images/d5ed40ad4ec71dbe8c9e9539d5518fe7e9894dec.png">`
    然后输入 ` npm install hexo-cli -g ` 按回车执行，完成

## 开始搭建

### 在github上创建仓库

进入github首页，在上面的标题栏上点一个加号形状的按钮，弹出三个选项，选第一个New repository

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/eafc43bc751ea1c766c9a518850b01c8ba2cb28f.png">`

然后出现下面这个界面

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/17006c7e3889b430f65fcea2b120cabdd6ffe391.png">`

仓库名格式必须是 ` xxx.github.io` ，这就是将来你访问你博客的域名，其中的xxx是自己github的用户名。

其他保持默认即可，然后点击创建，完成此步骤。

### 用Hexo初始化博客

还是找到Git的安装目录，在这个目录下找到 `Git Bash.vbs` 文件，打开

输入 `hexo init xxx.github.io`

执行成功后，会在Git的安装目录下创建出一个名为 `xxx.github.io` 的文件夹。

### 基础配置

找到刚刚创建的 `xxx.github.io` 的文件夹，打开名字叫 `_config.yml` 的文件，找到以下几个关键字，对照着进行修改

```yml
title:    xxx        //这里写你博客的名字
subtitle: xxx     //个性签名
description: xxx    //博客描述     
author: xxx    //你的名字(昵称)
language: zh-Hans        //语言 简体中文

theme: next       //主题的名称，后面我们会去配置

deploy:
    type: git    //使用Git 发布
    repo: https://github.com/你github的用户名/xxx.github.io.git    // 刚创建的Github仓库(可以到github上复制刚创建仓库的链接)
```

[查看Hexo更多详细配置信息](https://hexo.io/zh-cn/docs/configuration.html)

### 配置主题

其实Hexo提供了好多高逼格的主题，自己可以在[这里](https://hexo.io/themes/)挑选一个喜欢的

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/8dc97995ae5b360270d47344ebd0fa8ceee64cc4.png">`

下载完后解压，把文件夹名字改成你下载主题的名字 ( 全是小写字母 ) 例：again，然后复制到刚创建的文件夹 `xxx.github.io/themes` 目录下

首先到 `xxx.github.io` 文件夹下的 `_config.yml` 文件，现在可以设置上一步基础配置里的主题了

找到 theme 节点修改

```yml
theme: again    //主题的名字，就是我们拷贝到 xxx.github.io/themes  目录下那个文件夹的名字
```

接下来就是个性化主题了

来到 `xxx.github.io/themes/你主题的名字` 文件夹下 打开 `_config.yml` 修改相应节点。

一般主题的配置，在下载下的主题文件夹中会有一个叫 `README.md` 的文件，这里会有相应配置信息。

### 写文章

所有基础框架都已经创建完成，接下来可以开始写你的第一篇文章了

在 `xxx.github.io/source/_posts` 文件夹下

新建一个名为 `xxx.md` 的文件，用 [Markdown](http://pan.baidu.com/s/1eRTLOe2) 书写你的大作吧。

## 测试和发布

### 测试

继续打开 Git 安装目录下的 `Git Bash.vbs` 文件，输入

```bash
cd xxx.github.io 

hexo s
```

等待一会，出现以下内容

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/852d85027f5c41ecac54c9cd6af3eaa5409e04a0.png">`

就可以在浏览器中输入 `https://localhost:4000` 访问了 ( 其中 localhost 可以替换成本机 IP )。

### 发布博客

打开 Git 安装目录下的 `Git Bash.vbs` 文件，输入

```bash
cd xxx.github.io

hexo g && hexo d
```

等待一会，就会出现一堆东西

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/84f42884bf535d2367885fde4a7d8c505d671bcf.png">`

然后会让你输入 github 的 用户名 和 密码

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/a11c8b467ad8f69ced2184b004ddda36f1c6e95e.png">`

最后等待完成就可以了

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/242b121663fd705b49c133c06b67ca1e4c947dc2.png">`

### 去欣赏一下自己搭建的博客

打开浏览器，输入 `xxx.github.io` ,就可以访问到自己的博客啦

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/7642ec1030f823568406524b5674a9686164f607.png">`

## 结言

这是我结合群友发的两篇教程和自己的一点探索完成的，在内容和格式上参考了一些两位前辈的 [Chillax](http://opiece.me/) 和 [简书作者：dimsky](http://www.jianshu.com/users/965ae4feb718/latest_articles)

---

花了两个多小时，终于完成了这篇文章……



<!-- ##{"timestamp": 1472727372}## -->