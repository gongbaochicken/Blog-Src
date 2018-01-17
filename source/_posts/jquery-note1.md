---
title: jQuery Note 1
date: 2017-03-01 22:58:20
tags:
  - Front end
  - Note
categories:
  - Web
---

Disclaimer: This is my private note for learning jQuery, and they are useful so I prefer to write them down. Most of them are what I encountered during building some projects. I am a new learner about jQuery, so maybe this is useless to you, my dear readers.
<!--more-->

首先一个根本问题就是：jQuery操作的到底是个啥？DOM： Document Object Model.

## 引入jQuery：
可以通过CDN,或是源文件来引入。
在HTML中可以像这样插入。

```html
<script>
	$(document).ready(function(){
		...
	});
</script>

```
## 创建

```js
//创建element
var div = document.createElement("div");
document.body.appendChild(div);

//创建text
var div = document.createElement("div");
var txt = document.createTextNode("DOM");
div.appendChild(txt);
document.body.appendChild(div);

//setAttribute
div.setAttribute("title","盒子");
var $div = $("<div title="盒子">DOM</div>");
$(body).append($div);
```

## 选择器：
以下是常用选择器：

```js
$("\*"): 所有元素
$("#lastname"): id="lastname" 的元素
$(".intro"): 所有 class="intro" 的元素
$("p"): 所有<p>元素
$(".intro.demo"): 所有 class="intro"且 class="demo" 的元素
```

以下是位置选择器：

```js
:first -----对应方法------> .first()
:last  ----对应方法------> .last()
:not(?) ----对应方法------> .not(?)
:eq(num) ----对应方法------>  .eq(num)
:gt(num) ----对应方法------>  .gt(num)
:lt(num) ----对应方法------>  .lt(num)
```

## 遍历操作

```js
.next() 每个选中元素紧邻的下一个元素，可传入selector进行筛选
.nextAll() 每个选中元素后的所有同辈元素，可传入selector进行筛选
.nextUntil([selector],[filter]) 每个选中元素之后、直至但不包含第一个和selector匹配的元素，可传入filter进行筛选
.prev() 每个选中元素紧邻的上一个元素，可传入selector进行筛选
.prevAll([selector]) 每个选中元素前的所有同辈元素，可传入selector进行筛选
.prevUntil([selector],[filter]) 每个选中元素之前、直至但不包含第一个和selector匹配的元素，可传入filter进行筛选
```

## Methods
有一些常用的方法，比如添加/删减class, 以此来改变前端属性。

```js
.addClass();
.removeClass();
```

show vs hide

```js
//callback 是动画执行的方法
show(speed, callback);

//原理将高度，宽度，不透明度改为0.对应show的话，就是从上到下增加高度，从左到右增加宽度，从0-1增加不透明度。
hide()
```

滑动动画效果
slideUp(200), slideDown(100)

## 浏览器事件
```js
.resize()   //当浏览器window的尺寸改变时，window元素上绑定的resize事件将被触发

$(window).resize(function() {
    $('body').prepend('<div>' + $(window).width() + '</div>');
});


.scroll()  //当用户在元素内执行了滚动操作，就会在这个元素上触发scroll事件。

$(window).scroll(function () {
    $("span").css("display", "inline").fadeOut("slow");
});
```

## 事件绑定
```js
.ready(handler) //DOM和CSS完全加载后之间handler
.on(type or events,[selector],[data],handler)   //根据type/events对象的事件绑定多个事件处理程序
.off(type,[selector],[data],handler)  //解除on给元素绑定的事件处理程序
.bind(type,[data],handler)   //绑定type事件，并指定事件处理程序handler
```

## 事件触发：
```js
mouseenter,mouseleave, click
```
