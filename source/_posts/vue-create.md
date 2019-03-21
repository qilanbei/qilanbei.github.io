---
title: 如何搭建vue项目
date: 2016-08-20 01:56:14
description: 如何搭建vue项目
categories:
- Vue
tags: 
- 如何搭建vue项目
toc: true 文章目录
---

+ 如何搭建vue项目
<!-- more -->
<The rest of contents | 余下全文>

1. 开发环境要求
1> Node.js: javascript运行环境，不同系统不能直接运行各种编程语言;
2> npm: Node.js下的包管理器;
3> webpack: .vue的Vue组件 或者像 微信小程序的.wxml和.wxss等自定义的组件都无法被用户端的各种浏览器解析，需要被翻译和打包成.js文件;
4> vue-cli: Vue的脚手架工具，主要作用：目录结构、本地调试、代码部署、热加载、单元测试，相当于按照设计好的图来盖房子。
2. 安装环境
1> 安装nodejs
Node.js安装包及源码下载地址为：https://nodejs.org/en/download/
Windows 安装包(.msi): 
32 位安装包下载地址 : https://nodejs.org/dist/v4.4.3/node-v4.4.3-x86.msi
64 位安装包下载地址 : https://nodejs.org/dist/v4.4.3/node-v4.4.3-x64.msi
按照提示安装即可。node安装完成之后，自带npm
★ 如果npm下载的慢，可以使用淘宝镜像：
npm install -g cnpm --registry=https://registry.npm.taobao.org
2> vue的安装依赖于node.js，所以要先安装node.js再安装vue，此时npm 版本需要大于 3.0，如果低于此版本需要升级它 ，升级 npm: cnpm install npm -g；
安装全局vue-cli脚手架,用于帮助搭建所需的模板框架：
cnpm install -g vue-cli；
或者在下载vue-cli的后面加上淘宝镜像： 
npm install -g vue-cli --registry=https://registry.npm.taobao.org；
检查是否安装成功，在终端执行：vue；回车，如果出现vue信息说明安装成功，或者输入vue --version，返回版本号说明安装成功。
3> 此时环境已经搭建好了，下面用vue-cli构建项目
在终端执行命令：
// 创建一个基于webpack模板的新项目：
vue init webpack my-project
//命令执行后，会弹出以下配置信息：
vue init webpack my-project     //回车 

? Project name my-project
//输入项目名字
? Project description study vue
//输入项目描述
? Author niwei
//作者
? Vue build standalone

? Install vue-router? Yes
//是否安装vue-router，对于单页应用,官方提供了vue-router进行路由跳转的处理
? Use ESLint to lint your code? Yes
//是否进行eslint代码检测，ESLint是一个用来识别 ECMAScript 并且按照规则给出报告的代码检测工具
? Pick an ESLint preset Standard
//选择一个ESLint预设的标准
? Setup unit tests with Karma + Mocha? Yes
//是否单元测试
? Setup e2e tests with Nightwatch? Yes
// 是否端到端的测试

   vue-cli · Generated "my-project".

   To get started:

     cd my-project
     npm install
     npm run dev

   Documentation can be found at https://vuejs-templates.github.io/webpack
 // 进入到项目文件：cd my-project  （和package.json文件同路径）
  // 根据package.json文件安装相应依赖： cnpm install
 回车，发现文件下多了一个node_modules文件夹（里面就是安装的各种依赖）
3. 检测环境是否搭建成功
1> 在终端执行： npm run dev
4. 项目文件目录解析
![这里写图片描述](http://img.blog.csdn.net/20171006214745442?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlsYW5iZWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
