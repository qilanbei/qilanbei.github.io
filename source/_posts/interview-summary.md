---
title: 面试题总结
date: 2016-08-20 01:56:14
description: 自己面试题总结
categories:
- 前端面试总结
tags: 
- 自己面试题总结
toc: true 文章目录
---

+ 自己面试题总结
<!-- more -->
<The rest of contents | 余下全文>

1.一个宽度为120px的div里面有三个40px的div,这时候给三个div加上border 1px的话，会发生什么，如何解决？
答：加上border 1px的话，最后一个会掉下来，给父盒子设置display:flex; 给每个子盒子div设置flex为1；当给父盒子设置display:flex;时 就已经不会掉下来了。
2.两个img标签中间有间隙如何解决？
答：使用display: inherit;
发生场景：两个img设置父盒子的一半宽，两个img不会显示在一行上，原因就是两个img中间有间隙，导致一行放不下。
3.vertical-align的属性有哪些？
答：该属性定义行内元素的基线相对于该元素所在行的基线的垂直对齐；
允许指定负长度值和百分比值；

|baseline|元素放在父元素的基线上|默认值|
| :-------- | --------:| :------: |
| sub |垂直对齐文本的下标|...|
| super |垂直对齐文本的上标|...|
|top|把元素的顶端与行中最高元素的顶端对齐|...|
|text-top  |把元素的顶端与父元素字体的顶端对齐|...|
|middle|  把此元素放置在父元素的中部|...|
|bottom |把元素的顶端与行中最低的元素的顶端对齐|...|
|text-bottom |把元素的底端与元素字体的底端对齐|...|
|length|...|...|
|%  |使用继承的“line-height”属性的百分比来排列此元素|允许负值|
|inherit |规定应该从父元素继承vertical-align属性的值|任何版本的IE都不支持inherit属性|
应用场景：
```
//单选或者多选框
<input type="checkbox" class="checkbox"> 你好
//单选框或者复选框与文字不对齐，可以通过设置vertical-align:-2px;来保持选框和文字对齐。
```

4.什么是基线，基线定义的标准是什么？
没找到特别好的答案
5.
6.display: inline-block;和display: block都可以设置宽高吗?
太那什么了，当然都可以，行元素直接设置宽高会无效,这时给行元素设置inline-block或者block就可以设置宽高了
7.设置元素垂直居中对齐
```
<div class="box">
	<img src="./img.ipg">
</div>
//如果box的子元素是img的话，通过给box设置以下样式可以让img垂直居中对齐
//display: table-cell;vertical-align:middle;text-align:center;
//这里的vertical-align要在table-cell下才会生效
<div class="box">
	<div class="box1"></div>
</div>
//如果里面是div，给box设置上面的样式，是不会让他水平居中的，要这样设置
.box {
	display: table-cell;
	vertical-align: middle;
}
.box1 {
	//width: height:设置 为了容易看见可以设置background
	margin: 0 auto;
}
//通过定位让元素垂直居中(无论子元素是什么都可以)
父元素{position: relative}
子元素{
	position:absolute;
	top: 50%;
	left: 50%;
	margin-top:　－子元素高的一半;
	margin-left: -子元素宽的一半;
}
```
```
8. <input type="button">和<button</button>有什么区别？
```
1> 在button标签内可以放置文本和图像，以图像为背景的按钮
但是在input中添加img无法实现
2> 如果在 HTML 表单中使用 button 元素，不同的浏览器会提交不同的值。IE将提交 button标签之间的文本，而其他浏览器将提交 value 属性的内容。所以在 HTML 表单中使用 input 元素来创建按钮。
3> 如果是button标签的话，通过.val()获取的是标签之前的文本，而不是value的值
例如： 
```
<button class="button" value="test">按钮</button>
<script>
	$('.button').val()
	// 在IE上获取的是“按钮”，而不是“test”
</script>

```

9.数组的方法，那些会改变原数组，那些不改变，字符串的呢？
数组的方法总结：

|arr方法：|描述|是否改变原数组|
| :-------- | --------:| :------: |
|join()|根据字符将数组元素拼接成字符串|不改变原数组|
|push()和pop()| 从数组的前面添加和删除元素（push返回新的长度,pop返回数组的最后一个元素）| 改变|
|shift()和unshift()|从数组的后天删除和添加元素（shift返回最后一个元素，unshift返回数组的新长度）|改变|
|sort(sortby)| 数组排序（参数sortby是个函数，规定排序顺序）|改变原数组|
|concat()|连接两个数组或者更多的数组并返回结果|不改变原数组|
|reverse()|颠倒数组中元素的顺序|改变原数组|
|indexOf(m,n)和lastIndexOf()|查询数组中是否存在m,n是查找的起点位置,astIndexOf()从后开始查找|...|
|forEach()|对数组进行遍历循环||
|splice|可以实现删除、插入和替换|改变数组，该返回数组中包含从原始数组中删除的项，如果没有删除任何项，则返回一个空数组|
|slice(start,end)|截取一段数据含start,不含end|不改变原数组|
```
var arr = new Array(6)
arr[0] = "George"
arr[1] = "John"
arr[2] = "Thomas"
arr[3] = "James"
arr[4] = "Adrew"
arr[5] = "Martin"
document.write(arr.sort())
```
```
10. for(var i = 0; i < 5; i++){
			setTimeout(function(){
				console.log(i);
			},0);
		}
打印5个5
因为setTimeout是异步定时函数，就算定时为0，也要等到循环执行结束之后太执行定时操作
```
11.( function(){…} )()和( function (){…} () )这两种立即执行函数的写法有什么不同？
(function(){})(); 是 把函数当作表达式解析，然后执行解析后的函数
相当于 var a = function(){}; a(); a得到的是函数
(function(){}()); 是把函数表达式和执行当作语句直接执行、
相当于 var a = function(){}(); a得到的是结果

#### 会不断总结。。。
