---
title: 函数的调用
date: 2015-10-26 01:56:14
description: 函数的四种调用模式
categories:
- JavaScript 小知识
tags: 
- 函数的四种调用模式
toc: true 文章目录
---

+ 函数的四种调用模式
<!-- more -->
<The rest of contents | 余下全文>

##### 1. 普通函数执行
```angular2html
	this -> window
	声明一个函数后，就直接调用
```

##### 2. 方法调用模式

```angular2html
	this —> 该方法的调用者
	通过一个对象来调用方法
```

##### 3. 构造函数模式
```angular2html
	this -> 当前创建出来的对象
	配合new操作符 来 调用函数
```

##### 4. call/apply(上下文)模式
```angular2html
	this -> 用户动态指定的
	指定call or apply 方法的第一个参数
	区别：
	.call(thisObj, arg1, arg2, …,argN)：thisObj为 this的指向。之后为形参列表，就是fn在执行时的实参列表
	.apply(thisObj, [实参]): thisObj 为 this的指向，数组参数为 fn在执行时的实参
	注意：在非严格模式下，如果thisObj 赋值为 null 或者不传实参，此时this -> window 对象，就相当于 普通函数执行模式。

```

##### 5. 上下文模式的应用

```angular2html
	1. 数组的合并
	Array.prototype.push.apply(arr1, arr2);
	
	2. 借调方法（函数
	
	2.1 借用构造函数
	function parent(name, age, gender) {
		 this.name = name;
		 this.age = age;
		 this.gender = gender;
	 }
	 function child(name, age, gender, address) {
		 parent.call(this, name, age, gender);
		 this.address = address;         
	 }
	 
	2.2 借用原生方法
	求数组中的最大值
	获取内置对象类型
	将伪数组变成真数组
```

##### 6. 数组去重
实现思路：
声明一个函数实现数组去重，取名为unique；
判断浏览器是否支持数组的indexOf方法
如果不支持，就自己实现数组的indexOf方法，并且将其扩展到数组的原型对象上。
遍历数组，首先判断结果数组ret中是否含有当前遍历到的元素，
如果没有，就将当前元素添加到ret中。
循环结束，将ret返回。

##### 7. 面试题
```angular2html
function Foo(){
    getName = function(){ alert(1); };
    return this;
}
Foo.getName = function(){ alert(2); };
Foo.prototype.getName = function(){ alert(3); };
var getName = function(){ alert(4); };
function getName(){ 
       alert(5); 
}
Foo.getName();             // ?
getName();                 // ?
Foo().getName();           // ?
getName();                 // ?
new Foo.getName();         // ?
new Foo().getName();       // ?
new new Foo().getName();   // ?
```
