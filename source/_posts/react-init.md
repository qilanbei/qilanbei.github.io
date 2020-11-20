---
title: react入门学习
date: 2017-10-27 14:57:32
categories:
- react
tags: 
- react入门学习
toc: true 文章目录
---

+ react入门学习
<!-- more -->
<The rest of contents | 余下全文>

关于react自学路程

通过最近的面试给我的反馈就是react无疑已经是许多公司首选框架，我也觉得确实应该好好学习一下了，
自己喜欢整理一些东西，整理的过程可以使自己逻辑更清晰，也是检验自己是不是真的理解这个知识的一种途径。

### 1. 什么是react，react的优缺点

正如官方所说：React is a JavaScript library for building user interfaces.
react是facebook维护的一个为了更好的构建用户界面的js库，它可以在浏览器端运行，也可以在服务端运行。

react优点：

1> 因此虚拟dom的引入大大的提高了性能；
2> 虚拟dom也帮助我们解决了跨浏览器兼容的问题，它为我们提供了一套标准的api，在IE8上也是兼容的；
3> React推荐以组件的方式去重新思考UI构成，将UI上每一个功能相对独立的模块定义成组件，
然后将小的组件通过组合或者嵌套的方式构成大的组件，最终完成整体UI的构建，这样代码更见模块化，重用性高，可维护性高；

react缺点：

1> 首先新框架，都说学习成本高
2> 好多坑要自己慢慢踩，emmmmmm.....
3> React本身只是一个V而已，并不是一个完整的框架，所以如果是大型项目想要一套完整的框架的话，基本都需要加上ReactRouter和Flux才能写大型应用

### 2. react的diff算法简述

在Web开发中，我们总需要将变化的数据实时反应到UI上，这时就需要对DOM进行操作。

而复杂或频繁的DOM操作是很消耗性能的，因此react引入了虚拟DOM（virtual DOM），在浏览器端用Javascript实现了一套DOM API。

基于React进行开发时所有的DOM构造都是通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，
然后React将当前整个DOM树和上一次的DOM树进行对比（当两个节点不同时，应该如何处理。这分为两种情况：

- 节点类型不同 ;
- 节点类型相同，但是属性不同，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新。

而且React能够批处理虚拟DOM的刷新

在一个事件循环（Event Loop）内的两次数据变化会被合并，例如你连续的先将节点内容从A变成B，然后又从B变成A，React会认为UI不发生任何变化；

需要注意的是，删除节点意味着彻底销毁该节点，而不是再后续的比较中再去看是否有另外一个节点等同于该删除的节点。

如果该删除的节点之下有子节点，那么这些子节点也会被完全删除，它们也不会用于后面的比较。

### 3. JSX语法简介

HTML语言直接写在js语言中，不加任何引号，这就是JSX，可以允许html和js混写，例如下面：
```angular2html
var examples = ['name','age','gender'];
React.render(
	<ul>
	{
		examples.map(function(example){
			return <li>{example}</li>;
			}
		)	
	}
	</ul>,
	document.getElementById('container')
);
```

遍历examples数组，并用ul的li的形式输出到界面上；JSX也允许将模板插入到js中，例如：
```angular2html
var arr = {
	<h1>hello world</h1>,
	<h2>hello niwei</h2>,
}
React.render(
	<div>{arr}</div>,
	document.getElementById('container')
);
```
### 4. 关于react的学习

自己练习并整理一些代码，关于代码的理解都有注释，也是自己学习深入的过程
地址：https://github.com/qilanbei/ReactStudy
