---
title: 有关Object.assign()
date: 2019-07-31 21:10:22
categories:
- JavaScript 进阶
tags: 
- JavaScript 标准内置对象 - Object
toc: true 文章目录
---

+ Object.assign 知识点

<!-- more -->
<The rest of contents | 余下全文>

### Object.assign() 介绍

ES6提供了Object.assign()，用于合并/复制对象的属性。

语法为：`Object.assign(target, source_1, ..., source_n)`

查看[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)上面详细的解释：

> Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

### 使用示例

```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }

```

根据示例可以看出：如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。
后面的源对象的属性将类似地覆盖前面的源对象的属性。

### 特性简介

* 属性名可以为字符串或者 Symbol，String类型和 Symbol 类型的属性都会被拷贝

* 注意: Object.assign 不管source对象值为 null、undefined还是NaN，都不会报错

* 如果 target 参数不是对象，则会先转成对象，然后返回，
由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。

```javascript
Object.assign(undefined) //  报错
Object.assign(null) //  报错

```

* 除了字符串会以数组形式，拷贝入目标对象，其他值(非异常值，如：undefined等)都会被转化为 object 类型

```javascript
Object.assign(1) // 转化为 number 对象
// expected output: Number {1} 
Object.assign(true) // 转化为 boolean 对象
// expected output: Boolean {true} 
```

* Object.assign拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性

* 如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用

```javascript

var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);
obj1.a.b = 2;
console.log(obj2.a.b) 
// expected output: 2

// 因为 `Object.assign` 实现的是浅拷贝，
// 只是将obj1 里面的 对象a 拷贝给 obj2，对象a存储的是{b: 1}的地址
// 目标对象 obj2 拷贝得到的是这个对象的引用，即：obj2.a 指向的是{b: 1}的地址
// 所以当 obj1.a.b = 2; a指向{b: 1}的地址 里面的值发生了改变，
// 自然下次 obj2.a.b 再进行查询的时候 值是变化后的 2 

// 打个比方：我obj1有一把仓库的钥匙a, 
// 你obj2复制了一把和钥匙a一模一样的钥匙，
// 我昨天拿着钥匙a去仓库放了一筐鸡蛋，你在我之后去仓库看，
// 因为是同一个仓库，你也会看到是多了一筐鸡蛋，
// 或者你去放这个鸡蛋，我去看也是你放置后的结果
```

* 当Object.assign用来处理数组时，会把数组视为对象

```javascript
Object.assign([1, 2, 3], [4, 5])
// expected output: [4， 5, 3】
// 因为 Object.assign 会将 [1, 2, 3] 和 [4, 5] 两个数组转化为对象，
// 相当于 {0: 1, 1: 2, 2: 3} 合并 {0: 4, 1: 5}
// 所以输出：[4， 5, 3】
```

### 实际用途

* 初始化对象属性

构造器正是为了初始化对象的属性，通常，我们不得不多次重复属性的名字。
示例代码的constructor中，x与y均重复了两次：

```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
}
```

Object.assign()可以帮助我们减少一些重复：

```javascript
class Point {
    constructor(x, y) {
        Object.assign(this, { x, y });
    }
}
```

* 也可给对象添加方法

```javascript
// 为对象添加方法
Object.assign(SomeClass.prototype, {
	someMethod(arg1, arg2) {},
	anotherMethod() {}
});
//  等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {};

SomeClass.prototype.anotherMethod = function () {};
```

* 合并多个对象

```javascript
const merge =(target, ...sources) => Object.assign(target, ...sources);
```

* 为属性指定默认值

```javascript
const DEFAULTS = {
	params: {},
	data: {},
	headers: {}
};
function funName (options) {
	let options = Object.assign({}, DEFAULTS, options);
}
```

* 复制对象

使用Object.assign()深度复制对象，包括其prototype（深复制）

```javascript
var Point = function(x) {
    this.x = x;
};

Point.prototype.y = 2;

var obj = new Point(1);

var copy1 = Object.assign({ __proto__: obj.__proto__ }, obj); 
// expected output: {x:1,y:2}

// （深复制）或者:

var copy2 = Object.getPrototypeOf(obj);
return Object.assign(Object.create(copy), obj);

console.log(copy1, copy2); 
// expected output: {x:1,y:2}

```

仅复制自身属性（浅复制）：

```javascript
var obj = new Point(1);

var copy = Object.assign({}, obj);

console.log(copy);
// expected output: {x:1}
```

### 归纳参考：

[ES6之Object.assign()详解](https://blog.fundebug.com/2017/09/11/object-assign)

[MDN关于Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
