---
title: Jason聊Bootstrap：第一话
date: 2015-07-11 14:38:16
tags:
  - Front end
  - Tutorial
categories:
  - Web

---
Bootstrap是Twitter的一个孩子。按照它官方的说法，Bootstrap是一个简洁、直观、强悍的前端开发框架，让web开发更迅速、简单。我觉得Bootstrap流行最好的体现就是，很多网站，尤其是硅谷那些Startup公司的网站，大多采用了Bootstrap的框架进行前端的快速开发，使其看起来简约但不简单。尽管很多优秀的前端设计师已经开始公开抱怨，Bootstrap会溟灭一个公司网站的创意和设计感，使整个IT行业网站千篇一律，但是我觉得Bootstrap是一个非常值得初学者学习的前端综合框架。

![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150727-2.jpg)

目前，Bootstrap已经出到了3.3版本了，想初步了解更多信息，有个更加直观的认识，在国内可以访问其中文的官方地址为：http://v3.bootcss.com。

本文是Jason聊Bootstrap的第一话，旨在告诉大家Bootstrap最基本用法和一些常见的组件的使用。
<!--more-->

###下载和引用
在下载部分，可以根据自己的需要下载三种版本的Bootstrap：

* 用于生产环境的 Bootstrap，编译并压缩后的 CSS、JavaScript 和字体文件。不包含文档和源码文件。
* Bootstrap 源码，Less、JavaScript 和 字体文件的源码，并且带有文档。需要 Less 编译器和一些设置工作。使用起来比上一个稍微麻烦一些。
* Sass，这是 Bootstrap 从 Less 到 Sass 的源码移植项目，用于快速地在 Rails、Compass 或 只针对 Sass 的项目中引入。

如果你下载的是第一个版本，解开压缩包之后，文件大小有1.1MB左右，目录结构就是这样的：

{% codeblock %}
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   └── bootstrap-theme.min.css
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2

{% endcodeblock %}

凡是带min的都是压缩版本。

同时，Bootstrap官方基于国内云厂商的CDN，提供了一个免费的CDN加速服务，使得在国内加速效果更加明显。

```html
	<!-- 新 Bootstrap 核心 CSS 文件 -->
    <link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.5/css/bootstrap.min.css">

	<!-- 可选的Bootstrap主题文件（一般不用引入） -->
	<link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.5/css/bootstrap-theme.min.css">

	<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
	<script src="//cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>

	<!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
	<script src="//cdn.bootcss.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
```

当然，你要是个Git捍卫者和爱好者，你也可以去Github上面去Clone源码下来。

注意，Bootstrap的JS效果是基于jQuery，所以引用的时候应当先引入jQuery，再引入Bootstrap。

##第一个Bootstrap Demo：基本导航栏

```html
	<!DOCTYPE html>
	<html>
	<head lang="en">
    	<meta charset="UTF-8">
    	<meta name="viewpoint" content="width=device-width, initial-scale=1">
    	<title>Bootstrap Demo 01</title>
    	<link rel="stylesheet" href="bootstrap.min.css">
	</head>
	<body>
    	<nav class="navbar navbar-default" role="navigation">
       		<div class="container">
           	<div class="navbar-header">
              		<a href="#" class="navbar-brand"> Jason </a>
            	</div>
        	</div>
    	</nav>
	</body>
	</html>
```

通过meta标签定义一下屏幕宽度，设初始比例为1，即自适应时不放缩。通过link引入一个css样式。通过nav标签来看看一个默认default的标签样式，加一个标志一样的东西，这里随便写一个Jason吧。通过查看效果，如图：
![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150709-1.jpg)
很简约的森女风格有木有！当然，这里可以修改navbar的样式为别的，比如修改navbar-default为navbar-inverse,这时就会将导航的灰色背景改为黑色背景，极客风格有木有！

我们接下来继续添加导航栏的代码，我们添加三个具体项目到导航栏，分别是Home,Story, AboutMe，并对Home栏设为默认选中Active。因为导航栏的每一项都是可以选中的，所以我们一般都添加一个a标签进行连接。为了简化，我就只展示body里面的部分了，HTML代码和效果如下：

