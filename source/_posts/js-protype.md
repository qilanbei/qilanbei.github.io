---
title: javascript 原形链的江湖恩怨
date: 2018-03-22 01:56:14
description: javascript 原型链
categories:
- JavaScript 小知识
tags: 
- javascript 原型链
toc: true 文章目录
---

+ javascript 原型链

<!-- more -->
<The rest of contents | 余下全文>

#### 写在前面
说到原型链这个爱恨交织、错综复杂的东西，今天我们就来看看这是个神马东东
首先追本溯源，为啥有原型链这么个东西，这就要讲到原型，继承，对象这些概念
##### 1. 对象
对象 object 说到对象的概念，我们都会仰天长叹：万物皆对象；
在不同的编程语言中,设计者也利用各种不同的语言特性来抽象描述对象,最为成功的流派是使用“类”的方式来描述对象,这诞生了诸如 C++、Java 等流行的编程语言，
###### 然而 JavaScript 为了保证对象之间的联系，引入了原型与原型链的概念
##### 2. 原型(prototype)与原型链
如果说js是基于对象的，那么它也是基于原型的，现在我们先来理解几个点：
1. 对象和函数可以说是相辅相成，对象都是函数创建出来的
```
你会不会抬杠的问：
var a  = {name: 'nihao'}
那你知道不，其实这段代码是这样的：
 var a = new Object()
 a.name = 'nihao'
 你看还是函数创建出来的
```
3. 函数也是一个对象,不信你看
```
var fn = function(){}
fn instanceof Object // true
```
4. 每个实例对象（object ）都有一个私有属性__proto__
5. 每个函数都有一个 prototype 属性
6. 对象的私有属性__proto__指向它的原型对象prototype，该原型对象也有一个自己的原型对象__proto__
7. 只有原型对象才有constructor属性,constructor属性其实是一个Object，指向同名的构造函数
8. 依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾，即直到一个对象的原型对象为 null，根据定义，null 没有原型，并作为这个原型链中的最后一个环节。

根据这几个点，我们通过几张图来简单理解一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019030514255590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
再来一层
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190305144206800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
当把函数也看做一个对象的时候，对象就有__proto__属性，图形就更复杂了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190305151256492.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
