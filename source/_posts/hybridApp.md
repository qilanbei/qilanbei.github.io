---
title: 混合App开发介绍
date: 2016-11-26 23:24:34
description: 混合App开发介绍
categories:
- 混合App开发
tags: 
- 混合App开发
toc: true 文章目录
---
+ 混合App开发介绍
<!-- more -->
<The rest of contents | 余下全文>


#### 一、混合开发介绍
###### 1. App开发的三种方式
WEB App（HTML5 APP 框架开发模式）基于Web的系统和应用，其作用是向广大的最终用户发布一组复杂的内容和功能。
Hybrid App（混合模式移动应用）是指介于web-app、native-app这两者之间的app，兼具“Native App良好用户交互体验的优势”和“Web App跨平台开发的优势”。
Native App：该开发针对IOS、Android等不同的手机操作系统要采用不同的原生语言和框架进行开发。
三种方式的比较：

|介绍|Web App(网页应用)| Hybrid App(混合应用) | Native App(原生应用)|
| :-------- | --------:| :------: |
|开发成本|低|中|高|
|维护更新|简单|简单|复杂|
|体验|差|中|优|
|Store/market认可|不认可|认可|认可|
|安装|不需要|需要|需要|
|跨平台|优|优|差|

2. 企业如何选择App开发方式
移动端开发不能确定哪一种是最佳的开发方式，因为不存在最佳的开发方式，每种方式都有天生的优点和局限性，找到最适合本企业需求的一种开发方式是关键。
过度依赖Hybrid方案会造成Web前端开发成本快速上升，甚至造成 App整体体验下降，甚至造成功能缺失。不要为了Hybrid而Hybrid，控制好方案中Native与Web的边界。
3. Html5+介绍
简单了解基于HTML5的app开发
HTML5中国产业联盟，简称“HTML5+联盟”，是为了更好的推进HTML5的商用、更好的为HTML5开发者服务而由产 业链主流厂商共同组成的一个联盟。
联盟致力于整合产业链资源，建立围绕HTML5开发者的生态系统，通过产业链共同为HTMLl5开发者服务。
W3C中国是联盟的指导单位。
CSDN 和 DCloud 是联盟的秘书处单位。
DCloud官网：enter link description here
HBuilder-HTML5 IDE编程
是一款web开发工具。它能大幅提升开发效率，对程序员也设计了更人文关怀的UI，它包括最全面的语法库和浏览器兼容性数据。到现在 已经有两年的时间了，到现在大概有几十万的开发者在使用
5+Runtime-HTML5强化引擎能力，性能接近原生
是一个H5的增强引擎。这是一个可以去调用H5的能力和原生操作系统的产品。
MUI-HTML5前端框架轻巧、漂亮、开源
是高性能的前端框架，可用开发高性能App，也是目前最接近原生App效果的框架，可以有效解决HTML5原生开发中遇到的部分问题，同 时体积也小
流应用-HTML5发行新方式秒装，秒更新
是高性能的前端框架，可用开发高性能App，也是目前最接近原生App效果的框架，可以有效解决HTML5原生开发中遇到的部分问题，同时体积也小
Html5+开发的优缺点
提供了一个自定制的开发IDE，支持良好，甚至不需要安装，解压后就可以用。
开发出来的app性能和用户体验很高。
丰富API支持，号称40万+
Html5+缺点
庞大的文档，但并不容易理解，甚至让你不知道如何上手。
复杂的打包过程。
复杂的MUI框架，并不会让你少写代码，甚至更多。
4. React Native介绍
React Native简介
React Native使你能够在Javascript和React的基础上获得完全一致的开发体验，构建世界一流的原生APP。
React Native着力于提高多平台开发的开发效率 ——仅需学习一次，编写任何平台。(Learn once, write anywhere)
Facebook已经在多项产品中使用了React Native，并且将持续地投入建设React Native。
React Native核心实现
React Native里面没有webview，这货不是Hybrid app，里面执行JS是用的 JavascriptCore。
再说React Native的核心，iOS Native code提供了十来个最基本核心的类（RCTDeviceEventEmitter、RCTRenderingPerf等）、或组件（RCTView、RCTTextField、RCTTextView、RCTModalFullscreenView等），然后由React Native的JS部分，组成二十来个基本组件（Popover、Listview等），交由上层的业务方来使用（THGroupView）。
就如他们在宣传时所说，他们实现了一套类似css的子集，用来解决样式问题，相当复杂和强大，靠这个才能将Native的核心组件组成JS层的基本组件再组成业务端的业务组件，应该是采用facebook/css-layout · GitHub的C语言版本实现的（在ppt中我们看到了类似flex-direction: column一类的代码，这个正是css-layout支持的语法）。
在React Native中，写JS的工程师解决的是「将基本组件拼装成可用的React组件」的问题，写Native Code的工程师解决的是「提供核心组件，提供足够的扩展性、灵活性和性能」的问题。
React Native优点
不用Webview，彻底摆脱了Webview让人不爽的交互和性能问题。
有较强的扩展性，这是因为Native端提供的是基本控件，JS可以自由组合使用。
可以直接使用Native原生的「牛逼」动画。
可以通过更新远端JS，直接更新app。
5. 其他开发方法
5.1 Jquery Mobile简介
Jquery Mobile简介
jQuery Mobile 是一个为触控优化的框架，用于创建移动 web 应用程序。
jQuery 适用于所有流行的智能手机和平板电脑
jQuery Mobile 构建于 jQuery 库之上，这使其更易学习，如果您通晓 jQuery 的话。
它使用 HTML5、CSS3、JavaScript 和 AJAX 通过尽可能少的代码来完成对页面的布局。
为什么使用Jquery Mobile
jQuery Mobile 将“写得更少、做得更多”这一理念提升到了新的层次：它会自动为网页设计交互的易用外观，并在所有移动设计上保持一致。
您不需要为每种移动设备或 OS 编写一个应用程序：jQuery Mobile 解决了这个问题，因为它只用 HTML、CSS 和 JavaScript，这些技术都是所有移动 web 浏览器的标准。
5.2 App Can简介
App Can简介
AppCan.cn开发平台是基于HTML5技术的跨平台移动应用快速开发一体化解决方案。开发者利用HTML5+CSS3+JavaScript技术可 以快速地开发与本地应用体验相媲美的移动应用。AppCan.cn平台提供了UI快速开发框架、本地功能调用API接口、应用打包系统、IDE集成开发环 境和本地应用调试模拟器，预置数百套界面模板和数十种应用插件，提供多套应用模板。完善的框架接口，人性化的开发环境，丰富的开发资源，强大的服务支持， 使开发者可以快速迈入移动开发领域。
App Can特色
支持Hybrid混合应用开发模式，集合WebApp和NativeApp两者优势。
提供丰富的插件调用功能,插件仓库包含系统类、UI界面类、网络类、多媒体类、第三方扩展类。
自定义Native插件扩展机制，以满足开发者的定制需求，也可以提交至插件仓库给大众分享。
多窗口机制，始终贯穿应用实现中，实现页面交互的极致体验。
推送消息，为开发者提供向指定群组或指定用户推送即时消息的服务，针对不同属性的用户推送差异化信息。
统计分析，实时的运营数据统计及分析。也可以实现自定义事件统计，无限扩展分析维度。
5.3 Cordova介绍
Cordova是什么
Cordova提供了一组设备相关的API，通过这组API，移动应用能够以JavaScript访问原生的设备功能，如摄像头、麦克风等。
Cordova支持如下移动操作系统：iOS, Android,ubuntu phone os, Blackberry, Windows Phone, Palm WebOS, Bada 和 Symbian。
和Phonegap的关系？
Cordova是贡献给Apache后的开源项目，是从PhoneGap中抽出的核心代码，目前(PhoneGap和Apache Cordova之间的)唯一区别是下载的包的名字，这会持续一段时间。
5.4 Ng-Cordova介绍
ngCordova是在Cordova Api基础上封装的一系列开源的AngularJs服务和扩展，让开发者可以方便的在HybridApp开发中调用设备能力，即可以在AngularJs代码中访问设备能力Api。
在cordova插件的sucess和error js回调方法中，是无法使用 angularjs的$scope对象和注入的方法的，只能访问全局的方法和变量，这样会导致很多麻烦，必须使用传统的js方法写很多难看的代码。使用 ngCordova应该可以解决这个问题。
5.5 ES6简介
1996年11月，JavaScript的创造者Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，希望这种语言能够成 为国际标准。次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript。这个版本就是ECMAScript 1.0版。