```html
	<body>
    	<nav class="navbar navbar-inverse" role="navigation">
       		<div class="container">
           	<div class="navbar-header">
               	<a href="#" class="navbar-brand"> Jason </a>
            	</div>
            	<div class="collapse navbar-collapse">
               	<ul class="nav navbar-nav">
                  	<li class="active"><a href="#">Home</a></li>
                 		<li><a href="#">Story</a></li>
                 		<li><a href="#">AboutMe</a></li>
                	</ul>
            	</div>
        	</div>
    	</nav>
	</body>
```
![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150709-3.jpg)

最后，我们可以继续完善，使其更具一般形态。通过一个container，添加一个正文的文本,并调整合适的padding。HMTL和代码效果如下：
![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150709-4.jpg)


##Bootstrap:简单的表单

单独的表单控件会被自动赋予一些全局样式。所有设置了.form-control 类的 input、textarea 和 select 元素都将被默认设置宽度属性为 width: 100%;。 将 label 元素和前面提到的控件包裹在 .form-group 中可以获得最好的排列。

基本样式示例：
![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150724-1.jpg)

```html
	<!DOCTYPE html>
	<html>
	<head lang="en">
    	<meta charset="UTF-8">
    	<title></title>
    	<link rel="stylesheet" href="bootstrap.min.css">
	</head>
	<body>
    	<form class="form-horizontal" role="form">
       		<div class="form-group">
           	<label class="col-md-2 control-label" >User Name</label>
          		<div class="col-md-4">
             		<input type="text" class="form-control" placeholder="username">
            	</div>
        	</div>
        	<div class="form-group">
          		<label class="col-md-2 control-label">Email</label>
            	<div class="col-md-4">
             		<input type="email" class="form-control" placeholder="Email Address">
            	</div>
        	</div>
        	<div class="form-group">
          		<label class="col-md-2 control-label">Password</label>
         		<div class="col-md-4">
             		<input type="password" class="form-control" placeholder="password">
            	</div>
        	</div>
        	<div class="form-group">
          		<div class="col-md-offset-2 col-md-2">
             		<div class="checkbox">
            			<label>
               		<input type="checkbox">是否记住密码
                		</label>
                	</div>
            	</div>
        	</div>
        	<div class="form-group">
          		<div class="col-md-offset-2 col-md-2">
             		<label class="control-label">
                 		<button class="btn btn-default">登陆</button>
                	</label>
            	</div>
        	</div>
    	</form>
	</body>
	</html>
```

##BootStrap巨幕
巨幕在Bootstrap设计的网站中是非常常见的。它是一个轻量、灵活的组件，它能延伸至整个浏览器视口来展示网站上的关键内容，带给人很强烈的视觉第一印象。相关的巨幕可以借鉴Airbnb,Bootstrap官网等网站的巨幕设计。

示例：
![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150725-1.jpg)

```html
	<div class="jumbotron">
        <div class="container">
            <h1>Welcome</h1>
            <p>San Francisco, City by the Bay</p>
            <p><a class="btn btn-success" href="#" role="button">Learn More</a></p>
        </div>
    </div>
 ```

##BootStrap徽章
徽章给链接、导航等元素嵌套 <span class="badge"> 元素，可以很醒目的展示新的或未读的信息条目，就像Facebook，微信等应用中的那些未读消息提醒。

三种示例：
![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150725-2.jpg)

```html
    <div class="container">
        <a href="#">Messages <span class="badge">8</span></a>
    </div>
    <br>
    <div class="container">
        <button class="btn btn-success" type="button">Messages <span class="badge">99</span></button>
    </div>
    <br>
    <ul class="nav nav-pills">
        <li role="presentation" class="active"><a href="#">Home <span class="badge">10</span></a></li>
        <li role="presentation"><a href="#">Profile <span class="badge">99</span></a></li>
        <li role="presentation"><a href="#">Contact <span class="badge">25</span></a></li>
    </ul>
```
