---
title: js数组相关方法
date: 2018-04-09 10:32:36
categories:
- JavaScript 小知识
tags: 
- js数组相关方法
toc: true 文章目录
---
+ js数组相关方法
<!-- more -->
<The rest of contents | 余下全文>

### 写在前面

一直都对数组的好多方法了解的不是很清晰，就想总结起来，方便查阅和记忆

### forEach

**定义**：forEach() 方法对数组的每个元素执行一次提供的函数。

**语法**：
```angular2html
arr.forEach(callback[, thisArg])
```

**参数**：
- callback 为数组中每个元素执行的函数，该函数接收三个参数：
	currentValue 数组中正在处理的当前元素。
	index (可选) 数组中正在处理的当前元素的索引。
	array (可选) forEach() 方法正在操作的数组。
- thisArg (可选) 当执行回调函数时用作 this 的值(参考对象)。
	如果使用箭头函数表达式来传入函数参数，thisArg 参数会被忽略，因为箭头函数在词法上绑定了 this 值
	
**返回值**: undefined

**注意**：
- 传递给 callback 的值是 forEach 遍历到他们那一刻的值，那些已删除或者未初始化的项将被跳过
- 如果给 forEach() 传递了 thisArg 参数，当调用时，它将被传给 callback 函数，作为它的 this 值。否则，将会传入 undefined 作为它的 this 值。callback 函数最终可观察到 this 值，这取决于函数观察到 this 的常用规则。
- 并且不可链式调用
- 没有办法中止或者跳出 forEach() 循环，除了抛出一个异常。如果你需要这样，使用 forEach() 方法是错误的。

**示例**：
```angular2html
var array1 = ['a', 'b', 'c'];

array1.forEach(function(element) {
  console.log(element);
});
```
<hr>

### slice

**定义**：slice() 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。

**语法**：
```
arr.slice([begin[, end]])
```

**参数**：
- begin （可选）
  提取起始处的索引（从 0 开始），从该索引开始提取原数组元素。
  如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，slice(-2) 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
  如果省略 begin，则 slice 从索引 0 开始。
  如果 begin 大于原数组的长度，则会返回空数组。
- end （可选）
  提取终止处的索引（从 0 开始），在该索引处结束提取原数组元素。
  slice 会提取原数组中索引从 begin 到 end 的所有元素（ **包含 begin，但不包含 end** ）。


### Map

**定义**：map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果

**语法**：
```angular2html
<!--callback 函数会被自动传入三个参数：数组元素，元素索引，原数组本身-->
var new_array = arr.map(function callback(currentValue[, index[, array]]) {
 // Return element for new_array 
}[, thisArg])
```
**参数**：
- callback 生成新数组元素的函数，使用三个参数：
  currentValue
  callback 数组中正在处理的当前元素。
  index可选
  callback 数组中正在处理的当前元素的索引。
  array可选
  callback  map 方法被调用的数组。
- thisArg (可选) 执行 callback 函数时使用的this 值。
  如果 thisArg 参数有值，则每次 callback 函数被调用的时候，this 都会指向 thisArg 参数上的这个对象。如果省略了 thisArg 参数,或者赋值为 null 或 undefined，则 this 指向全局对象

**返回值**: 一个新数组，每个元素都是回调函数的结果

**注意**：
- map 不修改调用它的原数组本身（当然可以在 callback 执行时改变原数组）

```angular2html
<!--求数组中每个元素的平方根-->
var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt);
```
**示例**：

```angular2html
<!--下面的例子演示如何在一个 String  上使用 map 方法获取字符串中每个字符所对应的 ASCII 码组成的数组：-->
var map = Array.prototype.map
var a = map.call("Hello World", function(x) { 
  return x.charCodeAt(0); 
})
<!--a的值为[72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100]-->
```
<hr>

### filter

**定义**：filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

**语法**：
```angular2html
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
```
**参数**：
- callback
	用来测试数组的每个元素的函数。返回 true 表示该元素通过测试，保留该元素，false 则不保留。它接受一下三个参数：
	element
	数组中当前正在处理的元素。
	index可选
	正在处理的元素在数组中的索引。
	array可选
	调用了 filter 的数组本身。
- thisArg（可选），执行 callback 时，用于 this 的值。

**返回值**: 一个新的、由通过测试的元素组成的数组，如果没有任何数组元素通过测试，则返回空数组。

**注意**：
- filter 提供一个 thisArg 参数，则它会被作为 callback 被调用时的 this 值。否则，callback 的 this 值在非严格模式下将是全局对象，严格模式下为 undefined
- filter 不会改变原数组，它返回过滤后的新数组


**示例**：
```angular2html
<!--该数组的元素由原数组中值大于 10 的元素组成-->
function isBigEnough(element) {
  return element >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered is [12, 130, 44]

<!--使用 filter() 根据搜索条件来过滤数组内容-->

var fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];

/**
 * Array filters items based on search criteria (query)
 */
function filterItems(query) {
  return fruits.filter(function(el) {
      return el.toLowerCase().indexOf(query.toLowerCase()) > -1;
  })
}

console.log(filterItems('ap')); // ['apple', 'grapes']
console.log(filterItems('an')); // ['banana', 'mango', 'orange']

```
<hr>

### find

**定义**：find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。

**语法**：
```angular2html
arr.find(callback[, thisArg])
```
**参数**：
- callback，在数组每一项上执行的函数，接收 3 个参数：
	element，当前遍历到的元素。
	index可选，当前遍历到的索引。
	array可选，数组本身。
- thisArg可选，执行回调时用作this 的对象。

**返回值**: 数组中第一个满足所提供测试函数的元素的值，否则返回 undefined。

