---
title: 模块化开发
date: 2016-12-06 10:35:08
description: 关于模块化开发的学习笔记
categories:
- JavaScript 小知识
tags: 
- 模块化开发学习
toc: true 文章目录
---

+ 关于模块化开发的学习笔记
<!-- more -->
<The rest of contents | 余下全文>

## 模块化开发
### 1. 概述
##### 1.1 当你的网站开发越来越大复杂的时候，会经常遇到什么问题？
* 命名冲突
* 文件依赖
* 。。。。。。
* 各种问题。。。

##### 1.2 程序模块化开发优点
* 开发效率高
    - 代码方便重用，别人开发的模块直接拿过来就可以使用，
    - 不需要重复开发类似的功能
* 方便后期维护
    - 软件的声明周期中最长的阶段其实并不是开发阶段,
    而是维护阶段，需求变更比较频繁，使用模块化的开发

##### 1.3 总结
* 生产角度
    一种生产方式，生产效率高，维护成本低
* 软件开发角度
    就是一种开发模式，写代码的一种方式，开发效率高，
    方便后期维护

##### 1.4. 非模块化开发的问题
* 命名冲突
    团队协作开发，不同开发人员的变量和函数命名可能
    相同
* 文件依赖
    代码重用时，引入js文件的数目可能少了或者引入的
    顺序不对

#####  1.5 模块的维护扩展(计算器实例)
###### 1.5.1 非模块化下的弊端
* 计算器模块：在全局定义四个分别用来计算加减乘除的四个函数
* 全局函数形成的模块只能人为的认为它们属于一个模块
* 但是程序并不能区分哪些函数是同一个模块
问题
* “污染”了全局变量，无法保证不与其它模块发生变量名冲突
* 模块成员之间看不出直接关系

###### 1.5.2 对象封装 – 命名空间
* 将原来的计算器的四个函数封装到一个对象命名空间下
* 通过添加命名空间的形式从某种程度上解决了变量命名冲突的问题，但是并不能从根本上解决命名冲突
* 从代码级别可以明显的区分出哪些函数属于同一个模块
* 问题
    - 暴露了所有的模块成员，内部状态可以被外部改写，不安全
    - 命名空间越来越长


```lasso
<body>
第一个数：<input id='x' type="text" name='name'>
	<select name="" id="opt">
		<option value="+">+</option>
		<option value="-">-</option>
		<option value="*">*</option>
		<option value="/">/</option>
	</select>
	第二个数: <input  id='y' type="text" name='name'>
	<button type='button' name='button' id='btn'>=</button>
	结果：<input type="text" name='name' id='result'>
	<script src='./calculator.js'></script>
	<script>
		var cal = (function(c){
			c.divide = function(x,y){	return x / y;	};
			c.multiply = function(x,y){ return x * y;   }
			return c;
		})(window.cal||{});
		var btn = document.getElementById('btn');
		btn.onclick = function(){
			// 1. 获取文本框的值
		var x = parseInt(document.getElementById('x').value);
		var y = parseInt(document.getElementById('y').value);
		// 2. 获取运算符
		var opt = document.getElementById('opt');
		var sum = 0;
		// 3. 运算
			switch(opt.value){
				case '+':sum = cal.add(x,y);break;
				case '-':sum = cal.sub(x,y);break;
				case '*':sum = cal.multiply(x,y);break;
				case '/':sum = cal.divide(x,y);break;
				default:
			}
			var result = document.getElementById('result');
			result.value = sum;
		}
	</script>
</body>

```
* 私有共有成员分离
    - 将命名空间形式的计算器模块封装为立即执行函数形式，通过在函数内部返回值得形式向外暴露
    - 私有空间的变量和函数不会影响全局作用域
    - 公开公有方法，隐藏私有空间内部的属性、元素

```lasso
var cal= (function(){
	return {
		add:function(x,y){
			return x + y;
		},
		sub:function(x,y){
			return x - y;
		}
	}
})();

```
##### 总结
* 通过向对象的属性挂载功能实现扩展
* 开闭原则
* 代码健壮性判断

