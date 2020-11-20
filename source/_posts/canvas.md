---
title: Canvas学习笔记
date: 2015-10-26 01:56:14
description: 关于Canvas的学习笔记
categories:
- 前端绘图
tags: 
- Canvas学习
toc: true 文章目录
---

+ 关于Canvas的学习笔记
<!-- more -->
<The rest of contents | 余下全文>

### canvas简介

  + 概念：H5 提供的一个新的标签元素
  + Canvas是指可以在其上面通过脚本语言（通常为js） 绘制图形、图像以及制作动画的标签
  + Canvas本身不具备绘制能力，只是负责展示
  + 在浏览器不支持Canvas时，会将其当做div标签来解析
  + 在IE中，会将其当做文本节点来解析
  + 检查支持性
  
  	```javascript
	  var canvas = document.getElementById('tutorial');
	  if (canvas.getContext){
	    var ctx = canvas.getContext('2d');
	    // drawing code here
	  } else {
	    // canvas-unsupported code here
	  }
	```
  
### 基本使用
  + 创建一个画布
 	```html
	<canvas>在浏览器不支持canvas时，会显示这段文本。</canvas>
	```
	     
  - 在创建一canvas标签时，如果没有指定宽和高，默认为 300 * 150 （px）
  - 要用canvas的相关属性设置其宽和高；否则，用其他方式指定canvas的宽和高，会在默认值的基础上进行缩放。而在缩放时，并没有增加canvas内部的像素点的个数。
 
  + 获取绘制工具
  
  ```javascript
		// 第1步：获取相应画布
		var canvas = document.getElementById('canvasId');
		// 第2步：获取该画布的绘制工具
		var ctx = canvas.getContext('2d|WebGL');
  ```

  - getContext方法：参数为 '2d' or 'WebGL'
   * 2d:    获取绘制平面图形的工具
   * WebGL：获取绘制立体图形的工具
  - 在课程内，只学习绘制 2d 图形
  - 绘制工具是由归属的。只有画布自己的绘制工具 可以在本身上绘制图形、图像。
  
### canvas检查支持性
如果需要在UI上体现，我们可以在 `canvas` 标签内部放置元素，如果浏览器不支持 `canvas` 标签，那么内部的元素就会被浏览器解析，而显示出来。
```html
<canvas id="stockGraph" width="150" height="150">
  <p>Canvas not be support.</p>
</canvas>
```
除此之外，我们也可以用js的方式来检查。
```javascript
var canvas = document.getElementById('c1');
//如果canvas元素没有getContext方法，那么就证明浏览器不支持canvas。
if(!canvas.getContext){
  console.log('Canvas not be support.')
}
```
 
### 坐标系
 - 原点 在canvas的左上角
 - X轴：在原点的水平方向
 - Y轴：在原点的垂直方向
 
### 基本绘制工具
 - 移动笔触moveTo(x, y) x 确定点的 横坐标；y 确定点的 纵坐标。
 - 画线lineTo(x, y): 从当前笔触所在点 连线 到 指定点。
 
### 绘制直线步骤
 - 移动笔触moveTo
 - 画线lineTo
 - 描边stroke 或 填充fill
 
### 绘制矩形
不同于 svg，canvas 只支持两种形式的图形绘制：矩形和路径（由一系列点连成的线段），所有其他类型的图形都是通过一条或者多条路径组合而成的，不过，我们拥有众多路径生成的方法让复杂图形的绘制成为了可能。
 
 首先，我们回到矩形的绘制中，canvas提供了三种方法绘制矩形：
 - 绘制一个填充的矩形: fillRect(x, y, width, height) 
 - 绘制一个矩形的边框: strokeRect(x, y, width, height) 
 - 清除指定矩形区域，让清除部分完全透明: clearRect(x, y, width, height)
   
 ```javascript
	// 第1步：获取相应画布
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    // 第2步：获取该画布的绘制工具
		var ctx = canvas.getContext('2d');
		// 绘制一个填充的矩形: fillRect(x, y, width, height)
		// ctx.fillRect(25, 25, 100, 100)
		// 清除指定矩形区域，让清除部分完全透明: clearRect(x, y, width, height)
		// ctx.clearRect(25, 25, 100, 100)
		// ctx.clearRect(45, 45, 60, 60)
		// 绘制一个矩形的边框: strokeRect(x, y, width, height)
		ctx.strokeRect(50, 50, 50, 50)
	}
 ```
 
