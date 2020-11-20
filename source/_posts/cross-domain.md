---
title: 有关跨域知识总结
date: 2016-06-06 10:33:20
description: 有关跨域知识总结
categories:
- 跨域
tags: 
- 有关跨域知识总结
toc: true 文章目录
---
+ 有关跨域知识总结
<!-- more -->
<The rest of contents | 余下全文>


#### 有关跨域知识总结
什么是跨域，为什么会发生跨域
跨域是浏览器行为，是浏览器为安全性考虑实施的安全策略，是对js施加的安全限制，由于浏览器的同源策略，一般网站的协议protocol、主机host、端口号port这三个中的任意一个不同，网站间的数据请求与传输边构成了跨域调用。
由于ajax的实现原理是由XMLHttpRequest对象实现的，而浏览器对XMLHttpRequest对象有跨域限制，当通过ajax请求不同域下的资源的时候，请求会发送给服务器，只是服务器返回响应信息给浏览器时，被浏览器拦截了
#### 举例说明
举例之前，先以http://www.baidu.com为例，普及一些概念：
```angular2html
1> 在这里http就是一种协议，www.baidu.com是域名，浏览器默认http端口是80端口,https的端口号是443;
2> 在www.baidu.com中，.com是顶级域名；baidu.com是一级域名；www.baidu.com二级域名，类似的bbs.baidu.com和tieba.baidu.com都是二级域名。
3> 列举能发生跨域的url(也许地址不存在，仅是为了能够更加具体，举得例子)：
```
```angular2html
http://www.baidu.com和https://www.baidu.com：会发生跨域，由于协议不一样；
http://www.taobao.com和http:www.google.com：会发生跨域，域名不一样；
http://www.baidu.com/index.html和http://www.baidu.com:4000/index.html：会发生跨域，端口号不一样；
http://www.baidu.com/index.html和http://tieba.baidu.com/index.html：会发生跨域，域名不一样；
http://www.baidu.com/index.html和http://www.baidu.com/src/demo.html 不会发生跨域行为，只是所在的目录不一样.
```
#### 跨域解决方案
##### 1> 在顶级域名相同的情况下，可以通过domain.name来解决，同时设置domain.name=”设置的域名”；
##### 2> jsonp跨域原理是由于浏览器对script，link等标签的src属性没有跨域限制，所以jsonp原理就是通过动态创建一个script的标签，添加到head中，添加回调函数，将格式化的数据拼接到url上，设置超时时间，实现监听状态，指定script的src属性，通过src发送请求实现的跨域，使用后清除script标签和回调函数；由于是通过拼接url实现的跨域，所以只支持get请求方式。
使用 jQuery 集成的 $.ajax 实现 JSONP 跨域调用:
jquery的ajax调用：
```angular2html
$.ajax({
	url: '',
	data:data,
	cache: false,
	timeout: 5000,
	jsonp: 'callback',//jsonp字段的含义为服务器通过什么字段获取回调函数的名称
	jsonpCallback:'jsonpCallback',
	success: function (data){
		console.log('ajax success callback');
	},
	error: function(jqXHR,textStatus,errorThrown){
	console.log(textStatu+''+errorThrown);
	}
	})
	//利用 JSONP 格式返回的值一段要立即执行的 JavaScript 代码，所以不会像 ajax 的 XmlHttpRequest 那样可以监听不同事件对数据进行不同处理

```
3>cors方法，参考阮一峰：http://www.ruanyifeng.com/blog/2016/04/cors.html
