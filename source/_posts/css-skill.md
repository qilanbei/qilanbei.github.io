---
title: 你应该知道的css细节及技巧
date: 2016-04-26 00:41:29
categories:
- CSS相关
tags: 
- 你应该知道的css细节及技巧
toc: true 文章目录
---
+ 你应该知道的css细节及技巧
<!-- more -->
<The rest of contents | 余下全文>

#### 1. 关于line-height
```angular2html
	CSS中的 line-hight 属性是用来控制文本行之间的空隙的，
	line-height有单位时，子元素是继承父元素的line-height的，
	无单位时，其line-height等于无单位的数值乘以子元素本身的字体大小。
	显然为了不出现意外，还是建议首选无单位的。
```
#### 2. 文字溢出了不美观, 添加末尾省略号
```angular2html
    <!--添加固定高度-->
    width: 100px;
    
    <!--段落中的文本不进行换行 默认值normal-->
    white-space: nowrap;
    
    <!--超出部分截掉-->
    overflow: hidden;
    
    <!--当文本溢出包含元素时发生的事情 参数如下：-->
    <!--clip(默认值)        修剪文本-->
    <!--ellipsis	显示省略符号来代表被修剪的文本-->
    <!--string	使用给定的字符串来代表被修剪的文本-->
    text-overflow: ellipsis;
```
#### 3. 块元素垂直居中
```angular2html
<!--父元素-->
display: table

<!--子元素-->

display: table-cell;
vertical-align: middle;

```

#### 4. 移动端滚动流畅效果

-webkit-overflow-scrolling属性是来控制元素在移动设备上是否有回弹的效果

在移动端上，在你用overflow-y:scorll属性的时候，你会发现滚动的效果很木，很慢，这时候可以使用-webkit-overflow-scrolling:touch这个属性，让滚动条产生滚动回弹的效果，就像ios原生的滚动条一样流畅


参数:
- auto 使用普通滚动, 当手指从触摸屏上移开，滚动会立即停止。
- touch 使用具有回弹效果的滚动, 当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。

兼容性写法：
```javascript
verflow:auto;/* winphone8和android4+ */
-webkit-overflow-scrolling: touch; /* ios5+ */

```

**bug注意**

当你给一个元素设置过position:absolute;或者position:relative;后再增加-webkit-overflow-scrolling: touch;属性后，你会发现，滑动几次后可滚动区域会卡主，不能在滑动，这时给元素增加个z-index值就可以了

[-webkit-overflow-scrolling的更多查看](https://www.cnblogs.com/xiahj/p/8036419.html)