### 绘制路径

 - 绘制路径步骤：
	 + 首先，你需要创建路径起始点。
	 + 然后你使用画图命令去画出路径。
	 + 之后闭合路径closePath(),不是必需的。
	 + 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。
	 
 - 路径API:
	 + beginPath(): 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
	 + closePath(): 闭合路径之后图形绘制命令又重新指向到上下文中。
	 + stroke(): 通过线条来绘制图形轮廓。
	 + fill(): 通过填充路径的内容区域生成实心的图形。

** 注意 ** 
closePath()这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做
当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。

### 绘制三角形
```javascript
	var canvas = document.getElementById('canvasId')
	if (canvas.getContext) {
		var ctx = canvas.getContext('2d')
		// 填充三角形
		// ctx.beginPath()
	    // ctx.moveTo(75, 50)
	    // ctx.lineTo(100, 75)
	    // ctx.lineTo(100, 25)
	    // ctx.fill()
	    // 描边三角形
	    ctx.beginPath();
	    ctx.moveTo(125, 125);
	    ctx.lineTo(125, 45);
	    ctx.lineTo(45, 125);
	    ctx.closePath();
	    ctx.stroke();
	}
```

### 绘制梯形

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.beginPath();
		ctx.moveTo(35, 25);
		ctx.lineTo(125, 25);
		ctx.lineTo(155, 75);
		ctx.lineTo(5, 75);
		ctx.lineTo(35, 25);
		ctx.stroke();
	}
```

### 绘制笑脸

绘制圆弧或者圆的时候，我们可以使用如下方法：
- arc(x, y, radius, startAngle, endAngle, anticlockwise) 画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
- arcTo(x1, y1, x2, y2, radius) 根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。
- anticlockwise为true则表示逆时针绘制。

** 注意 ** arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式: 

弧度=(Math.PI/180)*角度。

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.beginPath();
	    ctx.arc(75, 75, 50, 0, Math.PI * 2, true); // 绘制
	    ctx.moveTo(110, 75);
	    ctx.arc(75, 75, 35, 0, Math.PI, false);   // 口(顺时针)
	    ctx.moveTo(65, 65);
	    ctx.arc(60, 65, 5, 0, Math.PI * 2, true);  // 左眼
	    ctx.moveTo(95, 65);
	    ctx.arc(90, 65, 5, 0, Math.PI * 2, true);  // 右眼
	    ctx.stroke();
	}
```

### 二次贝塞尔曲线及三次贝塞尔曲线

下一个十分有用的路径类型就是贝塞尔曲线。二次及三次贝塞尔曲线都十分有用，一般用来绘制复杂有规律的图形

- quadraticCurveTo(cp1x, cp1y, x, y) 绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
- bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y) 绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。

下面的图示能够很好的描述两者的关系，二次贝塞尔曲线有一个开始点（蓝色）、一个结束点（蓝色）以及一个控制点（红色），而三次贝塞尔曲线有两个控制点

![](/images/canvas_curves.png)

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
		// 绘制二次贝塞尔曲线  渲染对话气泡
	   	ctx.beginPath()
	   	ctx.moveTo(75, 25)
	   	ctx.quadraticCurveTo(25, 25, 25, 75)
	   	ctx.quadraticCurveTo(25, 125, 75, 125)
	   	ctx.quadraticCurveTo(50, 140, 40, 135)
	   	ctx.quadraticCurveTo(60, 140, 90, 125)
	   	ctx.quadraticCurveTo(90, 125, 170, 125)
	   	ctx.quadraticCurveTo(220, 125, 220, 75)
	   	ctx.quadraticCurveTo(220, 25, 170, 25)
	   	ctx.quadraticCurveTo(170, 25, 75, 25)
	   	ctx.stroke()
	   	// 绘制二次贝塞尔曲线 渲染对话气泡
	   	// ctx.beginPath();
	    // ctx.moveTo(75, 25);
	    // ctx.quadraticCurveTo(25, 25, 25, 62.5);
	    // ctx.quadraticCurveTo(25, 100, 50, 100);
	    // ctx.quadraticCurveTo(50, 120, 30, 125);
	    // ctx.quadraticCurveTo(60, 120, 65, 100);
	    // ctx.quadraticCurveTo(125, 100, 125, 62.5);
	    // ctx.quadraticCurveTo(125, 25, 75, 25);
	    // ctx.stroke();
	}
