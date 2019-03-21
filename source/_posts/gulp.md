---
title: Gulp学习笔记
date: 2016-04-26 00:41:29
description: 关于Gulp的学习笔记
categories:
- 构建工具
tags: 
- Gulp学习
toc: true 文章目录
---
+ 关于Gulp的学习笔记
<!-- more -->
<The rest of contents | 余下全文>

#### 1. Gulp概述
##### gulp是前端开发过程中一种基于流的代码构建工具，是自动化项目的构建利器；不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成；使用它，不仅可以很愉快的编写代码，而且大大提高我们的工作效率
+ gulp是基于Nodejs的自动任务运行器
+ 她能自动化地完成javascript、coffee、sass、less、html/image、css 等文件的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。
+ 在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。
> 流的补充:
> 简单来说就是建立在面向对象基础上的一种抽象的处理数据的工具
在流中，定义了一些处理数据的基本操作，如读取数据，写入数据等
程序员是对流进行所有操作的，而不用关心流的另一头数据的真正流向
而gulp正是通过流和代码优于配置的策略来尽量简化任务编写的工作。这看起来有点像jQuery的方法，把动作串起来创建构建任务

#### 2. Gulp安装
##### 2.1 安装步骤
1. 首先确保你已经正确安装了node环境。然后以全局方式安装gulp：
npm install -g gulp
2. 全局安装gulp后，还需要在每个要使用gulp的项目中都单独安装一次。把目录切换到你的项目文件夹中，然后在命令行中执行：
npm install gulp
3. 如果想在安装的时候把gulp写进项目package.json文件的依赖中，则可以加上--save-dev：
npm install --save-dev gulp
这样就完成了gulp的安装，接下来就可以在项目中应用gulp了。

##### 2.2 Gulp使用
1. 本地安装gulp 
进入项目根目录执行npm install gulp --save-dev（添加--save-dev会在package.json记录依赖关系）。
2. 任务清单
在项目根目录中创建gulpfile.js（这是一个配置文件）
3. 定义任务
在gulpfile.js定义构建任务，如压缩、合并，Gulp自身并不执行任何任务，是通过调用具体插件来完成的。
以编译LESS为例，安装npm install gulp-less，如下图定义任务
```lasso
//引入gulp
var gulp = require('gulp'),
//引入编译less的插件
less = require('gulp-less');
gulp.task('less',function(){
    return gulp.src('./public/less/*.less)
.pipe(less())
.pipe(gulp.dest('./public/css'));
});

```
4. 执行任务
1. 输入命令 gulp less
这样我们的LESS文件便会编译成CSS了。
##### 2.3 Gulp工作原理
通过不同的插件实现构建任务，Gulp只是按着配置文件调用执行了这些插件。

#### 3. Gulp API

##### 3.1 Gulp API简述
Gulp是基于NodeJS的，通过require可以引入一个NodeJS的包（模块），其作用类似于浏览器中的script标签引入资源，被引入的包存放在node_modules目录下。
引入gulp包（模块）后返回一个对象，习惯赋值给变量gulp，通过该对象提供的方法（API）完成任务的配置。
+ gulp.task() 定义各种不同的任务，如下图有两个参数
```lasso
gulp.task('css',function(){
	console.log('压缩css');
});
```
//不同任务间存在依赖关系时，可以指定依赖，如下图:

+ gulp.src() 需要构建资源的路径，字符串或数组（可以正则方式书写）
```lasso
gulp.task('css',function(){
	gulp.src('./css/main.css')
});

```
+ gulp.pipe() 管道，将需要构建的资源“输送”给插件。
```lasso
gulp.task('css',function(){
	gulp.src('./css/main.css')
	.pipe(cssmin())	
	.pipe(gulp.dest('./css/main.min.css'));	
	// cssmin相当于angular里的过滤器
	// pipe相当于angular里的管道符|
	// gulp是一个总指挥 不参与具体的事物 
	//通过调用gulp的插件（基于node.js）来实现
	// 插件需要通过命令行安装
});
```
+ gulp.watch() 
##### 3.2 常用Gulp插件
+ gulp-less 编译LESS文件
```lasso
//引入gulp-less插件
var less = require('gulp-less');
```
+ gulp-autoprefixer 添加CSS私有前缀
```lasso
//引入gulp-autoprefixer插件
var autoprefixer = require('gulp-autoprefixer');
gulp.task('css', function () {
	gulp.src('./css/main.css')
		.pipe(autoprefixer({
			// 浏览器往回倒2个版本
			browsers:['last 2 versions']
});
```
+ gulp-cssmin 压缩CSS
```lasso
//引入gulp-cssmin插件
var cssmin = require('gulp-cssmin');
gulp.task('css', function () {
	gulp.src('./css/main.css')
	// 通过pipe方法将上一次输入转成下一次输入
		.pipe(cssmin())
		// 通过dest方法存储
		.pipe(gulp.dest('./css/main.min.css'));
});

```
+ gulp-rname重命名
+ gulp-imagemin 图片压缩
+ gulp-uglify 压缩Javascript
+ gulp-concat 合并
+ gulp-htmlmin 压缩HTML
+ gulp-rev 添加版本号
+ gulp-rev-collector 内容替换
+ gulp-useref
+ gulp-if
```

