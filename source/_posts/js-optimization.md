---
title: 5个技巧让你书写更好的js条件语句
date: 2019-03-18 10:50:05
categories:
- JavaScript 小知识
tags: 
- 更好的使用js编程
toc: true 文章目录
---
+ 5 Tips to Write Better Conditionals in JavaScript
<!-- more -->
<The rest of contents | 余下全文>


#### 1. 使用 Array.includes 来处理多重条件

让我们看看下面的例子：

```angular2html

function getFruitColor(fruit) {
  if (fruit == 'apple' || fruit == 'strawberry') {
    console.log('red');
  }
}

```

好像能实现。但如果有更多红色的水果需要判断呢，比如樱桃和小红莓，我们要用更多的 || 来扩展这个表述吗

我们可以用 Array.includes 重写上面的条件

```angular2html

function getFruitColor(fruit) {
  // 提取条件存储为数组
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  if (redFruits.includes(fruit)) {
    console.log('red');
  }
}

```
#### 2. 更少的嵌套，尽早返回

让我们扩展前面的示例，以包含另外两个条件：

- 如果没有提供水果（名称），抛出错误。
- 如果（红色水果）数量超过 10 个，接受并打印。

看看上面的代码，我们有：

- 1组过滤无效条件的 if/else 语句
- 3层的 if 嵌套语句（条件 1、2、3）

```angular2html
 <!--当发现无效条件时，提前返回-->
 
function getFruitColor(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
 
  // condition 1: throw error early
  if (!fruit) throw new Error('No fruit!');
   
  // condition 2: must be red
  if (redFruits.includes(fruit)) {
      console.log('red');
   
      // condition 3: must be big quantity
      if (quantity &gt; 10) {
        console.log('big quantity');
      }
  }
}
```

通过反转条件和提早返回，我们可以进一步减少嵌套。看看下面的条件 1、2，我们是怎么做的

```angular2html
function getFruitColor(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
  
  // condition 1: throw error early
  if (!fruit) throw new Error('No fruit!');
   
  // condition 2: stop when fruit is not red
  if (!redFruits.includes(fruit)) return; 
 
  console.log('red');
 
  // condition 3: must be big quantity
  if (quantity &gt; 10) {
    console.log('big quantity');
  }
}
```
但是两个版本相对比而言，上一个版本：

代码简短而直接，如果嵌套，代码就更清晰了
过多的反转条件可能会导致更多的思考过程（增加认知负担）
因此，总是以更少的嵌套及尽早返回为目标，但不要过度。

如果你感兴趣的话，StackOverflow 有一篇相关的文章讨论了这个话题：