ES6是什么
ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现。在日常场合，这两个词是可以互换的。
2015年6月，ECMAScript 6正式通过，成为国际标准。
Node.js是JavaScript语言的服务器运行环境。它们对ES6的支持度，比浏览器更高。通过它们，可以体验更多ES6的特性。
这个标准的牛逼之处就在于会逐步统一前端，因为新增加的module,异步编程，Generator函数这些东西在angular中和node中都有很好的实现了。而他们又是按照ECMAScript5规范写的。
ECMAScript6学习网址 enter link description here
5.6 Angular简介
Angular中文网 enter link description here
Angular官网 enter link description here

AngularJS 诞生于2009年，由Misko Hevery 等人创建，后为Google所收购。是一款优秀的前端JS框架，已经被用于Google的多款产品当中。AngularJS有着诸多特性，最为核心的是：MVVM、模块化、自动化双向数据绑定、语义化标签、依赖注入等等。
Angular优点
模板功能强大丰富，并且是声明式的，自带了丰富Angular指令
是一个比较完善的前端MV*（MVW:Model-View-Whatever）框架，包含模板，数据双向绑定，路由，模块化，服务，过滤器，依赖注入等所有功能
自定义Directive，比jQuery插件还灵活，但是需要深入了解Directive的一些特性，简单的封装容易，复杂一点官方没有提供详细的介绍文档，我们可以通过阅读源代码来找到某些我们需要的东西，如：在directive使用 $parse。
ng模块化比较大胆的引入了依赖注入，能够很容易的写出可复用的代码，对于敏捷开发的团队来说非常有帮助，我们的项目从上线到目前，UI变化很大，在摸索中迭代产品，但是js的代码基本上很少改动。
Angular支持单元测试和e2e-testing。
Angular应用场景
单页面应用程序
Angular更适合于CRUD的管理系统开发。
也非常适合模块化，分层化，数据绑定
hybrid开发神器
Angular不适合做
内容网站，需要SEO的。(SEO目前也有了prerender解决方案)
交互频繁的，如游戏之类交互体验网站。（单页面应用程序）
太过于简单的页面。（因为要考虑mvc,注入等，就会笨重
5.7 Ionic介绍
IONIC 是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。 它使用 JavaScript MVVM 框架和 AngularJS 来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。Ionic是一个专注于用WEB开发技术，基于HTML5创建类似于手机平台原生应用的一个开发框架。Ionic框架的目的是从 web的角度开发手机应用，基于PhoneGap的编译平台，可以实现编译成各个平台的应用程序。

