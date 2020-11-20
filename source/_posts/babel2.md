---
title: Babel学习之旅(二)
date: 2019-05-01 17:02:31
categories:
- Babel
tags: 
- Babel学习笔记
toc: true 文章目录
---
+ 记录有关babel的学习旅程
<!-- more -->
<The rest of contents | 余下全文>

## 写在前面

上篇 babel学习之旅(一) 我们介绍了 What is Babel 以及 简单的配置，这篇文章 我们现在整体看一下babel是如何工作的

## Babel是如何工作的
    
Babel 会将源码转换 AST 之后，通过便利AST树，对树做一些修改，然后再将AST转成code，即成源码

![](/images/babel-ast.jpeg)

图片来源 [alloyteam](http://www.alloyteam.com/2017/04/analysis-of-babel-babel-overview/)
 
如上图所示，babel的编译过程和大多数其他语言的编译器大致相同，可以分为三个阶段:

- **解析(PARSE)**：将代码字符串解析成 AST (抽象语法树)。

- **转换(TRANSFORM)**：对 AST (抽象语法树)进行转换操作。

- **生成(GENERATE)**: 根据变换后的 AST (抽象语法树)再生成代码字符串。

## 解析

Babel的解析引擎是 [Babylon](https://github.com/babel/babylon),  babylon 是一个解析器，它可以将 JavaScript 字符串转换为对计算机来说更加友好的表现形式，称之为抽象语法树 AST（Abstract Syntax Tree）,

babylon并非由babel团队自己开发的，而是fork的acorn项目，为了是做一些代码的转换，是很不错的一款引擎，

不过acorn引擎只提供基本的解析 AST 的能力，遍历还需要配套的acorn-travesal, 替换节点需要使用acorn-，而这些开发，在Babel的插件体系开发下，变得一体化了。

**解析**的过程分为两步：

- 分词： 将整个代码字符串分割成 语法单元数组
- 语义分析：在分词结果的基础之上分析 语法单元之间的关系

什么是分词和语义分析，[请查阅这里](https://www.cnblogs.com/tugenhua0707/p/7863616.html)

babel转AST的解析过程在Babylon中完成, 解析成AST树使的是babylon.parse方法

## 转换

比如我们在 .babelrc 里配置的 presets 和 plugins (下面将介绍babel的配置)是在第二步 转换中 进行的。

[babel-traverse](https://www.npmjs.com/package/babel-traverse) 模块允许你浏览、分析和修改抽象语法树（AST）

## 生成

[babel-generator](https://www.npmjs.com/package/babel-generator) 模块用来将转换后的抽象语法树（AST）转换为 JavaScript 字符串。
