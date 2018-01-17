---
title: Github for first-time users
date: 2015-06-15 23:44:43
tags:
  - Tool
  - Tutorial
categories:
  - Tool
---

本文是篇干货，是一个基于linux 命令行操作的github快速使用指南。
<!--more-->

GitHub是一个基于Git版本控制工具的免费远程仓库。你可以将个人的开源项目，放到GitHub上分享给世界各地的程序员。同时，GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

在GitHub出现以前，让广大码农群众参与一个开源项目比较困难。当时参与群众也仅限于报个bug，即使能改掉bug，也只能把diff文件用邮件发过去，很不方便。但是在GitHub横空出世之后，利用Git极其强大的Clone克隆和Fork分支功能，广大码农群众真正可以第一次自由参与各种开源项目了。开源运动进入了一个新的高潮。因此，Github被戏称为“世界上最大的同性交友网站”.

喵~~你懂的。Bazinga.

## 浏览器端

请首先登录到 github.com，点击Create Repository按钮。

![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150708-4.jpg)

在添加repo名字之后，可点击创建按钮，进入下一步。同时，我建议对于正式一点的项目，添加一个ReadMe，这是对于你代码用户和读者的尊重；添加一个License，可以基本保护你的代码不被商业滥用，保障了自己的合法权益。多种License的试用范围有所区别，你可以自行查询，建议一般使用MIT License，比较试用。至于是否添加.gitignore，就看你是否需要，这个文件的作用是过滤出一些你不想commit的文件类型，比如你make之后的.c~， README~文件。

在下一步中，Github会给出如下所示操作代码提示。

```
touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/zxdong262/paper-slider.git
git push -u origin master
```

linux 命令行

```
//建立目录
mkdir paper-slider
cd paper-slider
//初始化 git
git init
//设置 git 用户名 邮箱地址（通常应该跟github注册邮箱一致）
//全局设置
git config --global user.name "yourname"
git config --global user.email "your-email-address"
//也可以给当前repo 单独设置
git config user.name "yourname"
git config user.email "your-email-address"
```

然后使用gedit, bluefish 或者 emacs 等任意工具在 paper-slider 目录创建 readme.md, markdown 基本语法请自行Google或者百度一下，然后哇塞，你就都知道了。

```
# paper-slider
a "paper slider" plugin for zepto and jQuery.
## demo
todo
## license
whatever
```
## 提交文件入库的步骤
简要来讲，就是首先通过Add,然后commit，最后再push，就可以讲一个文件，或者多个文件PUSH入仓库。下面以一个readme为例，

```
git add readme.md
git commit -m 'init'
git remote add origin https://github.com/zxdong262/paper-slider.git
git push -u origin master
//然后输入用户名密码
```

如果要需要给自己的Project写一个更加详细的说明，可以使用gihub-pages,那就

```
git push origin master:gh-pages
//然后输入用户名密码
```

这样就创建了gh-pages分支，之后就可以通过 http://zxdong262.github.io/paper-slider/ 访问你的文件，这样github就可以作为网站甚至是cdn.
到此就可以认为初始化完成，以后可以继续提交文件

```
git add file1-to-commit
git add file2-to-commit
git add file3-to-commit
git commit -m 'commit note'
git push origin master
git push origin master:gh-pages
//然后输入用户名密码
```