```

### 线性相关的属性
  + lineWidth：设置线宽
  + strokeStyle：设置线条颜色，值可以为 16进制、颜色字符串、rgb、rgba、渐变色
  + fillStyle：设置填充颜色
  + lineCap：设置线段两端的样式
	 - butt：默认
	 - round：设置线段两端为突出的圆角图形
	 - square：设置线段两端为突出的方形图形
  + lineJoin：设置线段相交点的样式
	 - round：圆角样式
	 - bevel：平角样式
	 - miter：尖角样式
	 
```javascript
// 绘制颜色
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    // fillStyle: 绘制颜色
		// ctx.fillStyle = '#FD0';
		// ctx.fillRect(0,0,75,75);
		// ctx.fillStyle = '#6C0';
		// ctx.fillRect(75,0,75,75);
		// ctx.fillStyle = '#09F';
		// ctx.fillRect(0,75,75,75);
		// ctx.fillStyle = '#F30';
		// ctx.fillRect(75,75,75,75);
		// ctx.fillStyle = '#FFF';
		// 设置透明度值
		// ctx.globalAlpha = 0.2;
		// 画半透明圆
		// for (var i=0;i<7;i++){
		//     ctx.beginPath();
		//     ctx.arc(75,75,10+10*i,0,Math.PI*2,true);
		//     ctx.fill();
		// }
		// lineJoin: 线两端设置样式
		ctx.lineWidth = 15;
		ctx.lineJoin = 'miter';
		ctx.beginPath();
		ctx.moveTo(20,20);
		ctx.lineTo(100,50);
		ctx.lineTo(20,100);
		ctx.stroke();
	}
// 绘制破浪
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    var lineJoin = ['round', 'bevel', 'miter'];
		ctx.lineWidth = 10;
		for (var i = 0; i < lineJoin.length; i++) {
		    ctx.lineJoin = lineJoin[i];
		    ctx.beginPath();
		    ctx.moveTo(-5, 5 + i * 40);
		    ctx.lineTo(35, 45 + i * 40);
		    ctx.lineTo(75, 5 + i * 40);
		    ctx.lineTo(115, 45 + i * 40);
		    ctx.lineTo(155, 5 + i * 40);
		    ctx.stroke();
		}
	}
```

![](/images/bolang.png)

### 绘制阴影效果

属性：
- shadowOffsetX: 设置阴影在 X 延伸距离，不受变换矩阵所影响，负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，默认 0 
- shadowOffsetY: 设置阴影在 Y 轴的延伸距离，不受变换矩阵所影响，负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，默认 0
- shadowBlur: 设置阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0
- shadowColor: 标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.shadowColor = "#545454";
		ctx.shadowOffsetX = 2;
		ctx.shadowOffsetY = 5;
		ctx.shadowBlur = 2;
		ctx.beginPath();
		ctx.moveTo(20,20);
		ctx.lineTo(100,50);
		ctx.lineTo(20,100);
		ctx.stroke();
	}
```

### 绘制立体圆圈
```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.fillStyle = "#FFF";
		ctx.fillRect(0,0,400,300);

		ctx.shadowColor = "#545454";
		ctx.shadowOffsetX = 5;
		ctx.shadowOffsetY = 5;
		ctx.shadowBlur = 2;

		ctx.arc(200, 150, 200, 0, Math.PI * 2 ,false);
		ctx.arc(200, 150, 115, 0, Math.PI * 2 ,true);
		ctx.fillStyle = "#00AAAA";
		ctx.fill();
	}
```

![](/images/circle-icon.png)

### 绘制文本

canvas 提供了两种方法来渲染文本:
- fillText(text, x, y [, maxWidth]) 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
- strokeText(text, x, y [, maxWidth]) 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.font = "48px serif";
	    ctx.textAlign = 'center';
		// ctx.fillText("Hello world", 10, 50);
		ctx.strokeText("Hello world", 10, 50);
	}
