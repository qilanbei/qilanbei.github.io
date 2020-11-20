---
title: JavaScript 异步操作
date: 2019-09-13 17:12:04
categories:
- JavaScript 进阶
tags:
- Async/await的使用
toc: true 文章目录
---

+ Async/await的使用

<!-- more -->
<The rest of contents | 余下全文>

### Javascript语言的执行环境是"单线程"

在之前的学习中，了解到：Javascript语言的执行环境是"单线程"（single thread）。

就是说：浏览器只分配给js一个主线程用来执行任务即函数，但是每次只能执行一个任务，只有等到当前任务执行完成后，才执行后面的任务，这些任务形成一个任务队列排队等候执行。

其实，JS中是没有线程概念的，所谓的单线程也只是相对于多线程而言。

JS的设计初衷就没有考虑这些，针对JS这种不具备并行任务处理的特性，我们称之为“单线程”。

正如以下代码，先输出1，在输出2，中间执行间隔了1s：

```javascript
function foo() {
    console.log('first');
    setTimeout(( function(){
        console.log('second');
    }), 1000);
}
 
for (var i = 0; i < 10; i++) {
    foo();
}
// expected output: 10个 first 输出完后才会输出10个 second
```

虽然JS运行在浏览器中，是单线程的，但浏览器不是单线程的，javascript引擎线程是浏览器多个线程中的一个；所以当多个事件触发时，会依次放入队列，然后一个一个响应。

所以上面的代码先执行for循环，foo()调用先输出10个 first，而当前有for循环的任务，所以把 setTimeout 的任务加入队列，所以只能等10个first输出完后才会输出 10 个 second

这种模式的好处是实现起来简单，执行环境相对单纯；坏处是如果前面任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。

常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。

为了解决这个问题，Javascript语言将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。

### JavaScript的异步的实现

* 回调函数

同步就不用说了，异步的实现值得一提，回调就是异步编程最基本的方法：

```javascript
function f1(callback){
　　setTimeout(function () {
　　　　callback();
　　}, 1000);
}

function f2 () {}

f1(f2);
```

采用这种方式，相当于先执行程序的主要逻辑，将耗时的操作推迟执行。

优点：简单、容易理解和部署
缺点：不利于代码的阅读和维护，各个部分之间高度耦合，流程会很混乱，而且每个任务只能指定一个回调函数。

* 事件监听

js异步操作另一种实现就是事件驱动模式，浏览器异步处理就是典型的事件驱动，事件机制实际上就是异步过程的通知机制

最基础的异步。事件触发才调用函数，而不是根据代码出现的顺序。

代码运行到指定事件，会把函数加入队列中，事件被触发就会调用函数，比如settimeout，onload。

```javascript
window.onload = function(){
    alert('hello!')
}
```

常用于DOM事件，比如点击鼠标、点击键盘等事件。

```javascript
var button = document.getElementById('#btn');
button.addEventListener('click', function() {
    console.log();
});
```

jquery中的dom监听操作：

```javascript
$('#btn').on('click', function () {})
```

优点：比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以 " 去耦合 "，有利于实现模块化
缺点：只能进行简单交互，串联异步调用麻烦！需要追踪事件对象，需要确保事件触发前绑定完毕。

* 发布订阅

上面的事件模式，我们可以理解成是一个"信号"

如果我们假定存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号；其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。

这就叫做"发布/订阅模式"（publish-subscribe pattern），又称"观察者模式"（observer pattern）。

jQuery中的观察者模式，实际上是让on方法绑定的自定义事件先不执行，直到使用trigger方法来触发事件。

```javascript
$.getJSON('data.json', function(results) {
	$(document).trigger('app.myevent', results); //相当于发布
});
$(document).on('app.myevent', function(e, results) { //相当于订阅
	console.log(results);
});
```

以上所示，on方法就像一个订阅者，它订阅了自定义事件app.myevent；而trigger方法就像一个发布者，它发布事件和参数后，才真正让订阅者方法得以执行。

使用jQuery的观察者模式的好处是：一次发布，多次订阅

发布订阅的使用场合就是：当一个对象的改变需要同时改变其它对象，并且它不知道具体有多少对象需要改变的时候，就应该考虑使用观察者模式。

优点：发布订阅模式所做的工作就是在解耦，让耦合的双方都依赖于抽象，而不是依赖于具体。从而使得各自的变化都不会影响到另一边的变化。

缺点：发布—订阅模式虽然可以弱化对象之间的联系，但如果过度使用的话，对象和对象之间的必要联系也将被深埋在背后，会导致程序难以跟踪维护和理解。
特别是有多个发布者和订阅者嵌套到一起的时候，不易跟踪 bug


* Promise

Promises对象是CommonJS工作组提出的一种规范，目的是为异步编程提供统一接口。

[MDN中对Promises的详解](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

简单理解：每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数

一个 Promise 有以下几种状态:

1. pending: 初始状态，既不是成功，也不是失败状态。
2. fulfilled: 意味着操作成功完成。
3. rejected: 意味着操作失败。

基本用法：

```javascript
new Promise( function(resolve, reject) {...} /* executor */  );
```

executor 是带有 resolve 和 reject 两个参数的函数 。

Promise 构造函数执行时立即调用 executor 函数， resolve 和 reject 两个函数作为参数传递给 executor（ executor 函数在Promise构造函数返回所建promise实例对象前被调用）。

resolve 和 reject 函数被调用时，分别将promise的状态改为 fulfilled（完成）或 rejected（失败）。

executor 内部通常会执行一些异步操作，一旦异步操作执行完毕(可能成功/失败)，要么调用 resolve 函数来将promise状态改成 fulfilled，要么调用 reject 函数将 promise 的状态改为 rejected。

如果在 executor 函数中抛出一个错误，那么该 promise 状态为 rejected。executor函数的返回值被忽略。


Promise也有一些缺点:

首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。

其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。

第三，当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。


* async与await

ES2017 提供了async函数，使得异步操作变得更加方便。async函数是什么？一句话，async函数就是Generator函数的语法糖。

```javascript
async function timeout(ms) {
  await new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);
// expected output: 50ms后输出 'hello world'
```

根据上面代码所示，可以说，async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await命令就是内部then命令的语法糖

### 参考推荐

[深入理解参考](https://segmentfault.com/a/1190000007535316)

[阮一峰的async 函数讲解](https://es6.ruanyifeng.com/#docs/async)

[有对executor相对详细解释](https://juejin.im/post/5c6515e0518825266c3ef852)
