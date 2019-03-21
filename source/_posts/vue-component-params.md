---
title: vue父子组件间数据传递
date: 2016-08-26 00:41:29
description: vue父子组件间怎么进行数据传递
categories:
- Vue
tags: 
- vue父子组件间数据传递
toc: true 文章目录
---
+ vue父组件和子组件间怎么进行数据传递
<!-- more -->
<The rest of contents | 余下全文>

★ 父组件传递数据给子组件(可以通过props属性来实现)：
由于组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。要让子组件使用父组件的数据，我们需要通过子组件的 props 选项；
在模板中，要动态地绑定父组件的数据到子模板的 props，与绑定到任何普通的 HTML 特性相类似，就是用 v-bind。每当父组件的数据变化时，该变化也会传导给子组件：
```
//父组件
//引入的add-widget组件
//使用 v-bind 的缩写语法通常更简单：
<add-widget :msg-val="msg"> //这里必须要用 - 代替驼峰
// HTML 特性是不区分大小写的。所以，当使用的不是字符串模板，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名，当你使用的是字符串模板的时候，则没有这些限制 
</add-widget>
data(){
    return {
        msg: [1,2,3]
    };
}
//如果你想要用一个对象作为 props 传递所有的属性，你可以使用不带任何参数的 v-bind (即用 v-bind 替换掉 v-bind:prop-name)。例如，已知一个 todo 对象：
todo: {
  text: 'Learn Vue',
  isComplete: false
}
//模板里：
<todo-item v-bind="todo"></todo-item>
//等价于：
<todo-item
  v-bind:text="todo.text"
  v-bind:is-complete="todo.isComplete"
></todo-item>

//子组件通过props来接收数据：
//方式1：
props: ['msgVal']
//方式2 :
props: {
    msgVal: Array //这样可以指定传入的类型，如果类型不对，会警告
}
//方式3：
props: {
    msgVal: {
        type: Array, //指定传入的类型
        //type 也可以是一个自定义构造器函数，使用 instanceof 检测。
        default: [0,0,0] //这样可以指定默认的值
    }
}
//注意 props 会在组件实例创建之前进行校验，所以在 default 或 validator 函数里，诸如 data、computed 或 methods 等实例属性还无法使用
```
★ 子组件与父组件通信
//由于prop是单向数据传递的，为了防止子组件无意修改了父组件的状态，所以子组件想要给父组件传递数据，vue是不允许的，这时候我们可以通过触发事件来通知父组件改变数据，从而达到改变子组件数据的目的
```
//子组件:
<template>
    <div @click="up"></div>
</template>
methods: {
    up() {
        this.$emit('upupEvent','hehe'); //主动触发upupEvent方法，'hehe'为向父组件传递的数据
    }
}
```
