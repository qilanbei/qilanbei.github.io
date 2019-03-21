---
title: NodeJS学习
date: 2016-05-06 10:33:20
description: 关于NodeJS的学习笔记
categories:
- NodeJS
tags: 
- NodeJS学习
toc: true 文章目录
---

+ 关于NodeJS的学习笔记
<!-- more -->
<The rest of contents | 余下全文>
### 1. Node.js概述
##### 1.1 Node.js是一个在浏览器之外可以解析和执行JavaScript代码的运行时环境，或者说是一个运行时平台;基于V8引擎，事件驱动，非阻塞IO模型，高效轻便
##### 1.2 Node.js的特性
* 无阻塞IO模型
* 事件驱动

##### 1.3 Node.js通常用来构建提供实时服务的应用程序
##### 1.4 Node.js的好处
* 可以在服务器端使用JavaScript了
* 统一了开发环境和语言，JavaScript无处不在
* 高性能的JavaScript引擎 – Google V8
* 诞生于2009年，由Ryan Dasl 发布，并且是开源的
* Node.js非常轻量
* Node.js同时支持Windows、Linux、Mac OSX  跨平台

##### 1.5 Node.js的用途
* 多人游戏、实时系统、联网软件和具有上千个并发用户的应用程序
* 实时多人游戏后台服务器
* 基于Web的聊天客户端
* 单页面浏览器应用程序
* 基于JSON的API 
* 不适合CPU密集型应用

##### 1.5.1 Node能做什么
* 操作文件
* 操作数据库
* 接收请求
* 操作系统底层
* 数据持久化（连接数据库）

##### 1.5.2 Node与PHP的区别
*类似java/c++/php  java-->服务器应用程序 web开发中 要接收请求 还需要服务器来运行java代码 IIS apache
* node.js不需要服务器 本身这个平台就是服务器

##### 1.5.3 谁在使用Node
* 国外：Uber/linked in 把消息队列从ruby语言切换到了node
* 国内：BAT 天猫 --> 2015年双11天猫将原来的java部分，切换成了node  970亿
    + PV page view 日访问量
* 处理高频率的访问


##### 1.5.4 node.js适合开发什么
node.js适合开发IO密集（基于网络数据的输入输出）
node.js不适合开发cpu密集

##### 1.6 Node.js基于哪个JavaScript引擎
Chrome 的 V8引擎
### 2. 安装与配置Node.js环境
##### 2.1 版本管理工具nvm
* 项目地址：https://github.com/creationix/nvm
* 直接输入nvm查看nvm的常用命令以及作用
* nvm的一些常用命令：
    - 安装指定版本 node	nvm install 版本号 [arch]
    - 卸载指定版本 node	nvm uninstall 版本号
    - 切换使用指定版本的 node	nvm use 版本号 [arch]
    - 查看本地安装的所有版本	nvm list|ls

##### 2.2 path环境变量
* 当要求系统运行一个程序而没有告诉它程序所在的完整路径时
    - 系统首先在当前目录下面寻找该程序
    - 如果找不到，则系统会跑到path中指定的路径去找，如果找到，直接运行
    - 如果最终path环境变量中也没有找到，则直接提示不是内部或外部命令，也不是可运行的程序
* path环境的添加的两种方式
    - 直接在path的变量值中以分好分隔加入程序所在的目录;（系统变量或者用户变量）path变量中，添加一个值，node js的目录 找到对应的node.exe
    - 也可以在外部先定义一个变量，然后在path以%变量名%的方式添加变量;定义一个系统变量NODE_PATH值：（最后一个）nodejs版本的路径 添加到path变量中%NODE_PATH%

##### 2.3 cmd
* cmd：command 命令行程序，允许用户可以在终端命令台中与操作系统交互，其实就是输出输出
* 作用：输入一些命令，cmd.exe可以执行，
* 在cmd中操作文件目录
    - cd（change directory）      切换目录
    - mkdir/md（make directory）  创建一个文件夹
    - rd（remove directory）	  删除文件夹
    - del（delete）		        删除指定文件
    - dir	列出当前目录中所有的内容
    - ren（rename）	改变文件名
##### 真伪全局的区别
哪里都能用 
真全局对象在程序运行的时候 所有模块共享一个值
伪全局对象在运行的时候 所有模块各有各自的值 
##### nvm的使用
nvm version （直接nvm给出对应的命令）
nvm ls 查看版本信息
nvm install 4.5[.0] [32||64]安装对应版本
nvm uninstall 4.5.0 卸载版本
nvm use 4.5.0 切换版本
node 版本切换  新老版本的维护开发 需求切换版本
绿色版：环境变量 -->安装新版本的时候，要更改原来的目录名称 使其不被删除
##### 环境变量配置
1. （系统变量或者用户变量）path变量中，添加一个值，node js的目录 找到对应的node.exe
2. 定义一个系统变量NODE_PATH值：（最后一个）nodejs版本的路径 添加到path变量中%NODE_PATH%
##### nodejs和javascript
javascript 在浏览器中和nodejs平台的区别
架构上nodejs中没有DOM和BOM  -->window和document 

