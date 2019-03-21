---
title: 浏览器加载解析过程
date: 2016-08-20 01:56:14
description: 浏览器加载解析过程
categories:
- 浏览器工作相关
tags: 
- 浏览器加载解析过程
toc: true 文章目录
---

+ 浏览器加载解析过程
<!-- more -->
<The rest of contents | 余下全文>

在面试的时候，有的面试官会问到js加载是不是会阻塞DOM树的解析和渲染，css会不会，或者问到相关问题，就想总结一下，首先有几个问题，自己在查询资料的时候，答案众说纷纭，通过自己学习，总结如下，也欢迎纠正
1.css会不会阻塞DOM树的解析和渲染?
css是不会阻塞DOM树的解析；因为当浏览器解析到link的href去加载外部css文件的时候，浏览器还是会继续加载dom结构，为了效率，它会把该完成的都解析完，生成相应的dom树，但是会阻塞dom的渲染，它要等外部的css文件加载解析完，才会解析style标签里的样式，这时候才是根据样式的顺序也好，权重也好去渲染dom。
```
//小demo:
//demo.css文件：
.demo {
	background-color: yellow !important;   //1
}
//.html文件
<link ref="stylesheet" href="./demo.css">
//style:
	.demo {
		width: 200px;
		height: 200px;
		background-color: red !important;   //2
	}
//body:
	<div class="demo" style="background-color: blue">你好</div>  //3
	显示的你好是什么颜色? 
```
2.js会不会阻塞DOM树的解析和渲染?
js的加载会阻塞DOM树的解析和渲染，还有后面一些资源的下载，js的加载和执行有以下特点：
1>载入js后浏览器会立即执行
2>执行时会阻塞后面内容的构建，渲染及以下资源的下载，因为浏览器需要一个稳定的DOM树结构，而JS中很有可能有代码直接改变了DOM树结构，比如使用 document.write 或 appendChild,甚至是直接使用的location.href进行跳转，浏览器为了防止出现JS修 改DOM树，需要重新构建DOM树的情况，所以 就会阻塞其他的下载和呈现。
```
//运行个demo，自己可以测试一下：
//把脚本放在头部和放在底部，在js里面去获取dom元素，看看能不能获取到，或者写个死循环，让脚本一直运行，看dom能显示出来不 
<script>
		while(true){
			console.log(1);
		}
</script>
```