- [Avoid Else, Return Early](http://blog.timoxley.com/post/47041269194/avoid-else-return-early) by Tim Oxley
- [StackOverflow discussion](https://softwareengineering.stackexchange.com/questions/18454/should-i-return-from-a-function-early-or-use-an-if-statement) on if/else coding style

#### 3. 使用默认的函数参数和解构

我们在使用 JavaScript 时总是需要检查 null 或 undefined 值并分配默认值

```angular2html

function test(fruit, quantity) {
  if (!fruit) return;
  
  // if quantity not provided, default to one
  const q = quantity || 1; 

  console.log(`We have ${q} ${fruit}!`);
}

//test results
test('banana'); // We have 1 banana!
test('apple', 2); // We have 2 apple!

```

事实上，我们可以通过指定默认的函数参数来消除变量 q

```angular2html
function test(fruit, quantity = 1) {
  if (!fruit) return;

  console.log(`We have ${quantity} ${fruit}!`);
}
```

如果我们的 fruit 是一个对象呢？我们可以指定默认参数吗？

```angular2html

function test(fruit) {
  // printing fruit name if value provided
  if (fruit && fruit.name)  {
    console.log (fruit.name);
  } else {
    console.log('unknown');
  }
}
 
//test results
test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple

```

请看上面的示例，如果 fruit.name 是有效的，打印该水果名称，否则我们将打印 unknown

我们可以避免使用与默认函数参数和解构对条件 fruit && fruit.name 进行检查

```angular2html

function test({name} = {}) {
  console.log (name || 'unknown');
}

```
这样做的好处有：

- 因为我们只要水果中的属性 name，所以我们可使用 {name} 来解构，
然后我们可以在代码中使用 name 作为变量，而不是 fruit.name

- 我们还将空对象 {} 指定为默认值。如果我们不这样做，
当执行 test(undefined)，不能解构 undefined 或 null 的属性名时，您将会得到错误。因为在 undefined没有 name 属性

如果您不介意使用第三方库，有一些方法可以减少 null 检查：

- 使用 Lodash 的 get 函数
- 使用 Facebook 的开源库 [idx](https://github.com/facebookincubator/idx)（以及 Babeljs）

以下是使用Lodash的示例：
```angular2html
// Include lodash library, you will get _
function test(fruit) {
  console.log(__.get(fruit, 'name', 'unknown'); // get property name, if not available, assign default value 'unknown'
}
 
//test results
test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple

```

此外，如果您是函数式编程（FP）的粉丝，您可以选择使用[Lodash fp](https://github.com/lodash/lodash/wiki/FP-Guide)，Lodash的函数式版本（方法为get或getOr）。

#### 4. 选择 Map 或对象字面量，而不是 Switch 语句

我们想要基于颜色打印水果名称:

```angular2html
function test(color) {
  // use switch case to find fruits in color
  switch (color) {
    case 'red':
      return ['apple', 'strawberry'];
    case 'yellow':
      return ['banana', 'pineapple'];
    case 'purple':
      return ['grape', 'plum'];
    default:
      return [];
  }
}
 
//test results
test(null); // []
test('yellow'); // ['banana', 'pineapple']

```
我们发现虽然可以实现但代码相当冗长。同样的结果可以通过对象字面量和更简洁的语法来实现：

```angular2html
// use object literal to find fruits in color
  const fruitColor = {
    red: ['apple', 'strawberry'],
    yellow: ['banana', 'pineapple'],
    purple: ['grape', 'plum']
  };
 
function test(color) {
  return fruitColor[color] || [];
}
```

或者，可以使用 Map 来实现相同的结果：

```angular2html
// use Map to find fruits in color
  const fruitColor = new Map()
    .set('red', ['apple', 'strawberry'])
    .set('yellow', ['banana', 'pineapple'])
    .set('purple', ['grape', 'plum']);
 
function test(color) {
  return fruitColor.get(color) || [];
}
```
[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) 是 ES2015 以后可用的对象类型，允许您存储键值对。

Todd Motto有一篇文章深入研究switch语句与对象文字，你可以在[这里](https://toddmotto.com/deprecating-the-switch-statement-for-object-literals/)阅读。

#### 重构的语法

对于上面的例子，我们实际上可以使用Array.filter来重构我们的代码:

```angular2html
const fruits = [
   { name: 'apple', color: 'red' },
   { name: 'strawberry', color: 'red' },
   { name: 'banana', color: 'yellow' },
   { name: 'pineapple', color: 'yellow' },
   { name: 'grape', color: 'purple' },
   { name: 'plum', color: 'purple' }
];
 
function test(color) {
 // use Array filter to find fruits in color
 
 return fruits.filter(f => f.color == color);
}
```

#### 5. 使用Array.every和Array.some

```angular2html
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ];
function test() {
  let isAllRed = true;
  // condition: all fruits must be red
  for (let f of fruits) {
    if (!isAllRed) break;
    isAllRed = (f.color == 'red');
  }
  console.log(isAllRed); // false
}
```
代码太长了！我们可以通过Array.every减少行数：
```angular2html
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ];
function test() {
  // condition: short way, all fruits must be red
  const isAllRed = fruits.every(f => f.color == 'red');
  console.log(isAllRed); // false
}
```
以类似的方式，如果我们想测试水果中是否有红色，我们可以用Array.some来实现它
```angular2html
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
];
function test() {
  // condition: if any fruit is red
  const isAnyRed = fruits.some(f => f.color == 'red');
  console.log(isAnyRed); // true

```

参考：[ 5 Tips to Write Better Conditionals in JavaScript ](https://scotch.io/bar-talk/5-tips-to-write-better-conditionals-in-javascript) by Jecelyn Yeen