```

有样式的文本:
- font = value 当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 10px sans-serif。
- textAlign = value 文本对齐选项. 可选的值包括：start, end, left, right or center. 默认值是 start。
- textBaseline = value 基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。默认值是 alphabetic。
- direction = value 文本方向。可能的值包括：ltr, rtl, inherit。默认值是 inherit。

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.font = "40px sans-serif";
		ctx.textAlign = 'left'
		ctx.textBaseline = "hanging";
		ctx.direction = 'inherit';
		// ctx.fillText("Hello world", 10, 50);
		ctx.strokeText("Hello world", 10, 50);
	}
```

当你需要获得更多的文本细节时，下面的方法可以给你测量文本的方法。
- measureText() 将返回一个 TextMetrics对象的宽度、所在像素，这些体现文本特性的属性。

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.font = "40px sans-serif";
		// ctx.fillText("Hello world", 10, 50);
		ctx.strokeText("Hello world", 10, 50);
		var text = ctx.measureText("foo"); // TextMetrics object
		console.log(text.width) // 16;
	}
```

### 渐变

我们用下面的方法新建一个 canvasGradient 对象，并且赋给图形的 fillStyle 或 strokeStyle 属性

- createLinearGradient(x1, y1, x2, y2): createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。
- createRadialGradient(x1, y1, r1, x2, y2, r2): createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。

创建出 canvasGradient 对象后，我们就可以用 addColorStop 方法给它上色了

- gradient.addColorStop(position, color): 2 个参数，position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。color 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）

```javascript
var radgrad = ctx.createRadialGradient(45,45,10,52,50,30);
		radgrad.addColorStop(0, '#A7D30C');
		radgrad.addColorStop(0.9, '#019F62');
		radgrad.addColorStop(1, 'rgba(1,159,98,0)');
  		ctx.fillStyle = radgrad;
  		ctx.fillRect(0,0,150,150);
```

### 使用图片
引入图像到canvas里需要以下两步基本操作：
- 获得一个指向`HTMLImageElement`的对象或者另一个`canvas`元素的引用作为源，也可以通过提供一个URL的方式来使用图片
- 使用`drawImage()`函数将图片绘制到画布上

canvas的API可以使用下面这些类型中的一种作为图片的源：
- `HTMLImageElement` 这些图片是由`Image()`函数构造出来的，或者任何的`<img>`元素。
- `HTMLVideoElement` 用一个HTML的 `<video>`元素作为你的图片源，可以从视频中抓取当前帧作为一个图像。
- `HTMLCanvasElement` 可以使用另一个 `<canvas>` 元素作为你的图片源。
- `ImageBitmap` 这是一个高性能的位图，可以低延迟地绘制，它可以从上述的所有源以及其它几种源中生成。

这些源统一由 `CanvasImageSource` 类型来引用。

#### 第一步：获得需要绘制的图片方法->
1. 我们可以通过下列方法的一种来获得与canvas相同页面内的图片的引用：
- document.images集合
- document.getElementsByTagName()方法
如果你知道你想使用的指定图片的ID，你可以用document.getElementById()获得这个图片
2. 使用其它域名下的图片: 在 `HTMLImageElement` 上使用`crossOrigin`属性，你可以请求加载其它域名上的图片。如果图片的服务器允许跨域访问这个图片，那么你可以使用这个图片而不污染`canvas`，否则，使用这个图片将会污染`canvas`
3. 使用其它 canvas 元素: 和引用页面内的图片类似地，用 document.getElementsByTagName 或 document.getElementById 方法来获取其它 canvas 元素。但你引入的应该是已经准备好的 canvas。
4. 由零开始创建图像: 或者我们可以用脚本创建一个新的 `HTMLImageElement` 对象。要实现这个方法，我们可以使用很方便的`Image()`构造函数。
```javascript
var img = new Image();   // 创建一个<img>元素
img.src = 'myImage.png'; // 设置图片源地址

