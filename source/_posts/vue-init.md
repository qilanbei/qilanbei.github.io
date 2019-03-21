---
title: Vue学习笔记
date: 2016-8-25 01:56:14
description: Vue入门学习笔记
categories:
- Vue
tags: 
- Vue入门学习
toc: true 文章目录
---

+ Vue入门学习笔记
<!-- more -->
<The rest of contents | 余下全文>


学习过程中整理,参考网址https://segmentfault.com/a/1190000008879966
官网地址：https://cn.vuejs.org/

1.vue 生命周期
>所有的生命周期钩子自动绑定 this 上下文到实例中，因此你可以访问数据，对属性和方法进行运算。这意味着 你不能使用箭头函数来定义一个生命周期方法 (例如 created: () => this.fetchTodos())。这是因为箭头函数绑定了父上下文，因此 this 与你期待的 Vue 实例不同， this.fetchTodos 的行为未定义。

1.1生命周期概总：
>根组件实例：8个 (beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed)
组件实例：8个 (beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed)
全局路由钩子：2个 (beforeEach、afterEach)
组件路由钩子：3个 (beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave)
指令的周期： 5个 (bind、inserted、update、componentUpdated、unbind)
beforeRouteEnter的next所对应的周期
nextTick所对应的周期
1.2生命周期详解
生命周期图示：![enter image description here](https://cn.vuejs.org/images/lifecycle.png)

组件实例周期:
1> beforeCreate: 
在vue实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用，页面初始化数据加载一般写这里。此时组件的选项还未挂载，因此无法访问methods，data,computed上的方法或数据。

2> created():
vue实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见，所以如果涉及操作dom对象的方法不适合写在这里。
你可以在这里调用methods中的方法、改变data中的数据，并且修改可以通过vue的响应式绑定体现在页面上、获取computed中的计算属性等等。
通常我们可以在这里对实例进行预处理。
在这里也可以发ajax请求，值得注意的是，这个周期中是没有什么方法来对实例化过程进行拦截的。因此假如有某些数据必须获取才允许进入页面的话，并不适合在这个页面发请求。建议在组件路由勾子beforeRouteEnter中来完成。

3> beforeMonut:
在挂载开始之前被调用，相关的 render 函数首次被调用。该钩子在服务器端渲染期间不被调用。开始挂载编译生成的HTML到对应位置时触发。
vue 已经绑定到元素身上，但是还没把数据同步给相应的元素

4> mounted():
在这个周期内，对data的改变可以生效，可以获取dom元素，但是要进下一轮的dom更新，dom上的数据才会更新。
该钩子在服务器端渲染期间不被调用。
beforeRouteEnter的next的钩子比mounted触发还要靠后，指令的生效在mounted周期之前。

5>beforeUpdate():
数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

6>updated():
由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态(？)。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。
该钩子在服务器端渲染期间不被调用。

7>beforeDestroy():
实例销毁之前调用。在这一步，实例仍然完全可用，还可以用this来获取实例，一般在这一步做一些重置的操作。比如清除掉组件中的定时器和监听的dom事件。
该钩子在服务器端渲染期间不被调用。

8>destroyed():
Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
该钩子在服务器端渲染期间不被调用。

全局路由钩子:
作用于所有路由切换，一般在main.js里面定义。
1>router.beforeEach():
```
//示例：
router.beforeEach((to, from, next) => {
  console.log('路由全局勾子：beforeEach -- 有next方法')
  next()
})
```
一般在这个勾子的回调中，对路由进行拦截。
比如，未登录的用户（或者是未绑定手机号的用户），直接进入了需要登录才可进的页面，那么可以用next(false)来拦截，使其跳回原页面或者跳到登录页，值得注意的是，如果没有调用next方法，那么页面将卡在那。
>next的四种用法：
1.next() 跳入下一个页面
2.next('/path') 改变路由的跳转方向，使其跳到另一个路由
3.next(false)  返回原来的页面
4.next((vm)=>{})  仅在beforeRouteEnter中可用，vm是组件实例。

2> router.afterEach():
```
//示例
router.afterEach((to, from) => {
  console.log('路由全局勾子：afterEach --- 没有next方法')
})
```
在所有路由跳转结束的时候调用，和beforeEach是类似的，但是它没有next方法。

组件路由钩子:
和全局钩子不同的是，它仅仅作用于某个组件，一般在.vue文件中去定义。
1>beforeRouteEnter:
```
//示例
  beforeRouteEnter (to, from, next) {
    console.log(this)  //undefined，不能用this来获取vue实例
    next(vm => {
      console.log(vm)  //vm为vue的实例
      console.log('组件路由勾子beforeRouteEnter的next')
//这个里面的代码是很晚执行的，在组件mounted周期之后,beforeRouteEnter的代码很早执行的，在组件beforeCreate之前；但是next里面回调的执行，很晚，在mounted之后，可以说是离dom渲染最近的一个周期。
    })
  }
```
beforeRouterEnter在组件创建之前调用，所以它无法直接用this来访问组件实例。但是可以给next传一个回调，回调的第一个参数即是组件实例，可以利用这点，对实例上的数据进行修改，调用实例上的方法。可以在这个方法去请求数据，在数据获取到之后，再调用next就能保证进页面的时候，数据已经获取到了。没错，这里next有阻塞的效果。你没调用的话，就会一直卡在那。

3>beforeRouteLeave:
```
  beforeRouteLeave (to, from, next) {
    console.log(this)    //可以访问vue实例
    next();
  },
```
在离开路由时调用。可以用this来访问组件实例。但是next中不能传回调。
4>beforeRouteUpdate:
```
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是改组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候调用，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  }
```
这个方法是vue-router2.2版本加上的。因为原来的版本中，如果一个在两个子路由之间跳转，是不触发beforeRouteLeave的。这会导致某些重置操作，没地方触发。在之前，我们都是用watch $route来hack的。

自定义指令的钩子函数：
绑定自定义指令的时候也会有对应的周期。
1>bind:
只调用一次，指令第一次绑定到元素时调用。
用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。

2>inserted:
被绑定元素插入父节点时调用（父节点存在即可调用，不必存在document 中）实际上是插入vnode的时候调用。

3>update:
所在组件的 VNode 更新时调用，但是可能发生在其孩子的 VNode 更新之前，（被绑定元素所在的模板更新时调用），不论指令的值（绑定值）是否变化。通过比较更新前后的值来忽略不必要的模板更新 。
注意，如果在指令里绑定事件，并且用这个周期的，记得把事件注销。

4>componentUpdated:
所在组件的 VNode 更新时调用，但是可能发生在其孩子的 VNode 更新之前。
被绑元素所在模板完成一次更新周期时调用。

5>unbind:
只调用一次，指令与元素解绑时调用。

Vue.nextTick、vm.$nextTick:
```
//示例：
  created () {
    this.$nextTick(() => {
      console.log('nextTick')  //回调里的函数一直到真实的dom渲染结束后，才执行
    })
    console.log('组件：created')
  },
```
nextTick方法的回调会在dom更新后再执行，因此可以和一些dom操作搭配一起用，如 ref。
>应用场景：
你用ref获得一个输入框，用v-model绑定。
在某个方法里改变绑定的值，在这个方法里用ref去获取dom并取值，你会发现dom的值并没有改变。
因为此时vue的方法，还没去触发dom的改变。
因此你可以把获取dom值的操作放在vm.$nextTick的回调里，就可以了。

VNode图示：
![enter image description here](https://sfault-image.b0.upaiyun.com/339/610/3396104381-5899cba0185f2_articlex)
>VNode可以理解为vue框架的虚拟dom的基类，通过new实例化的VNode大致可以分为几类: 
EmptyVNode: 没有内容的注释节点
TextVNode: 文本节点
ElementVNode: 普通元素节点
ComponentVNode: 组件节点
CloneVNode: 克隆节点，可以是以上任意类型的节点，唯一的区别在于isCloned属性为true
...
 
2.vue-cli脚手架
2.1 安装vue-cli
1>在终端执行: npm install -g vue-cli;
2>安装完成之后创建自己的项目:
```
 vue init <template-name> <project-name>
```
 
 例如：vue init webpack myProject 会提示创建的项目名字，作者，是否需要安装vur-router等 根据自己需求而定
3>创建完成后，到新建的项目文件中 cd myProject; 下载所有需要的依赖：npm install; 依赖安装好后；就可以运行项目了：npm run dev
其实npm run dev执行的是package.json文件里的这段命令中的'dev'：
```
"scripts": {
    "dev": "node build/dev-server.js",
    "start": "node build/dev-server.js",
    "build": "node build/build.js"
  },

```
3.学习如何搭建一个项目
3.1 搭建项目目录结构 安装依赖
1>新建文件夹demo后，cd demo;
2>执行npm init -y 会在该文件夹下面生成一个package.json文件;
3>然后可以执行npm install <依赖> --save-dev;
4>在demo文件下新建一个src文件夹和一个index.html文件；
```
index.html: 
<body>
	<div id="app"></div>
</body>
```
5>在src文件夹下新建app.vue文件、入口js文件main.js、components文件等;
app.vue:
```
<template>
	<div class="demo">
		<h1>vue学习之路</h1>
	</div>
</template>
<script>
	export default {
		name: '',
		data (){
			return {}
		},
		components: {
   //       加载的各种组件
		},
		mounted (){
			// 钩子函数
		},
		methods: {
        // 各种方法
		},
		computed: {
		// 计算值
		}
	}
</script>
<style lang="css"></style>
```

```
//main.js:
import Vue from 'vue'
import App from './App'-> app.vue文件的路径
// 创建一个vue实例 注意大小写 new Vue
new Vue({
	el: '#app',
	template: '<App/>',
	components: {App}
})

```
关于组建的学习在后面
3.2 配置webpack
1>在demo文件下面新建一个build文件夹;
2>在build文件下新建一个webpack.base.cont.js文件
```
'user strict';
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
	entry: 'entry',
	// entry就是入口文件 制定main.js为入口js,就是entry: 'main.js的路径',
	output: {
	// 一些输出配置
		path: path.resolve(_dirname, 'outputPath'),
		firename: 'firename', // 例如 '[name].[hash:8].js'
		publicPath: 'path', // 例如'static/''
	},
	module: {
		rules: {
			// 就是解析模块的一些规则 
			{
				test: /\.vue$/,
				loader: 'vue-loader', //使用vue-loader解析.vue的文件
			},
			{
				test: /\.js$/,
				loader: 'babel-loader?presets=es2015',//使用babel-loader解析.js的文件
				exclude: /node_modules/
			}
		}
	},
	resolve: {
		extensions:['.js','.vue'],
		alias:{
			'vue$':'vue/dist/vue.esm.js'
		}
	},
	plugins: {
		new HtmlWebpackPlugin({
			firename: 'index.html',
			template: path.resolve(__dirname,'../index.html')
		})
	}
}
```
3>配置好webpack.base.cont.js文件后，在终端执行webpack --config/webpack.base.cont.js 命令；在相应文件下会生成解析好的文件；但是每次文件不能做到实时编译
4.实时编译

5 总结一些vue知识
1>阻止事件冒泡：
```
@clcik.stop="" (vue提供)| event.cancelBubble = true(事件对象event自带)
```

2>阻止默认事件 :
 例如@contextmenu 浏览器会弹出操作菜单 @contextmenu.prevent="" (vue提供) | event.preventDefault() (事件对象event自带)
click.prevent()="clcikFun()"

注意在使用event时，@click="fun(\$event)",传入$event。

3>:class和:style
:class="['blue']"； :style="['margin': 20 + 'px']"或者:style="json格式的值"
:style 使用复合样式时，使用驼峰命名

7 vue遍历问题
由于 JavaScript 的限制， Vue 不能检测以下变动的数组：
```
// 当你利用索引直接设置一个项时，例如： vm.items[indexOfItem] = newValue
// 当你修改数组的长度时，例如： vm.items.length = newLength
```
[] instanceof(Array) = true
array.constructor == Array
★ 需要了解的是由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除：
对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性
为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果，同时也将触发状态更新：
```
// Vue.set （可以设置某个值也能让对象添加属性）：
// Vue 指全局的vue实例，在使用的时候 用import先引用vue；
// indexOfItem指index 如果是对象，存在key值 则indexOfItem可以用key代替;
// example1.items 指 Object | Array
Vue.set(example1.items, indexOfItem, newValue) 
// Array.prototype.splice
example1.items.splice(indexOfItem, 1, newValue)
```
```
<body>
	<ul>
		<li v-for="item in arr"></li>
	</ul>
</body>
<script>
	var vm = new Vue({
		data (){
			arr: ['aaa','bbb','ccc'],
			obj: {
				'aaa': 111,
				'bbb': 222,
				'ccc': 333
			}
		}
	}) 
	vm.arr[0] = 'ddd'; // 不会触发视图更新
	Vue.set(this.arr,0,'ddd'); // 会触发视图更新
	vm.$set(this.arr,0,'ddd'); // 会触发视图更新
	// 如果使用局部实例vm
	Vue.set(this.obj,'aaa',444); // 会触发视图更新
	vm.$set(this.obj,'aaa',444); // 会触发视图更新
</script>

```
如果属性是js动态添加的，set改变后，视图不会更新问题
```
<script>
	export default {
		name: '',
		data (){
			obj: {}
		},
		methods: {
			changeProp (){
				this.obj.aaa = 222; //视图不会更新为222，依然还是111
			},
			addProp (){
				this.obj.aaa = 111; // js动态添加上一个aaa的属性
			}
		}
	}
</script>
```		
解决方案：
```
// 1/ 改变完 值之后 改变一下对象
	changeProp (){
		vm.$set(this.obj,'aaa',222); //视图不会更新为222，依然还是111
	},
	addProp (){
		this.obj.aaa = 111;
	}
// 2/添加属性的时候 通过vm.$set()添加,改变时 通过vm.$set()改变
methods: {
	changeProp (){
		vm.$set(this.obj,'aaa',222); //视图不会更新为222，依然还是111
	},
	addProp (){
		this.obj.aaa = '111'; //  通过.的方式加载属性在obj对象上
		vm.$set(this.obj,'aaa',111);
	}
}

```		