##### 1.6 模块的第三方依赖
* 模块最好要保证模块的职责单一性，最好不要与程序的其它部分直接交互
* 通过向匿名函数注入依赖项的形式，除了保证模块的独立性，还使模块之间的依赖关系变得明显
```lasso
<body>
	<script src='./jquery-1.11.1.min.js'></script>
	<script src='./third.js'></script>
</body>

```

third.js:

```lasso
//匿名自执行函数
(function($,q,t){//可以通过控制台报错来纠正
	console.log($);
	$('body').css({'backgroundColor':'green'});
}(window.$||{},window.q||{},window.t||{}));//对外显示出这段代码所依赖的第三方
```

### 2. SeaJS的使用
一个基于CMD规范实现的模块化开发解决方案
作者：Alibaba 玉伯
官网：http://seajs.org/
github：https://github.com/seajs/seajs
特性：
简单友好的模块定义规范
自然直观的代码组织方式

##### 2.1 全局函数
```
// js加载机制中,由上到下加载
// 如果遇上变量的声明 是全局作用域
// 当全局中出现两个同名变量时? 会被覆盖,  而不是先声明后不允许重复声明
```
* 文件引入顺序的依赖
* 全局变量的污染,无法使用多个同名变量
* 变量之间的关系不明显
##### 2.2 命名空间
* 解决了同名函数命名冲突问题
* 对象将所有数据都向外暴露了,可以供外部修改,不安全
  + 私有属性,一般不向外暴露,而是提供公共的访问方式
##### 2.3 划分私有命名空间
* 1: 为了保证属性私有化,使用自执行函数,让变量是函数的作用域
* 2: 不再将变量直接暴露,而是提供对应的访问方式
* 3: 基于命名空间的保护,可以允许同名属性名称
```
function getMoney(){
  return money;
}
```
##### 2.4 扩展与维护
* 坚持开闭原则(对于添加开发,对于修改是封闭)
* 使用自执行函数,将对象传递进来,进行属性的添加(挂载新属性)
* window.cal||{} 是为了保证程序的健壮性
```
var cal = (function(c){
    //添加乘法和除法功能
    c.divide = function(x,y){//查找机制是当前没有,就向上级查找
      return x/y;
    }
    c.multiply = function(x,y){
      return x * y;
    }
     return c;//将对象返回回去
})(window.cal||{}); //从当前window对象取cal对象,如果没有,就给空对象,

```
##### 2.5 第三方依赖
* 依赖没有文档说明:  根据报错一个个的来整
* 依赖注入: 将自己逐级向上查找,转换为,通知外部传入
  + (window.$||{},wndow.q||{},wndow.t||{});  将需要的对象在参数列表声明
  
##### 2.6 模块化的好处
* 模块化的好处: 生产效率高/便于维护/便于扩展/复用性高

##### 2.7 模块化分类  CMD&AMD
* CMD  推崇同步加载 加载机制: 延迟加载(滞后/懒加载)
    + 开发代码的规范（严格来讲就是模块化） ---制约 文本要求
    + seajs 按照CMD实现（先有规范 后有实现）
    + seajs 模块化加载框架  代码就是CMD代码 seajs作者 玉伯
* AMD  推崇异步加载 加载机制: 前置加载(优先加载)
    + 开发代码的规范（严格来讲就是模块化） ---制约 文本要求
    + require.js (老外开发)