```
当脚本执行后，图片开始装载。
若调用 drawImage 时，图片没装载完，那什么都不会发生（在一些旧的浏览器中可能会抛出异常）。因此你应该用load事件来保证不会在加载完毕之前使用这个图片：
```javascript
var img = new Image();   // 创建img元素
img.onload = function(){
  // 执行drawImage语句
}
img.src = 'myImage.png'; // 设置图片源地址
// 如果你只用到一张图片的话，这已经够了
```
5. 通过 data: url 方式嵌入图像
```javascript
img.src = 'data:image/gif;base64,R0lGODlhCwALAIAAAAAA3pn/ZiH5BAEAAAEALAAAAAALAAsAAAIUhA+hkcuO4lmNVindo7qyrIXiGBYAOw==';

```
6. 使用视频帧: 你还可以使用<video> 中的视频帧（即便视频是不可见的）。例如，如果你有一个ID为“myvideo”的<video> 元素，你可以这样做：
```javascript
function getMyVideo() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    return document.getElementById('myvideo');
  }
}
// 它将为这个视频返回HTMLVideoElement对象，正如我们前面提到的，它可以作为我们的Canvas图片源
```

#### 第二步：绘制图片->
一旦获得了源图对象，我们就可以使用 drawImage 方法将它渲染到 canvas 里。drawImage 方法有三种形态，下面是最基础的一种。
- drawImage(image, x, y) 
其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。
- drawImage(image, x, y, width, height) 
这个方法多了2个参数：width 和 height，这两个参数用来控制 当向canvas画入时应该缩放的大小
- drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
参数参照下面的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。
![](/images/drawImg-slicing.png)

### 利用drawImage的切片功能

```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    var img = new Image();
		img.onload = () => {
		    // 绘制的原图片 切片前
			ctx.drawImage(img,0,0);
			// 绘制切片后
			ctx.drawImage(img,33,71,104,124,300,80,100,104);
		};
		img.src = 'https://mdn.mozillademos.org/files/5397/rhino.jpg';
	}
```
效果如下：
![](/images/slice-img.png)

### 平铺图像
```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    // 第一步：由零开始创建图像
	    var img = new Image();
		img.onload = () => {
			for (var i=0;i<4;i++){
				for (var j=0;j<3;j++){
				    // 第二步：获取到图片源img，drawImage 绘制图片
					ctx.drawImage(img,j*50,i*38,50,38);
				}
			}
		};
		img.src = 'https://mdn.mozillademos.org/files/5397/rhino.jpg';
	}
```
平铺3行4列效果如下：

![](/images/tile-picture.png)

### 控制图像的缩放行为

Gecko 1.9.2 引入了 mozImageSmoothingEnabled 属性，值为 false 时，图像不会平滑地缩放。默认是 true 。
```javascript
cx.mozImageSmoothingEnabled = false;
```

### canvas变形

#### 状态的保存和恢复

在下面学习变形之前，先介绍两个在开始绘制复杂图形时必不可少的方法:

- save() 保存画布(canvas)的所有状态
- restore() save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。

Canvas状态存储在栈中，每当 save() 方法被调用后，当前的状态就被推送到栈中保存
你可以调用任意多次 save 方法。每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复。

#### 连续矩形绘制
```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.fillRect(0,0,150,150);   // 使用默认设置绘制一个矩形
		ctx.save();                  // 保存默认颜色状态1

		ctx.fillStyle = '#09F'       // 在原有配置基础上对颜色做改变
		ctx.fillRect(15,15,120,120); // 使用新的设置绘制一个矩形
		ctx.save();                  // 保存当前颜色状态2

		ctx.fillStyle = '#FFF'       // 再次改变颜色配置
		ctx.globalAlpha = 0.5;
		ctx.fillRect(30,30,90,90);   // 使用新的配置绘制一个矩形

		ctx.restore();               // 重新加载之前的颜色状态2
		ctx.fillRect(45,45,60,60);   // 使用上一次的配置绘制一个矩形

		ctx.restore();               // 重新加载默认颜色状态1
		ctx.fillRect(60,60,30,30);   // 使用加载的配置绘制一个矩形
	}