Ionic框架构成
CSS框架 - 提供原生 App 质感的 CSS 样式模拟。 ionic 这部分的实现使用了 ionicons 图标样式库。
JavaScript框架 - 提供移动 Web 应用开发框架。ionic 基于 AngularJS 基础框架开发，遵循 AngularJS 的框架约束；此外 ionic 使用 AngularJS UI Router 实现前端路由。
命令行/CLI - 命令行工具集用来简化应用的开发、构造和仿真运行。ionic 命令行工具使用了 Cordova，依赖于平台 SDK（Android & iOS）实现将移动 Web 项目打包成原生 App。
Ionic框架特点
基于Angular语法，简单易学。
是一个轻量级框架。性能优越，运行速度快。
完美的融合下一代移动框架，支持 Angularjs 的特性， MVC ，代码易维护。
提供了漂亮的设计，通过 SASS 构建应用程序，它提供了很多UI组件来帮助开发者开发强大的应用。
专注原生，让你看不出混合应用和原生的区别
提供了强大的命令行工具。
Ionic Tips
由于 ionic 使用了 HTML5 和 CSS3 的一些新规范，所以要求 iOS7+ / Android4.1+。
在低于这些版本的手机上使用 ionic 开发的应用，有时会发生莫名其妙的问题。
使用IONIC制作APP
4.1 环境搭建
4.1.1 系统环境的搭建
Android
Java jdk（android ADT需要）
C++环境（Node需要）
Android ADT（打包android应用的时候需要）
Node（插件环境需要）
Git（从github上下载模板，团队代码管理）
IOS
XCode（打包ios应用的时候需要）
Node（插件环境需要）
Git（从github上下载模板，团队代码管理）
-4.1.2 项目依赖环境搭建
cordova（打包工具）
npm install -g cordova
ionic（框架）
npm install -g ionic
4.2 项目搭建
4.2.1 创建项目模板
Ionic start myApp(的方式)
Ionic start myApp blank
Ionic start myApp tabs
Ionic start myApp sidemenu
4.2.2 模拟器运行
android
ionic emulate android
ios
ionic emulate ios
4.2.3 打包app
添加项目平台
android – ionic platform add android
ios – ionic platform add ios
打包
android – ionic build android
ios – ionic build ios
直接运行在手机
ionic run android
6. 流行开发平台介绍
Angular 2.0介绍
性能的限制
AngularJS当初是提供给设计人员用来快速构建HTML表单的一个内部工具。随着时间的推移，各种特性 被加入进去以适应不同场景下的应用开发。然而由于最初的架构限制（比如绑定和模板机制），性能的 提升已经非常困难了。
快速变化的WEB
在语言方面，ECMAScript6的标准已经完成，这意味着浏览器将很快支持例如模块、类、lambda表达式、generator等新的特性，而这些特性将显著地改变JavaScript的开发体验。
在开发模式方面，Web组件也将很快实现。然而现有的框架，包括Angular1.x对WEB组件的支持都不够好。
简单易用
说实话，Angular1.x太复杂了，学习曲线太陡峭了，这让人望而生畏。Angular团队希望在Angular2中将复杂性 封装地更好一些，让暴露出来的概念和开发接口更简单。
移动化
想想5年前……现在的计算模式已经发生了显著地变化，到处都是手机和平板。Angular1.x没有针对移动 应用特别优化，并且缺少一些关键的特性，比如：缓存预编译的视图、触控支持等。
Angular2新特性
移动：新的版本将专注于移动应用的开发。依据是它更容易处理桌面方面的事情，一旦挑战涉及到移动（性能、加载时间），注重这方面会使问题得到解决。
模块化：各个模块将从Angular的核心中移除，从而获得更好的性能。这意味着你可以选择你需要的零件。
现代化：Angular 2.0将把ES6和“常青”现代浏览器（自动更新到最新版本）作为目标。这意味着开发者可以专注于业务领域相关的代码。
