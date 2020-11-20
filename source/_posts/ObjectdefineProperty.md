---
title: Object.defineProperty()有何名堂？
date: 2019-09-19 15:44:51
categories:
- JavaScript 进阶
tags: 
- JavaScript 标准内置对象 - Object
toc: true 文章目录
comments: true
---

+ 有关Object.defineProperty()

<!-- more -->
<The rest of contents | 余下全文>

### defineProperty是个啥

来自[MDN中的解释](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)：

>Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

语法： Object.defineProperty(obj, prop, descriptor)

参数：

* object 必需。 要在其上添加或修改属性的对象。 这可能是一个本机 JavaScript对象（即用户定义的对象或内置对象）或 DOM 对象。
* prop(property name) 必需。 一个包含属性名称的字符串。
* descriptor 必需。 属性描述符。 它可以针对数据属性或访问器属性。

属性描述符：

- 数据描述符是一个具有值的属性，该值可能是可写的，也可能不是可写的

- 存取描述符是由getter-setter函数对描述的属性

数据描述符还具有以下可选键值：

- value: 该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。

- writable: 当且仅当该属性的 writable 键值为 true 时，属性的值，也就是上面的 value，才能被赋值运算符改变。默认为 false。

- configurable: 当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。

- enumerable: 当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。默认为 false。

存取描述符还具有以下可选键值：

- get: 当访问该属性时会调用此函数。执行时不传入任何参数，但会传入 this 对象（由于继承关系，this 不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。默认为 undefined。

- set: 当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。默认为 undefined。

- configurable: 同数据描述符configurable

- enumerable: 同数据描述符enumerable

注意: 

- 属性描述符有以上两种主要形式：数据描述符和存取描述符，描述符必须是这两种形式之一，不能同时是两者；即：如果一个描述符同时拥有 value 或 writable 和 get 或 set 键，则会产生一个异常

- 如果一个描述符不具有 value、writable、get 和 set 中的任意一个键，那么它将被认为是一个数据描述符。

- 这些选项不一定是自身属性，也要考虑继承来的属性。为了确认保留这些默认值，在设置之前，可能要冻结 Object.prototype，明确指定所有的选项，或者通过 Object.create(null) 将 __proto__ 属性指向 null

```javascript
// 使用 __proto__
var obj = {};
var descriptor = Object.create(null); // 没有继承的属性
// 默认没有 enumerable，没有 configurable，没有 writable
descriptor.value = 'static';
Object.defineProperty(obj, 'key', descriptor);

// 以上相当于显式写法，如下
Object.defineProperty(obj, "key", {
  enumerable: false,
  configurable: false,
  writable: false,
  value: "static"
});
```

- 当 writable 属性设置为 false 时，该属性被称为“不可写的”，它不能被重新赋值，哪怕是相同的赋值。

```javascript
var obj = {}

Object.defineProperty(obj, 'a', {
  value: 37,
  writable: false
})

obj.a = 25; // 结果不会报错，但是obj.a的数值还是37，不会被改变

// 但是在严格模式下，这样操作就会报异常
(function () {
  'use strict'
  var obj = {};
  Object.defineProperty(obj, 'b', {
    value: 37,
    writable: false
  });
  obj.b = 25;
  return obj.b;
}())
// 抛异常：Cannot assign to read only property 'b' of object
```

示例:

1、利用defineProperty()方法创建属性

```javascript
// 添加一个数据描述符
var obj = {}

Object.defineProperty(obj, 'a', {
  value: 37,
  writable: true,
  configurable: true,
  enumerable: true
})

// 给对象添加一个存取描述符
var bValue;
Object.defineProperty(obj, 'b', {
  get: function () { return bValue },
  set: function (newValue) { bValue = newValue },
  configurable: true,
  enumerable: true
})

// 这样设置后，除非重新定义 obj.b，否则 obj.b 的值总是与 bValue 相同，无论 bValue 的值如何变化

bValue = 4

console.log(obj.b) // expected output: 4

```

2. enumerable 定义了对象的属性是否可以在 for...in 循环和 Object.keys() 中被枚举。

```javascript
var o = {};
Object.defineProperty(o, "a", { value : 1, enumerable: true });
Object.defineProperty(o, "b", { value : 2, enumerable: false });
Object.defineProperty(o, "c", { value : 3 }); // enumerable 默认为 false

o.d = 4; // 如果使用直接赋值的方式创建对象的属性，则 enumerable 为 true

// 通过 判断是否可枚举
o.propertyIsEnumerable('a'); // true
o.propertyIsEnumerable('b'); // false
o.propertyIsEnumerable('c'); // false
o.propertyIsEnumerable('d'); // true

var p = { ...o }

p.a // undefined 不可枚举
// 扩展运算符本身也是要通过for in Object.keys来遍历取值赋值，如果 enumerable 为 false 取不到值

```

3. configurable 特性表示对象的属性是否可以被删除，以及除 value 和 writable 特性外的其他特性是否可以被修改

```javascript
var o = {};
Object.defineProperty(o, 'a', {
  get() { return 1; },
  configurable: false
});

Object.defineProperty(o, 'a', {
  configurable: true
}); // throws a TypeError
```

4. 考虑特性被赋予的默认特性值非常重要，通常，使用点运算符和 Object.defineProperty() 为对象的属性赋值时，数据描述符中的属性默认值是不同的

```javascript
var o = {};

o.a = 1;
// 等同于：
Object.defineProperty(o, "a", {
  value: 1,
  writable: true,
  configurable: true,
  enumerable: true
});


// 另一方面，
Object.defineProperty(o, "a", { value : 1 });
// 等同于：
Object.defineProperty(o, "a", {
  value: 1,
  writable: false,
  configurable: false,
  enumerable: false
});
```

5. Getter & Setter 

```javascript
// Getter 为读取属性时调用的函数. Setter 为设置属性是调用的函数, Setter 会有一个参数, 即设置的那个值
function Archiver() {
  var temperature = null;
  var archive = [];

  Object.defineProperty(this, 'temperature', {
    get: function() {
      console.log('get!');
      return temperature;
    },
    set: function(value) {
      temperature = value;
      archive.push({ val: temperature });
    }
  });
  
  this.getArchive = function () {
    return archive
  }
}

var arc = new Archiver();
arc.temperature; // expected output: 'get!'
arc.temperature = 11;
arc.temperature = 13;

console.log(arc.getArchive()) // expected output: [{ val: 11 }, { val: 13 }]
```

6. 继承属性

```javascript
// 如果访问者的属性是被继承的，它的 get 和 set 方法会在子对象的属性被访问或者修改时被调用。如果这些方法用一个变量 value 存值，该值会被所有对象共享

function myClass () {}

var value;

Object.defineProperty(myClass.prototype, 'x', {
  get () {
    return value
  },
  set (v) {
    value = v
  }
})

var a = new myClass()
var b = new myClass()

a.x = 1

console.log(b.x) // expected output: 1

myClass.prototype.isPrototypeOf(a) // true
myClass.prototype.isPrototypeOf(b) // true

// 这可以通过将值存储在另一个属性中解决。在 get 和 set 方法中，this 指向某个被访问和修改属性的对象

function myClass () {}

Object.defineProperty(myClass.prototype, 'x', {
  get () {
    return this.store_x
  },
  set (v) {
    this.store_x = v
  }
})

var a = new myClass()
var b = new myClass()

a.x = 1

console.log(b.x) // expected output: undefined
```