```

### 移动 Translating

- translate(x, y): 它用来移动 canvas 和它的原点到一个不同的位置; x 是左右偏移量，y 是上下偏移量，如下图所示。
![](/images/canvas-translate.png)

这个例子显示了一些移动 canvas 原点的好处。如果不使用 translate 方法，那么所有矩形都将被绘制在相同的位置（0,0）。translate 方法同时让我们可以任意放置这些图案，而不需要在 fillRect() 方法中手工调整坐标值，既好理解也方便使用。

我在 draw 方法中调用 fillRect() 方法 9 次，用了 2 层循环。每一次循环，先移动 canvas ，画螺旋图案，然后恢复到原始状态

```javascript
	var canvas = document.getElementById('canvasId');
  	if (canvas.getContext) {
  	    var ctx = canvas.getContext('2d')
  	    for (var i = 0; i < 3; i++) {
            for (var j = 0; j < 3; j++) {
                // 在做变形之前先保存状态是一个良好的习惯
                ctx.save();
                ctx.fillStyle = 'rgb(' + (51 * i) + ', ' + (255 - 51 * i) + ', 255)';
                ctx.translate(10 + j * 50, 10 + i * 50);
                ctx.fillRect(0, 0, 25, 25);
                ctx.restore();
            }
          }
	}
```

### 旋转 Rotating
第二个介绍 rotate 方法，它用于以原点为中心旋转 canvas。

- rotate(angle) 这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法。

通过例子了解一下：
这里我们又用到了两层循环。第一层循环决定环的数量，第二层循环决定每环有多少个点。每环开始之前，我都保存一下 canvas 的状态，这样恢复起来方便。每次画圆点，我都以一定夹角来旋转 canvas，而这个夹角则是由环上的圆点数目的决定的。最里层的环有 6 个圆点，这样，每次旋转的夹角就是 360/6 = 60 度。往外每一环的圆点数目是里面一环的 2 倍，那么每次旋转的夹角随之减半
```javascript
	var canvas = document.getElementById('canvasId');
	if (canvas.getContext) {
	    var ctx = canvas.getContext('2d')
	    ctx.translate(75,75);
	    for (var i=1;i<6;i++){ // Loop through rings (from inside to out)
			ctx.save();
			ctx.fillStyle = 'rgb('+(51*i)+','+(255-51*i)+',255)';
		
			for (var j=0;j<i*6;j++){ // draw individual dots
			  ctx.rotate(Math.PI*2/(i*6));
			  ctx.beginPath();
			  ctx.arc(0,i*12.5,5,0,Math.PI*2,true);
			  ctx.fill();
			}
			ctx.restore();
	    }
	}
```
### 缩放 Scaling
接着是缩放。我们用它来增uni-app减图形在 canvas 中的像素数目，对形状，位图进行缩小或者放大。

- scale(x, y) 可以缩放画布的水平和垂直的单位。两个参数都是实数，可以为负数，x 为水平缩放因子，y 为垂直缩放因子，如果比1小，会比缩放图形， 如果比1大会放大图形。默认值为1， 为实际大小。

画布初始情况下， 是以左上角坐标为原点的第一象限。如果参数为负实数， 相当于以x 或 y轴作为对称轴镜像反转（例如， 使用translate(0,canvas.height); scale(1,-1); 以y轴作为对称轴镜像反转， 就可得到著名的笛卡尔坐标系，左下角为原点）。

默认情况下，canvas 的 1 个单位为 1 个像素。举例说，如果我们设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。同理，设置为 2.0 时，1 个单位就对应变成了 2 像素，绘制的结果就是图形放大了 2 倍

```javascript
  var canvas = document.getElementById('canvasId');
  if (canvas.getContext) {
      var ctx = canvas.getContext('2d')
      ctx.save();
      ctx.scale(10, 3);
      ctx.fillRect(5, 10, 10, 10);
      ctx.restore();

      // mirror horizontally
      ctx.scale(-1, 1);
      ctx.font = '48px serif';
      ctx.fillText('QILANBEI', -180, 120);
  }
