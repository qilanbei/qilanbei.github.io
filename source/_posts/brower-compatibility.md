---
title: 浏览器兼容性
date: 2016-08-20 01:56:14
description: 浏览器兼容性
categories:
- 浏览器工作相关
tags: 
- 浏览器兼容性
toc: true 文章目录
---

+ 浏览器兼容性(必须要总结一下这个了)
<!-- more -->
<The rest of contents | 余下全文>


1. 获取document对象的文本内容：
大部分浏览器都支持：innerhtml
IE支持：innerhtml和innerText
firefox支持：innerhtml和textContent
2. 事件event对象相关兼容性问题：
1>获取event事件对象
chrome，firefox，safari等直接直接通过event获取，但IE采用了一种非标准的方式，将事件作为window对象的event属性；
IE是把event事件对象作为全局对象window的一个属性；可以使用event或window.event来访问；
FireFox和Chrome等主流浏览器是通过把【事件对象】作为【事件响应函数】的【参数】进入传入的；
jquery的event一般不用考虑兼容性了
```
//原生js获取事件对象兼容性写法:
function foo(event){
    var evt = event || window.event;
}
```
2>获取事件源对象的方式也不一样：
在IE中：window.event.srcElement；
在firefox中：window.event.target；
3>event.x和event.y的兼容性
x，y事件发生的位置的 x 坐标和 y 坐标
offsetX,offsetY发生事件的地点在事件源元素的坐标系统中的 x 坐标和 y 坐标
pageX,pageY返回鼠标指针的位置，相对于文档的左边缘
IE取鼠标点击绝对位置使用event.x,event.y;
IE取鼠标点击相对位置使用event.offsetX,event.offsetY
firefox取鼠标指针位置使用event.pageX,event.pageY
3. const兼容问题
const是es6新增的用来定义常量的关键字，IE9一下不支持
解决方法：通过npm下载babel来兼容，
```
//引入babel文件
 <script src="browser.min.js"></script>
    <script type="text/babel">
        const Name = 'niwei';//使用新增的关键字：const声明常量
        console.log(Name);
    </script>
```
