---
title: js内存泄漏识别和优化方法
date: 2019-04-09 10:32:36
categories:
- JavaScript 小知识
tags: 
- js内存管理
toc: true 文章目录
---
+ js内存泄漏识别和优化方法
<!-- more -->
<The rest of contents | 余下全文>

## 内存泄漏的识别

1. 经验之谈：如果连续五次垃圾回收之后，内存占用一次比一次大，就有内存泄漏。这就要求实时查看内存占用。

2. 浏览器: Chrome 浏览器查看内存占用，按照以下步骤操作:

- 打开开发者工具，选择 Performance 面板 (以前叫做Timeline)
- 在顶部的Capture字段里面勾选 Memory
- 点击左上角的录制按钮。
- 在页面上进行各种操作，模拟用户的使用情况。
- 一段时间后，点击对话框的 stop 按钮，面板上就会显示这段时间的内存占用情况。
如图所示：

![](/images/js-memory.png)

![](/images/js-memory-2.png)

如果内存占用基本平稳，接近水平，就说明不存在内存泄漏，反之，就是内存泄漏了。

了解更多，请查看[这里](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/?hl=zh-cn)

## 内存泄漏的优化方法

1. 避免循环引用，这个我已经在[js内存管理](https://qilanbei.github.io/2019/02/08/js-memory/)文章中介绍到
   
2. 优化内存的一个最好的衡量方式就是只保留程序运行时需要的数据，对于已经使用的或者不需要的数据，应该将其值设为 null，这个方法被称为“解除引用”。需要注意的是，解除一个值的引用不代表垃圾回收器会立即将这段内存回收，这样做的目的是让垃圾回收器在下一个回收周期到来时知道这段内存需要回收

3. 无意的全局变量

```angular2html
function foo(arg) {
    const bar = "";
    <!--或者-->
    bar = ""
    <!--没有声明变量，会被当做全局变量处理-->
}
foo();
```

上面的代码就无意中声明了一个全局变量，会得到 window 的引用，bar 实际上是 window.bar，它的作用域在 window 上，所以 foo 函数执行结束后，bar 也不会被内存收回

另外一种无意的全局变量的情况是：

```angular2html
function foo() {
   <!--在 foo 函数中，this 指的是 window-->
    this.bar = "";
    <!--"use strict"; -->
    <!--this.bar2 = "严格模式下this指向undefined"; -->
    <!--bar = "报错";-->
}
```

不管有意还是无意的设置了全局变量，我们都需要注意，如果声明的全局变量占用大量的内存，那么在使用完后将变量声明为 null

4. 被遗忘的计时器和回调函数

```angular2html
let someResource = getData();
setInterval(() => {
    const node = document.getElementById('Node');
    if(node) {
        node.innerHTML = JSON.stringify(someResource));
    }
}, 1000);
```

上面的例子中，我们每隔一秒就将得到的数据放入到文档节点中去。但在 setInterval 没有结束前，回调函数里的变量以及回调函数本身都无法被回收。那什么才叫结束呢？就是调用了 clearInterval。如果回调函数内没有做什么事情，并且也没有被 clear 掉的话，就会造成内存泄漏。不仅如此，如果回调函数没有被回收，那么回调函数内依赖的变量也没法被回收。上面的例子中，someResource 就没法被回收。同样的，setTiemout 也会有同样的问题。所以，当不需要 interval 或者 timeout 时，最好调用 clearInterval 或者 clearTimeout

5. 在 IE8 以下的版本里，DOM 对象经常会跟 JavaScript 之间产生循环引用

```angular2html
function setHandler() {
    const ele = document.getElementById('id');
    ele.onclick = function() {};
}
```
在这个例子中，DOM 对象通过 onclick 引用了一个函数，然而这个函数通过外部的词法环境引用了这个 DOM 对象，形成了循环引用。不过现在不必担心，因为所有现代浏览器都采用了标记－整理方法，避免了循环引用的问题

除了这种情况，我们现在还会在其他时候在使用 DOM 时出现内存泄漏的问题。当我们需要多次访问同一个 DOM 元素时，一个好的做法是将 DOM 元素用一个变量存储在内存中，因为访问 DOM 的效率一般比较低，应该避免频繁地反问 DOM 元素。所以我们会这样写：

```angular2html
const button = document.getElementById('button');
```

当删除这个按钮时：

```angular2html
document.body.removeChild(document.getElementById('button'));
```

虽然这样看起来删除了这个 DOM 元素，但这个 DOM 元素仍然被 button 这个变量引用，所以在内存上，这个 DOM 元素是没法被回收的。所以在使用结束后，还需要将 button 设成 null。

另外一个值得注意的是，代码中保存了一个列表 ul 的某一项 li 的引用，将来决定删除整个列表时，我们自觉上会认为内存仅仅会保留那个特定的 li，而将其他列表项都删除。但事实并非如此，因为 li 是 ul 的子元素，子元素与父元素是引用关系，所以如果代码保存 li 的引用，那么整个 ul 将会继续呆在内存里。


6. 减少内存垃圾的另一个方法就是避免创建对象

new Object() 是一个比较明显的创建对象的方式，另外 const arr = [];、const obj = {};也会创建新的对象。另外下面这种写法在每次调用函数时都会创建一个新的对象：
```angular2html
function func() {
    return function() {};
}

```
另外，当清空一个数组时，我们通常的做法是 array = []，但这种做法的背后是新建了一个新的数组然后将原来的数组当作内存垃圾。建议的做法是 array.length = 0，这样做不仅可以重用原来的变量，而且还避免创建了新的数组.

7. 闭包可以维持函数内局部变量，使其得不到释放，造成内存泄漏

```angular2html
 function bindEvent() {
      var obj = document.createElement("XXX");
      var unused = function () {
          console.log(obj,'闭包内引用obj obj不会被释放');
      };
      // obj = null;
    }
```
解决方法：手动解除引用，obj = null。

8. console保存大量数据在内存中

过多的console，比如定时器的console会导致浏览器卡死。

解决：合理利用console，线上项目尽量少的使用console

9. 自动类型装箱转换
```angular2html
var s = "test test";
alert(s.length);

```

s 本身是一个 string 而非 object，它没有 length 属性，所以当访问 length 时，JS 引擎会自动创建一个临时 String 对象封装 s，而这个对象一定会泄漏。这个 bug 匪夷所思，所幸解决起来相当容易，记得所有值类型做.运算之前先显示转换一下：
```angular2html
var s = "test test";
alert(new String(s).length);

```