```

![](/images/canvas-scaling.png)


### 变形 Transforms 

##### 状态的保存和恢复
在下面学习变形之前，先介绍两个在开始绘制复杂图形时必不可少的方法:
• save() 保存画布(canvas)的所有状态
• restore() save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。
• 
Canvas状态存储在栈中，每当 save() 方法被调用后，当前的状态就被推送到栈中保存
你可以调用任意多次 save 方法。每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复。

连续矩形绘制：
```javascript
	var canvas = document.getElementById('canvasId');
		if (canvas.getContext) {
		  var ctx = canvas.getContext('2d')
		  ctx.fillRect(0,0,150,150);   // 使用默认设置绘制一个矩形
		ctx.save();                  // 保存默认颜色状态1
		
		ctx.fillStyle = '#09F'       // 在原有配置基础上对颜色做改变
		ctx.fillRect(15,15,120,120); // 使用新的设置绘制一个矩形
		ctx.save();                  // 保存当前颜色状态2
		
		ctx.fillStyle = '#FFF'       // 再次改变颜色配置
		ctx.globalAlpha = 0.5;
		ctx.fillRect(30,30,90,90);   // 使用新的配置绘制一个矩形
		
		ctx.restore();               // 重新加载之前的颜色状态2
		ctx.fillRect(45,45,60,60);   // 使用上一次的配置绘制一个矩形
		
		ctx.restore();               // 重新加载默认颜色状态1
		ctx.fillRect(60,60,30,30);   // 使用加载的配置绘制一个矩形
	}
```
如图所示：
![](/images/canvas-rect.png)

##### 移动 Translating

translate(x, y): 它用来移动 canvas 和它的原点到一个不同的位置; x 是左右偏移量，y 是上下偏移量，如下图所示:

![](/images/canvas-transform-axis.png)

通过例子来了解一下：
用draw 方法中调用 fillRect() 方法 9 次，用了 2 层循环。每一次循环，先移动 canvas ，画螺旋图案，然后恢复到原始状态

```javascript
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i = 0; i < 3; i++) {
    for (var j = 0; j < 3; j++) {
      ctx.save();
      ctx.fillStyle = 'rgb(' + (51 * i) + ', ' + (255 - 51 * i) + ', 255)';
      ctx.translate(10 + j * 50, 10 + i * 50);
      ctx.fillRect(0, 0, 25, 25);
      ctx.restore();
    }
  }
}
```

![](/images/canvas-draw-rect.png)

##### 旋转 Rotating

第二个介绍 rotate 方法，它用于以原点为中心旋转 canvas。

 - rotate(angle) 这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法。

![](/images/canvas-rota-axis.png)

通过例子了解一下：
这里我们又用到了两层循环。第一层循环决定环的数量，第二层循环决定每环有多少个点。每环开始之前，我都保存一下 canvas 的状态，这样恢复起来方便。每次画圆点，我都以一定夹角来旋转 canvas，而这个夹角则是由环上的圆点数目的决定的。最里层的环有 6 个圆点，这样，每次旋转的夹角就是 360/6 = 60 度。往外每一环的圆点数目是里面一环的 2 倍，那么每次旋转的夹角随之减半

```javascript
var canvas = document.getElementById('canvasId');
  if (canvas.getContext) {
      var ctx = canvas.getContext('2d')
      ctx.translate(75,75);
      for (var i=1;i<6;i++){ // Loop through rings (from inside to out)
      ctx.save();
      ctx.fillStyle = 'rgb('+(51*i)+','+(255-51*i)+',255)';
    
      for (var j=0;j<i*6;j++){ // draw individual dots
        ctx.rotate(Math.PI*2/(i*6));
        ctx.beginPath();
        ctx.arc(0,i*12.5,5,0,Math.PI*2,true);
        ctx.fill();
      }
      ctx.restore();
      }
  }
```
![](/images/canvas-rotate-circle.png)



### 非零环绕原则
  + 目的：确定某一区域是否需要填充。
  + 描述：定义计数默认为0，从当前区域选取任意点，然后从该点到整个图形外部绘制一条射线。
	 观察和这条射线相交的所有线段。
	 如果该相交的线段是顺时针穿过这条射线，那么 计数 + 1；如果是逆时针穿过，那么计数就 - 1；
	 如果计数为 0，那么就不填充该区域；
	 如果计数非 0 ，那么就填充该区域。         
  + 练习：绘制空心矩形 
  + 总结：从左到右穿出 或 从上至下 穿出，那么就是顺时针。计数 + 1；否则， 计数 - 1；
               
### 奇-偶原则
  + 目的：同上
  + 描述：就看相交线的个数，如果为奇数，就填充；否则就不填充。
      
### 纵向的颜色渐变
### 绘制坐标系
### 折线图中点的描绘
### 用小矩形来描绘点