* 先要根据规范才有实现 --> seajs 或者 requireJS  
##### 2.8 使用步骤
1. 引入 sea.js 库
```lasso
<script src='../code/seajs-3.0.0/dist/sea.js'></script>
```
2. 启动模块
```lasso
seajs.use(‘模块id’,function( 模块对象 ){ 业务代码 }); 
```
3. 定义模块
* 由于需要用到module 当前模块得声明成define(function(require, exports, module)
```lasso
define(function(require, exports, module){ 模块代码 });
```
4. 引入模块 获取返回值
（暴露接口）
* 一个js文件通过 module.exports = 向外返回的对象,让外部拿到
* 外部需要拿到对象 通过require('./spinning');
```lasso
define(function(require,exports,module){
	// 引入模块
	var xiaohong = require('./xiaohong');
	console.log(xiaohong);
})
```

5. 导出模块

```lasso
//定义一个模块
define(function(require,exports,module){
	module.exports = '小红';

```
##### 2.8.1 定义模块 define
* 先有规范，后有实现
* 在CMD规范中，一个模块就是一个js文件
* define 是一个全局函数，用来定义模块define( factory )
```lasso
    - 对象{}这种方式，外部会直接获取到该对象
    define(function(require,exports,module){})
```
```lasso
    - 字符串
    define('字符串')
```
```lasso
    - 函数function( require, exports, module ){ // 模块代码 }
    为了减少出错，定义函数的时候直接把这三个参数写上
    duidefine({name:'jack','age':18})
```
##### 2.8.2 exports 和 module.exports
* 功能：通过给 exports或module.exports动态的挂载变量、函数或对象，外部会获取到该接口
* exports 等价于 module.exports
exports == module.exports  true
* 可以通过多次给exports 挂载属性向外暴露
* 不能直接给 exports 赋值
```lasso
eg: module.exports -> 空对象 <- exports
当两者被赋值时 module.exports = '123';exports = '234';
二者的指向改变  module.exports -> '123';exports -> '234';
seajs内部机制是return module.exports
所以 返回的是module.exports的值
```
* 如果想暴露单个变量、函数或对象可以通过直接给 module.exports 赋值 即可

##### 2.8.3 启动模块 seajs.use
* 在调用 seajs 之前，必须先引入 sea.js 文件
* 通过 seajs.use() 函数可以启动模块--默认传递一个参数
```lasso
(‘模块id’[,callback] )	加载一个模块，并执行回调函数
( [ ‘模块1’, ‘模块2’ ]  [, callback] )
```
* 加载多个模块，并执行回调函数--传递多个参数 返回值一一对应传递的数组参数顺序
callback 参数是可选的
seajs.use 和 DOM ready 事件没有任何关系
```

```
* 最好不要在 define 中 使用 seajs.use

##### 2.8.4 加载模块 require
* require(‘模块id路径字符串’) 
* 用于根据一个模块id加载该模块
* 参数必须是一个字符串
* 该方法返回值是 要加载的模块中的 module.exports 对象
* 只能在模块环境define中使用，define(factory)的构造方法第一个参数必须命名为 require
* 不要重命名require函数或者在任何作用域中给 require 重新赋值

##### 2.8.5 模块标识
* 模块标识就是一个字符串，用来标识模块
* 模块标识可以不包含后缀名  .js
* 以 . /或 ../ 开头的相对路径模块，相对于 require 所在模块的路径
* 不以 ./ 或 ../ 开头的顶级标识，会相对于模块的基础路径解析（配置项中的base）
1. 相对路径 ./main.js
2. 绝对路径
    - E:/frontend/employcourse/itcastCourse/课堂练习/14node/01-day/demo/10-path/main.js
    - 也可以是url地址： http://127.0.0.1:8080/js/a.js/js/a.js
    
3. 'base路径': 默认就是seajs加载的路径 dist路径
 	+ 不能以./开头 
 	+ 不能以盘符开头 
 	+ 不能以/开头 
 	+ 直接是一个名称 可以/结尾
```lasso

seajs.use(['abc'],function(){});
//abc不存在 默认在这里找
E:/frontend/employcourse/itcastCourse/课堂练习/14node/01-day/demo/seajs-3.0.0/dist/abc.js
//默认就是seajs加载的路径 dist路径
//如果在seajs加载的路径 dist路径下存在abc.js文件 就不会报错
否则找不到abc会报错
```
##### 2.9 高级配置SeaJS
1. alias：别名配置
* 起一个别名 可以是绝对路径 不能是盘符目录
```lasso
html文件：
seajs.config({
		alias:{
				'jack':'E:/frontend/employcourse/itcastCourse/课堂练习/14node/01-day/demo/11-config/b.js'// 只能做文件的别名
			}
		});
		// seajs.use(['main'],function(a){});
		seajs.use(['./tmp/main'],function(a){});
main.js文件:
define(function(require,exports,module){
     require('jack');
})

```
2. paths：路径配置 (只能做文件的别名)
* 给目录起一个别名
使用： require(对应的字段名)
```lasso
html文件：
seajs.config({
			 paths:{
			'dirA':'E:/frontend/employcourse/itcastCourse/课堂练习/14node/01-day/demo/11-config'
		 }
		});
		seajs.use(['./tmp/main'],function(a){});
main.js文件:
define(function(require,exports,module){
     require('dirA/b.js');
})

```
3. base：基础路径
```lasso
html文件：
seajs.config({
		 base:'E:/frontend/employcourse/itcastCourse/课堂练习/14node/01-day/demo/11-config/tmp'
		});
		// seajs.use(['main'],function(a){});
		seajs.use(['./tmp/main'],function(a){});
main.js文件:
define(function(require,exports,module){
     require('../b.js');
})
```
4. vars：变量配置
5. map：映射配置
* 应用场景：目录层级过深 或者跨盘符的情况就可以使用

##### 3.0 前端模块自动化整合
* 减少请求 js代码合并
* 合并的时候 如何区分模块
gulp gulp-concat 
gulp-transport gulp-useref
seajs合并
本身index.html可以引入具体的文件,辨识
```lasso
//生成的路径  已经根据dest函数指定的路径了
<!-- build:js js/seajs.js -->
	<script src='../../seajs-3.0.0/dist/sea.js'></script>
<!-- build:js js/seajs.js -->
//将src目录下的内容构建到dist目录下

```

##### 4. seajs与require
两者区别：
* seajs：
	+ 同步加载
	+ 加载机制：延迟加载 | 懒加载 | 滞后
	+ 什么时候用什么时候加载
* require.js:
	+ 推崇异步加载
	+ 加载机制：加载前置 | 优先加载
	+ 要用什么就先加载什么

### use 参数方式 异步加载  让用户不会觉得加载慢


	

### require 特点
 * require 加载机制中 模块优先从缓存中获取 如果缓存中存在 就取 不存在 就加载
 * 异步调用
  + 可以使用： require.async()
 * 不管异步调用多少次  都是同步优先
  + 异步就是不阻塞 后续代码执行

 ### 两种路径类别
 * 补上.js也可以 相对路径 ./main.js
 * 绝对路径  带盘符
 	+ 绝对路径还可以是url路径 http://xxx.com
 * base路径 默认就是seajs加载的路径 dist路径
 	+ 不能以./开头 不能以盘符开头 不能以/开头 直接是一个名称 可以/结尾

 ### 高级配置
 * alias 指定一个文件的别名 可以是绝对路径 不能是盘符目录
 * paths 给目录起一个别名
 	+ 如何使用? require(对应的字段名)
 * base 基础路径 
 基于非绝对路径 非相对路径  才去处理
 ```


 ```

 * 应用场景 目录层级过深或者跨盘符的情况就可以使用
 
### 前端模块自动化整合
* 减少请求 js代码合并
* 合并的时候  如何区分模块
gulp gulp-concat gulp-seajs-transport gulp-useref
本身index.html 可以引入具体的文件,辨识
```

```

将src目录下的内容构建到dist目录

### requireJS
* 引入
* 启动模块 requirejs([依赖的模块1，依赖的模块2],function(模块1的返回值，模块2的返回值){
	
})
* require引入的时候必须是数组 推荐：其他模块define的时候也用数组
* define([依赖的模块1，依赖的模块2],(模块1的返回值，模块2的返回值){}
* 接受模块的返回值，function的参数中接受（模块1的返回值）
* 向外导出 函数内直接return

### seajs和requireJS的区别
* 代码的样子：requirejs 依赖向声明前置 seajs声明滞后
* 加载机制：requirejs加载前置 seajs加载滞后（何时用何时加载）
* CMD 和AMD CMD推崇同步 AMD推崇异步
* 时间换空间

```


```
