---
title: core-js@3
date: 2020-08-18 10:08:48
categories:
- Babel 进阶
tags: 
- Babel学习笔记
toc: true 文章目录
---
+ core-js@3升级之旅
<!-- more -->
<The rest of contents | 余下全文>

### core-js 是什么，有什么作用

1. core-js是一个用于JavaScript的模块化标准库
  * 它支持最新的ECMAScript标准，包含截至2019年ECMAScript的polyfill：promises，符号，集合，迭代器，类型化数组等许多其他功能
  * ECMAScript 标准库提案
  * 一些 WHATGW / W3C 标准（跨平台或者 ECMAScript 相关）
2. 最大程度模块化：可以轻松地选择仅加载将要使用的功能
3. 使用它而不会导致全局命名空间污染
4. 它与babel紧密集成：这允许对core-js导入进行许多优化

来自作者[官方阐述](https://github.com/zloirock/core-js/blob/master/docs/2019-03-19-core-js-3-babel-and-a-look-into-the-future.md)，它是最普遍、最流行给 JavaScript 标准库打补丁的方式，但是有很大一部分开发者并不知道他们间接的使用了core-js，
比如你使用了 babel-runtime、babel-plugin-transform-runtime 或者 babel-polyfill，你就已经间接的引入了 core-js 标准库。

### core-js@3 有哪些变化

参考：https://juejin.im/post/6844904055005773831

### babel重遇到core-js版本问题的处理

参考：https://segmentfault.com/a/1190000020237817

### 升级core-js@3动机

参考：https://www.cnblogs.com/sefaultment/p/11631314.html

### core-js@3升级之旅

参考：https://segmentfault.com/a/1190000020237817
