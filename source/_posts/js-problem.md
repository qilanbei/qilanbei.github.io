---
title: javascript踩过的坑
date: 2016-11-26 23:24:34
description: javascript踩过的坑
categories:
- JavaScript 小知识
tags: 
- javascript踩过的坑
toc: true 文章目录
---
+ javascript踩过的坑
<!-- more -->
<The rest of contents | 余下全文>

### javaScript踩过的坑
1. click事件累加绑定
情况一：(未复现)
在页面中为一个元素绑定事件，事件执行后页面未刷新且将元素隐藏，但是元素绑定的事件未解绑，然后你再次点击，元素又被绑定个一次点击事件，这样第二次点击就会执行两次;
情况二：
```
$("#cover").mouseover(function (){
	$('#btn').click(function (){
		alert('测试累加绑定次数')
	}
}
// js中嵌套的click事件，点击多次会出现累加绑定，调用多次alert；
```
情况三：
jquery嵌套event也会触发多次，在一次写的代码的时候就遇见过，代码如下：
```
$("#btn_submit").on("click", function (){
	// 模态框显示
	$('模态框').modal("show");
	// 模态框里的按钮
	$('#btn').on("click", function (){
		if(输入验证正确){
			ajax操作
		}
	})
})
```
这时，click事件会出现累加绑定的现象，当输入不正确的时候，点击btn按钮，验证不通过不会触发ajax操作，但是click时间会累加绑定事件，当输入正确，点击btn按钮的时候，会触发两次绑定的事件，查看发现调用了两次ajax操作。

##### 解决方法：
1> 情况一可以将元素彻底删除，重新生成，在重新绑定事件，这样每次绑定的事件都是重新绑定的，不会出现累加的情况，或者使用以下方法；
2> 如果是使用bind绑定的事件，可以用unbind方法解除绑定，现在bind方法已经不推荐使用了；
```
$('#btn').unbind("click").bind("click", function (){})
```
3> 如果使用了jquery的on方法绑定事件，可以有两种方式处理：
3.1> 使用one()方法为元素绑定一个一次性的事件处理函数，这个事件只会执行一次；
```
$('#btn').one("click", function (){})
```
3.2> 使用off()方法解除绑定；
```
$('#btn').off("click").on("click", function (){})
```
