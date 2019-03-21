---
title: es6的扩展运算符
date: 2018-08-22 00:56:14
description: es6的扩展运算符
categories:
- JavaScript 小知识
tags: 
- es6的扩展运算符
toc: true 文章目录
---

+ es6的扩展运算符 ... 你了解多少呢

<!-- more -->
<The rest of contents | 余下全文>


##### 写在前面
之前学习react 过程中，遇到... 知道的不是很清楚，只是知道例如下面这样...arr1 相当于2, 4, 5, 6
```
let arr1 = [2, 4, 5, 6]
let arr2 = [...arr1]
arr2 输出为 [2, 4, 5, 6]
```
程序员一定要一直保持学习的状态，反正我就是秉着这样的心态在前端这个领域一直摸索，以至于我的颈椎、前脑门、曾经一抓一大把的头发。。。你们懂得
言归正传，我查了好多资料，现在整理一下我对这个...得认知：
人家有个专业名词，叫做es6的扩展运算符，是一种展开语法，有什么用处呢，我们来看看

在MDN上面的介绍比较详细了:
>展开语法(Spread syntax), 可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；还可以在构造字面量对象时, 将对象表达式按key-value的方式展开。(译者注: 字面量一般指 [1, 2, 3] 或者 {name: "mdn"} 这种简洁的构造方式)可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开

什么意思呢？就想我开篇写的那样  ...arr1 就相当于2, 4, 5, 6
let arr2 = [...arr1] 就相当于let arr2 = [2, 4, 5, 6]，...起到的作用将一个数组转为用逗号分隔的参数序列
##### 具体用处简单总结如下：
1. 合并数组
```
// ES5 的写法
let arr1 = [1, 2, 3]
let arr2 = [4, 5, 6]
Array.prototype.push.apply(arr1, arr2) ==> [1, 2, 3, 4, 5, 6]
// ES6 的写法
arr1.push(...arr2)
```
2. 数组最大元素
```
var args = [1, 3, 5];
// ES5 的写法
Math.max.apply(null, args)  ==>5
// ES6 的写法
Math.max(...args) ==>5
// 是不是简单了许多
```
3. 可以解构赋值结合, 用于生成数组，就是把剩余的东西放到一个array里面赋值给它。一般只针对array的解构
```
let arr = [1, 2, 3, 4]
let first = arr[0]
let rest = arr.slice(1) 
console.log(item) ==>[2, 3, 4]
// 使用扩展运算符
[first, ...rest] = arr
console.log(item) ==>[2, 3, 4]
* 需要注意的是：如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错
```
4. 在对象的使用
```
let obj1 = {
a: 1,
b: 2
}
let obj2 = {
c: 3,
...obj1
}
```
5. 扩展运算符还可以将字符串转为数组
```
[...'strToArr'] ==> ['s', 't', 'r', 'T', 'o', 'A', 'r', 'r']
```
6. 好多获取dom的方法，例如querySelectorAll方法返回的是一个nodeList对象。它不是数组，而是一个类似数组的对象。可以扩展运算符可以将其转为真正的数组
```
let domArr =[...document.querySelectorAll('.list')]
```
7. 扩展运算符内部调用的是数据结构的 Iterator 接口(遍历器)，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构
```
let map = new Map([
[1, 'one'],
[2, 'two'],
[3, 'three'],
]);
let arr = [...map.keys()]; // [1, 2, 3]
```
今天肩膀有点痛哦，加油吧，小伙伴们 !!!

对了，补充一下，如果在vue项目中用到...报错，需要安装一个插件： npm install --save-dev babel-plugin-transform-object-rest-spread 
在.babelrc文件中配置： {"plugins":["transform-object-rest-spread"]} 

