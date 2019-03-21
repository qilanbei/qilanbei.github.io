---
title: let和const的区别你真的请楚吗
date: 2016-08-20 01:56:14
description: let和const的区别
categories:
- JavaScript 小知识
tags: 
- let和const的区别
toc: true 文章目录
---

+ let和const的区别<!-- more -->
<The rest of contents | 余下全文>

let与const都是只在声明所在的块级作用域内有效
let用来定义变量，无论是不是严格模式下，声明未初始化的时候，都不会报错，和var作用一样，返回undefined
const用来定义常量，而且声明时必须初始化，否则报错

const不能修改定义的常量的值 但是可以修改常量的属性值，例如：
```angular2html
const a = {b: 9};
    a.b = 1;
    console.log(a);
```
因为对于复合类型的变量，如数组和对象，变量名不指向数据，而是指向数据所在的地址。
const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变，所以可以修改const定义的数组或者对象的数值；
也可以通过Object.freeze(name)方法冻结这个常量;例如Object.freeze(a);这时候a的属性值就不能随意更改了

有意思的是：
```angular2html
var let = 1; 在严格模式下报错，非严格模式下，打印let是1
const let = 1;在不在严格模式下都会报错
```