**注意**：
- 如果数组中一个尚未被callback函数访问到的元素的值被callback函数所改变，那么当callback函数访问到它时，它的值是将是根据它在数组中的索引所访问到的当前值。被删除的元素仍旧会被访问到
- 当在回调中删除数组中的一个值时，当访问到这个位置时，其传入的值时 undefined
- find方法不会改变数组。

**示例**：
```angular2html
<!--用对象的属性查找数组里的对象-->

var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function findCherries(fruit) { 
    return fruit.name === 'cherries';
}

console.log(inventory.find(findCherries)); // { name: 'cherries', quantity: 5 }
```
<hr>

### every

**定义**：every() 方法测试数组的所有元素是否都通过了指定函数的测试。

**语法**：
```angular2html
arr.every(callback[, thisArg])
```
**参数**：
- callback 用来测试每个元素的函数。
- thisArg 执行 callback 时使用的 this 值。

**返回值**: every 和数学中的"所有"类似，当所有的元素都符合条件才返回true。另外，空数组也是返回true。(空数组中所有元素都符合给定的条件，注：因为空数组没有元素)。

**注意**：
- 如果数组中存在的元素被更改，则他们传入 callback 的值是 every 访问到他们那一刻的值
- every 不会改变原数组。

**示例**：
```angular2html
<!--检测数组中的所有元素是否都大于 10-->
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// passed is false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// passed is true

```

**兼容性**:

```angular2html
<!--你可以把下面的代码放到脚本的开头来解决此问题，该代码允许在那些没有原生支持 every 的实现环境中使用它-->
if (!Array.prototype.every)
{
  Array.prototype.every = function(fun /*, thisArg */)
  {
    'use strict';

    if (this === void 0 || this === null)
      throw new TypeError();

    var t = Object(this);
    var len = t.length >>> 0;
    if (typeof fun !== 'function')
        throw new TypeError();

    var thisArg = arguments.length >= 2 ? arguments[1] : void 0;
    for (var i = 0; i < len; i++)
    {
      if (i in t && !fun.call(thisArg, t[i], i, t))
        return false;
    }

    return true;
  };
}
```
<hr>

### fill 

**定义**：fill() 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引

**语法**：
```angular2html
arr.fill(value[, start[, end]])
```
**参数**：
- value，用来填充数组元素的值。
- start 可选，起始索引，默认值为0。
- end 可选，终止索引，默认值为 this.length。

**返回值**: 修改后的数组。

**注意**：
- 如果 start 是个负数, 则开始索引会被自动计算成为 length+start，如果 end 是个负数, 则结束索引会被自动计算成为 length+end
- fill 方法故意被设计成通用方法, 该方法不要求 this 是数组对象
- 当一个对象被传递给 fill方法的时候, 填充数组的是这个对象的引用

**示例**：
```angular2html
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5);         // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}

// Objects by reference.
var arr = Array(3).fill({}) // [{}, {}, {}];
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```

<hr>

### includes

**定义**：includes() 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。

**语法**：
```angular2html
arr.includes(valueToFind[, fromIndex])
```

**参数**：
- valueToFind，需要查找的元素值。
- fromIndex 可选，从fromIndex 索引处开始查找 valueToFind。如果为负值，则按升序从 array.length + fromIndex 的索引开始搜 （即使从末尾开始往前跳 fromIndex 的绝对值个索引，然后往后搜寻）。默认为 0。

**返回值**: 返回一个布尔值 Boolean ，如果在数组中找到了（如果传入了 fromIndex ，表示在 fromIndex 指定的索引范围中找到了）则返回 true 。

**注意**：
- 使用 includes()比较字符串和字符时是区分大小写
- fromIndex 大于等于数组的长度，则会返回 false，且该数组不会被搜索
- 如果 fromIndex 为负值，计算出的索引将作为开始搜索searchElement的位置, 如果计算出的索引小于 0，则整个数组都会被搜索

**示例**：
```angular2html
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true

<!--includes() 方法有意设计为通用方法。它不要求this值是数组对象，所以它可以被用于其他类型的对象 (比如类数组对象)。下面的例子展示了 在函数的 arguments 对象上调用的 includes() 方法。-->
(function() {
  console.log([].includes.call(arguments, 'a')); // true
  console.log([].includes.call(arguments, 'd')); // false
})('a','b','c');
```
<hr>

### reduce

**定义**：reduce() 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值

**语法**：
```angular2html
arr.reduce(callback[, initialValue])
```
**参数**：
- callback, 执行数组中每个值的函数，包含四个参数：
	accumulator，累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（见于下方）。
	currentValue，数组中正在处理的元素。
	currentIndex可选，数组中正在处理的当前元素的索引。 如果提供了initialValue，则起始索引号为0，否则为1。
	array可选，调用reduce()的数组
- initialValue 可选, 作为第一次调用 callback函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

**返回值**: 函数累计处理的结果

**注意**：
- 如果没有提供initialValue，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供initialValue，从索引0开始。
- 如果数组为空且没有提供initialValue，会抛出TypeError

**示例**：
```angular2html
[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
  return accumulator + currentValue;
});
```
callback 被调用四次，每次调用的参数和返回值如下表：

| callback | accumulator | currentValue | currentIndex | array | return value
| :--- | :---| :--- | :--- | :--- | :--- |
| first call | 0 | 1 | 1 | [0, 1, 2, 3, 4] | 1 | 
| second call |	1 | 2 |	2 |	[0, 1, 2, 3, 4] | 3 |
| third call | 3 | 3 |	3 |	[0, 1, 2, 3, 4]	| 6 |
| fourth call |	6 |	4 |	4 |	[0, 1, 2, 3, 4] | 10 |

常见使用：
```angular2html
<!--数组求和-->
function arrSum (arr) {
	return arr.reduce((pre, curr) => {
		return pre + curr
	}, 0)
}

```
