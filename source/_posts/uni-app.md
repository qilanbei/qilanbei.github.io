---
title: uni-app开发学习之路
date: 2020-10-16 15:54:46
description: uni-app
categories:
- uni-app
tags: 
- uni-app
toc: true 文章目录
---

+ uni-app入门学习
<!-- more -->
<The rest of contents | 余下全文>

## 什么是uni-app

uni-app是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉/淘宝）、快应用等多个平台。

uni-app在手，做啥都不愁。即使不跨端，uni-app也是更好的小程序开发框架（详见）、更好的App跨平台框架、更方便的H5开发框架。不管领导安排什么样的项目，你都可以快速交付，不需要转换开发思维、不需要更改开发习惯。

有关：[uni-app的由来](https://uniapp.dcloud.io/history)

uni-app: [如何学习](https://uniapp.dcloud.io/resource)
 
uni-app: [插件市场](https://ext.dcloud.net.cn/)

更多介绍请关注：[uni-app官网](https://uniapp.dcloud.io/README)

## 快速体验

代码示例学习参考：[Github示例代码](https://github.com/dcloudio/hello-uniapp)
一套代码编到10个平台，这不是梦想。眼见为实，扫描10个二维码，亲自体验最全面的跨平台效果！
![](/images/uni-app-demo.png)

## 框架简介

1. 先了解一下项目目录结构：

![](/images/demo-catalogue.png)

Tips:

- `static` 目录下的 `js` 文件不会被编译，如果里面有 `es6` 的代码，不经过转换直接运行，在手机设备上会报错。

- `css、less/scss` 等资源同样不要放在 `static` 目录下，建议这些公用的资源放在 `common` 目录下

2. 生---
    title: uni-app开发学习之路
    date: 2020-10-16 15:54:46
    description: uni-app
    categories:
    - uni-app
    tags: 
    - uni-app
    toc: true 文章目录
    ---
    
    + uni-app入门学习
    <!-- more -->
    <The rest of contents | 余下全文>
    
    ## 什么是uni-app
    
    uni-app是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉/淘宝）、快应用等多个平台。
    
    uni-app在手，做啥都不愁。即使不跨端，uni-app也是更好的小程序开发框架（详见）、更好的App跨平台框架、更方便的H5开发框架。不管领导安排什么样的项目，你都可以快速交付，不需要转换开发思维、不需要更改开发习惯。
    
    有关：[uni-app的由来](https://uniapp.dcloud.io/history)
    
    uni-app: [如何学习](https://uniapp.dcloud.io/resource)
     
    uni-app: [插件市场](https://ext.dcloud.net.cn/)
    
    更多介绍请关注：[uni-app官网](https://uniapp.dcloud.io/README)
    
    ## 快速体验
    
    代码示例学习参考：[Github示例代码](https://github.com/dcloudio/hello-uniapp)
    一套代码编到10个平台，这不是梦想。眼见为实，扫描10个二维码，亲自体验最全面的跨平台效果！
    ![](/images/uni-app-demo.png)
    
    ## 框架简介
    
    1. 先了解一下项目目录结构：
    
    ![](/images/demo-catalogue.png)
    
    Tips:
    
    - `static` 目录下的 `js` 文件不会被编译，如果里面有 `es6` 的代码，不经过转换直接运行，在手机设备上会报错。
    
    - `css、less/scss` 等资源同样不要放在 `static` 目录下，建议这些公用的资源放在 `common` 目录下
    
    2. 生命周期
    
    应用生命周期 => uni-app 支持如下应用生命周期函数：
    
    | 函数名 | 说明 |
    | ---- | ---- |
    | onLaunch | 当 `uni-app` 初始化完成时触发（全局只触发一次）|
    | onShow | 当 `uni-app` 启动，或从后台进入前台显示 |
    | onHide | 当 `uni-app` 从前台进入后台 |
    | onError | 当 `uni-app` 报错时触发 |
    | onUniNViewMessage | 对 `nvue` 页面发送的数据进行监听，可参考 nvue 向 vue 通讯 |
    | onUnhandledRejection | 对未处理的 Promise 拒绝事件监听函数（2.8.1+）|
    | onPageNotFound | 页面不存在监听函数 |
    | onThemeChange | 监听系统主题变化 |
    
    注意：
    
    - 应用生命周期仅可在App.vue中监听，在其它页面监听无效。
    - onlaunch里进行页面跳转，如遇白屏报错，请参考[](https://ask.dcloud.net.cn/article/35942)
    
    3. 页面生命周期
    uni-app 支持如下页面生命周期函数：
    
    | 函数名 | 说明 |
    | --- | ----- |
    | onLoad | 监听页面加载，其参数为上个页面传递的数据，参数类型为Object(用于页面传参) |
    | onShow | 监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面 |
    | onReady | 监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发 | 
    | onHide | 监听页面隐藏 |
    | onUnload | 监听页面卸载 |
    | onResize | 监听窗口尺寸变化 |
    | onPullDownRefresh | 监听用户下拉动作，一般用于下拉刷新 |
    | onReachBottom | 页面上拉触底事件的处理函数 |
    | onTabItemTap | 点击 tab 时触发，参数为Object，具体见下方注意事项 |
    | onShareAppMessage | 用户点击右上角分享 |
    | onPageScroll | 监听页面滚动，参数为Object |
    | onNavigationBarButtonTap | 监听原生标题栏按钮点击事件，参数为Object |
    | onBackPress | 监听页面返回，返回 event = {from:backbutton、 navigateBack} ，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是 uni.navigateBack |
    | onNavigationBarSearchInputChanged | 监听原生标题栏搜索输入框输入内容变化事件 |
    | onNavigationBarSearchInputConfirmed | 监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发 |
    | onNavigationBarSearchInputClicked | 监听原生标题栏搜索输入框点击事件 |
    | onShareTimeline | 监听用户点击右上角转发到朋友圈 |
    | onAddToFavorites | 监听用户点击右上角收藏 |
    
    ## 快速上手
    
    关于如何学习uni-app，可以参考官网[uni-app学习指引](https://uniapp.dcloud.io/resource)，里面还有官方视频教学，方便大家快速上手学习
    
    uni-app支持通过[可视化界面](https://uniapp.dcloud.io/quickstart?id=_1-%e9%80%9a%e8%bf%87-hbuilderx-%e5%8f%af%e8%a7%86%e5%8c%96%e7%95%8c%e9%9d%a2)、[vue-cli命令行](https://uniapp.dcloud.io/quickstart?id=_2-%E9%80%9A%E8%BF%87vue-cli%E5%91%BD%E4%BB%A4%E8%A1%8C) 两种方式快速创建项目。
    
    #### 通过 HBuilderX 可视化界面创建项目
    可视化的方式比较简单，HBuilderX内置相关环境，开箱即用，无需配置nodejs。
    
    开始之前，开发者需先下载安装 HBuilderX 工具：[HBuilderX 官方IDE下载地址](https://www.dcloud.io/hbuilderx.html)
    
    HBuilderX是通用的前端开发工具，但为uni-app做了特别强化，对[vue支持也很强大](https://ask.dcloud.net.cn/article/19601)。
    
    下载App开发版，可开箱即用；如下载标准版，在运行或发行uni-app时，会提示安装uni-app插件，插件下载完成后方可使用。
    如使用cli方式创建项目，可直接下载标准版，因为uni-app编译插件被安装到项目下了
    
    
    ##### 1. 创建项目：在点击工具栏里的文件 -> 新建 -> 项目
    
    这时会弹出一个新建项目的弹窗，如下图所示：
    ![](/images/uni-app-create.png)
    
    继续选择 `uni-app` 类型，输入项目名称，选择模板，点击创建，即可成功创建。
    
    uni-app 自带的模板有 Hello uni-app ，是官方的组件和API示例。还有一个重要模板是 `uni ui` 项目模板，日常开发推荐使用该模板，已内置大量常用组件。
    
    ##### 2. 运行uni-app项目
    
    进入hello-uniapp项目，点击工具栏的运行，如下图所示：
    
    ![](/images/uni-app-start.png)
    
    - 浏览器运行：点击工具栏的运行 -> 运行到浏览器 -> 选择浏览器，即可在浏览器里面体验uni-app 的 H5 版。
    
    - 真机运行：连接手机，开启USB调试，点击工具栏的运行 -> 真机运行 -> 选择运行的设备，即可在该设备里面体验uni-app。
    
    - 在微信开发者工具里运行：点击工具栏的运行 -> 运行到小程序模拟器 -> 微信开发者工具，即可在微信开发者工具里面体验uni-app。
    
    如果是第一次使用，需要先配置小程序ide的相关路径，才能运行成功。以微信开发者工具为例，如下图所示:
    
    ![](/images/ide-source.png)
    
    需在输入框输入微信开发者工具的安装路径：
    
    ![](/images/wechat-ide-source.png)
    
    若HBuilderX不能正常启动微信开发者工具，可以在微信开发者工具中菜单栏查看：设置->安全设置，如图所示：
    
    ![](/images/wx-safety-setting.png)
    
    查看微信开发者工具的服务端口是否打开，如果没有，将服务端口打开：
    
    ![](/images/wx-port-setting.png)
    
    如果HBuilderX仍然不能正常启动微信开发者工具，需要开发者手动启动，然后将uni-app生成小程序工程的路径拷贝到微信开发者工具里面，在HBuilderX里面开发，在微信开发者工具里面就可看到实时的效果
    
    Tips:
    - 如果是第一次使用，需要配置开发工具的相关路径。点击工具栏的运行 -> 运行到小程序模拟器 -> 运行设置，配置相应小程序开发者工具的路径。
    - 支付宝/百度/字节跳动/360小程序工具，不支持直接指定项目启动并运行。因此开发工具启动后，请将 HBuilderX 控制台中提示的项目路径，在相应小程序开发者工具中打开。
    - 如果自动启动小程序开发工具失败，请手动启动小程序开发工具并将 HBuilderX 控制台提示的项目路径，打开项目。
    
    ##### 3. 发布uni-app
    
    - 打包为原生App（云端）
    
    在HBuilderX工具栏，点击发行，选择原生app-云端打包，如下图：
    
    ![](/images/release-demo.png)
    
    - 打包为原生App（离线）
    
    `uni-app` 支持离线打包，在 HBuilderX 发行菜单里生成离线打包资源，然后参考离线打包文档操作，可以从HBuilderX的发行菜单里找到[文档链接](https://nativesupport.dcloud.net.cn/AppDocs/README)。
    
    在HBuilderX工具栏，点击发行，选择本地打包，如下图，点击即可生成本地打包APP资源：
    
    ![](/images/local-release.png)
    
    - 发布为H5
    
    在 `manifest.json` 的可视化界面，进行如下配置（发行在网站根目录可不配置应用基本路径），此时发行网站路径是 `www.xxx.com/h5`
    
    ![](/images/h5-release.png)
    
    在HBuilderX工具栏，点击发行，选择网站-H5手机版，点击即可生成 H5 的相关资源文件，保存于 unpackage 目录。
    
    - 发布为小程序
    
    申请微信小程序AppID，参考：[微信教程](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/getstart.html#%E7%94%B3%E8%AF%B7%E5%B8%90%E5%8F%B7)
    在HBuilderX中顶部菜单依次点击 "发行" => "小程序-微信"，输入小程序名称和appid点击发行即可在 `unpackage/dist/build/mp-weixin` 生成微信小程序项目代码。
    
    ![](/images/wx-release.png)
    
    在微信小程序开发者工具中，导入生成的微信小程序项目，测试项目代码运行正常后，点击“上传”按钮，之后按照 “提交审核” => “发布” 小程序标准流程，逐步操作即可，详细查看：[微信官方教程](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/release.html#%E5%8F%91%E5%B8%83%E4%B8%8A%E7%BA%BF)
    
    其他小程序的发布请参见[官网教程](https://uniapp.dcloud.io/quickstart?id=%e5%8f%91%e5%b8%83uni-app)
    
    #### 通过vue-cli命令行
    
    除了HBuilderX可视化界面，也可以使用 `cli` 脚手架，可以通过 `vue-cli` 创建 `uni-app` 项目。
    
    环境安装 - 全局安装vue-cli
    
    ```
    npm install -g @vue/cli
    ```
    创建uni-app
    
    ```
    使用正式版（对应HBuilderX最新正式版）:
    vue create -p dcloudio/uni-preset-vue my-project
    
    使用alpha版（对应HBuilderX最新alpha版）:
    vue create -p dcloudio/uni-preset-vue#alpha my-alpha-project
    ```
    
    此时，会提示选择项目模板，初次体验建议选择 hello uni-app 项目模板，如下所示：
    
    ![](/images/template-demo.png)
    
    运行、发布uni-app
    
    ```
    npm run dev:%PLATFORM%
    npm run build:%PLATFORM%
    ```
    
    更多平台的运行和发布请参见[官网教程](https://uniapp.dcloud.io/quickstart?id=quickapp)
命周期

应用生命周期 => uni-app 支持如下应用生命周期函数：

| 函数名 | 说明 |
| ---- | ---- |
| onLaunch | 当 `uni-app` 初始化完成时触发（全局只触发一次）|
| onShow | 当 `uni-app` 启动，或从后台进入前台显示 |
| onHide | 当 `uni-app` 从前台进入后台 |
| onError | 当 `uni-app` 报错时触发 |
| onUniNViewMessage | 对 `nvue` 页面发送的数据进行监听，可参考 nvue 向 vue 通讯 |
| onUnhandledRejection | 对未处理的 Promise 拒绝事件监听函数（2.8.1+）|
| onPageNotFound | 页面不存在监听函数 |
| onThemeChange | 监听系统主题变化 |

注意：

- 应用生命周期仅可在App.vue中监听，在其它页面监听无效。
- onlaunch里进行页面跳转，如遇白屏报错，请参考[](https://ask.dcloud.net.cn/article/35942)

3. 页面生命周期
uni-app 支持如下页面生命周期函数：

| 函数名 | 说明 |
| --- | ----- |
| onLoad | 监听页面加载，其参数为上个页面传递的数据，参数类型为Object(用于页面传参) |
| onShow | 监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面 |
| onReady | 监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发 | 
| onHide | 监听页面隐藏 |
| onUnload | 监听页面卸载 |
| onResize | 监听窗口尺寸变化 |
| onPullDownRefresh | 监听用户下拉动作，一般用于下拉刷新 |
| onReachBottom | 页面上拉触底事件的处理函数 |
| onTabItemTap | 点击 tab 时触发，参数为Object，具体见下方注意事项 |
| onShareAppMessage | 用户点击右上角分享 |
| onPageScroll | 监听页面滚动，参数为Object |
| onNavigationBarButtonTap | 监听原生标题栏按钮点击事件，参数为Object |
| onBackPress | 监听页面返回，返回 event = {from:backbutton、 navigateBack} ，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是 uni.navigateBack |
| onNavigationBarSearchInputChanged | 监听原生标题栏搜索输入框输入内容变化事件 |
| onNavigationBarSearchInputConfirmed | 监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发 |
| onNavigationBarSearchInputClicked | 监听原生标题栏搜索输入框点击事件 |
| onShareTimeline | 监听用户点击右上角转发到朋友圈 |
| onAddToFavorites | 监听用户点击右上角收藏 |

## 快速上手

关于如何学习uni-app，可以参考官网[uni-app学习指引](https://uniapp.dcloud.io/resource)，里面还有官方视频教学，方便大家快速上手学习

uni-app支持通过[可视化界面](https://uniapp.dcloud.io/quickstart?id=_1-%e9%80%9a%e8%bf%87-hbuilderx-%e5%8f%af%e8%a7%86%e5%8c%96%e7%95%8c%e9%9d%a2)、[vue-cli命令行](https://uniapp.dcloud.io/quickstart?id=_2-%E9%80%9A%E8%BF%87vue-cli%E5%91%BD%E4%BB%A4%E8%A1%8C) 两种方式快速创建项目。

#### 通过 HBuilderX 可视化界面创建项目
可视化的方式比较简单，HBuilderX内置相关环境，开箱即用，无需配置nodejs。

开始之前，开发者需先下载安装 HBuilderX 工具：[HBuilderX 官方IDE下载地址](https://www.dcloud.io/hbuilderx.html)

HBuilderX是通用的前端开发工具，但为uni-app做了特别强化，对[vue支持也很强大](https://ask.dcloud.net.cn/article/19601)。

下载App开发版，可开箱即用；如下载标准版，在运行或发行uni-app时，会提示安装uni-app插件，插件下载完成后方可使用。
如使用cli方式创建项目，可直接下载标准版，因为uni-app编译插件被安装到项目下了


##### 1. 创建项目：在点击工具栏里的文件 -> 新建 -> 项目

这时会弹出一个新建项目的弹窗，如下图所示：
![](/images/uni-app-create.png)

继续选择 `uni-app` 类型，输入项目名称，选择模板，点击创建，即可成功创建。

uni-app 自带的模板有 Hello uni-app ，是官方的组件和API示例。还有一个重要模板是 `uni ui` 项目模板，日常开发推荐使用该模板，已内置大量常用组件。

##### 2. 运行uni-app项目

进入hello-uniapp项目，点击工具栏的运行，如下图所示：

![](/images/uni-app-start.png)

- 浏览器运行：点击工具栏的运行 -> 运行到浏览器 -> 选择浏览器，即可在浏览器里面体验uni-app 的 H5 版。

- 真机运行：连接手机，开启USB调试，点击工具栏的运行 -> 真机运行 -> 选择运行的设备，即可在该设备里面体验uni-app。

- 在微信开发者工具里运行：点击工具栏的运行 -> 运行到小程序模拟器 -> 微信开发者工具，即可在微信开发者工具里面体验uni-app。

如果是第一次使用，需要先配置小程序ide的相关路径，才能运行成功。以微信开发者工具为例，如下图所示:

![](/images/ide-source.png)

需在输入框输入微信开发者工具的安装路径：

![](/images/wechat-ide-source.png)

若HBuilderX不能正常启动微信开发者工具，可以在微信开发者工具中菜单栏查看：设置->安全设置，如图所示：

![](/images/wx-safety-setting.png)

查看微信开发者工具的服务端口是否打开，如果没有，将服务端口打开：

![](/images/wx-port-setting.png)

如果HBuilderX仍然不能正常启动微信开发者工具，需要开发者手动启动，然后将uni-app生成小程序工程的路径拷贝到微信开发者工具里面，在HBuilderX里面开发，在微信开发者工具里面就可看到实时的效果

Tips:
- 如果是第一次使用，需要配置开发工具的相关路径。点击工具栏的运行 -> 运行到小程序模拟器 -> 运行设置，配置相应小程序开发者工具的路径。
- 支付宝/百度/字节跳动/360小程序工具，不支持直接指定项目启动并运行。因此开发工具启动后，请将 HBuilderX 控制台中提示的项目路径，在相应小程序开发者工具中打开。
- 如果自动启动小程序开发工具失败，请手动启动小程序开发工具并将 HBuilderX 控制台提示的项目路径，打开项目。

##### 3. 发布uni-app

- 打包为原生App（云端）

在HBuilderX工具栏，点击发行，选择原生app-云端打包，如下图：

![](/images/release-demo.png)

- 打包为原生App（离线）

`uni-app` 支持离线打包，在 HBuilderX 发行菜单里生成离线打包资源，然后参考离线打包文档操作，可以从HBuilderX的发行菜单里找到[文档链接](https://nativesupport.dcloud.net.cn/AppDocs/README)。

在HBuilderX工具栏，点击发行，选择本地打包，如下图，点击即可生成本地打包APP资源：

![](/images/local-release.png)

- 发布为H5

在 `manifest.json` 的可视化界面，进行如下配置（发行在网站根目录可不配置应用基本路径），此时发行网站路径是 `www.xxx.com/h5`

![](/images/h5-release.png)

在HBuilderX工具栏，点击发行，选择网站-H5手机版，点击即可生成 H5 的相关资源文件，保存于 unpackage 目录。

- 发布为小程序

申请微信小程序AppID，参考：[微信教程](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/getstart.html#%E7%94%B3%E8%AF%B7%E5%B8%90%E5%8F%B7)
在HBuilderX中顶部菜单依次点击 "发行" => "小程序-微信"，输入小程序名称和appid点击发行即可在 `unpackage/dist/build/mp-weixin` 生成微信小程序项目代码。

![](/images/wx-release.png)

在微信小程序开发者工具中，导入生成的微信小程序项目，测试项目代码运行正常后，点击“上传”按钮，之后按照 “提交审核” => “发布” 小程序标准流程，逐步操作即可，详细查看：[微信官方教程](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/release.html#%E5%8F%91%E5%B8%83%E4%B8%8A%E7%BA%BF)

其他小程序的发布请参见[官网教程](https://uniapp.dcloud.io/quickstart?id=%e5%8f%91%e5%b8%83uni-app)

#### 通过vue-cli命令行

除了HBuilderX可视化界面，也可以使用 `cli` 脚手架，可以通过 `vue-cli` 创建 `uni-app` 项目。

环境安装 - 全局安装vue-cli

```
npm install -g @vue/cli
```
创建uni-app

```
使用正式版（对应HBuilderX最新正式版）:
vue create -p dcloudio/uni-preset-vue my-project

使用alpha版（对应HBuilderX最新alpha版）:
vue create -p dcloudio/uni-preset-vue#alpha my-alpha-project
```

此时，会提示选择项目模板，初次体验建议选择 hello uni-app 项目模板，如下所示：

![](/images/template-demo.png)

运行、发布uni-app

```
npm run dev:%PLATFORM%
npm run build:%PLATFORM%
```

更多平台的运行和发布请参见[官网教程](https://uniapp.dcloud.io/quickstart?id=quickapp)
