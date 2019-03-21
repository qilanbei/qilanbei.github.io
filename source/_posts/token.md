---
title: token相关
date: 2017-01-03 23:34:12
description: 关于token相关笔记
categories:
- JavaScript 小知识
tags:
- token相关
toc: true 文章目录
---

+ 关于token相关笔记
<!-- more -->
<The rest of contents | 余下全文>

### token相关
1.token的工作原理
1> 登录时候,客户端通过用户名与密码请求登录
2> 服务端收到请求区验证用户名与密码
3> 验证通过,服务端会签发一个Token,再把这个Token发给客户端.
4> 客户端收到Token,存储到本地,如Cookie,SessionStorage,LocalStorage.
5> 客户端每次像服务器请求API接口时候,都要带上Token.
6> 服务端收到请求,验证Token,如果通过就返回数据,否则提示报错信息.
2.token值得注意的事
1> token 过期时间为 30 分钟
2> Token 应该被保存起来（放到 local / session stograge 或者 cookies）
在单页应用程序中，有些用户刷新浏览器后会带来一些跟 token 相关的问题。而解决方法很简单：你应该把 token 保存到起来：放到 session storage, local storage 或者是客户端的 cookie 里。而浏览器不支持 session storage 时都应该转存到 cookies 里。在这种情况下要把 cookie 当作一个储存机制，而不是一种验证机制。
3> 登录时有关token的操作
3.1> ajax请求login接口的时候，接口会返回token和refreshToken信息，存储到localStorage中
```
// ajax回调函数中：
// data 为ajax请求回来的数据
let token = {
  token: data.token,
  refreshToken: data.refreshToken,
};
 window.localStorage.setItem('admintoken', JSON.stringify(token));
// JSON的一个常见用途是将数据与Web服务器进行交换。将数据发送到Web服务器时，数据必须是字符串。使用JSON.stringify（）将JavaScript对象转换为字符串。
 utils.setCookie('haslogin','1');
 //登录成功 记录haslogin为1；用来router.js设置未登录或者登录未成功设置路由跳转到登录页
 utils.setCookie('Token', token.token);
```
3.2> 封装setCookie()
此处有个时间转化的问题（待解决）
Date的方法：
例如当前时间为：2017/8/7 上午11:50:24
toGMTString()：此日期会在转换为字符串之前由本地时区转换为 GMT 时区 Mon, 07 Aug 2017 03:50:24 GMT（实际已经被toUTCString转化），
注意：不赞成使用此方法。在使用toGMTString时，最后会被toUTCString() 取而代之
toUTCString()：会将日期用世界时表示，Mon, 07 Aug 2017 03:50:24 GMT
toLocaleString()：根据本地时间格式，把 Date 对象转换为字符串，转化为当前时间2017/8/7 上午11:50:24
```
// expires 为token设置的过期时间
setCookie: function (key, value, expires) {
    var cValue = key + "=" + value;
    if (expires) {
      cValue += ";expires=" + expires;
    } else {
      let exp = new Date()
      exp.setTime(exp.getTime() + 55 * 60 * 1000);
// Date对象的getTime()方法可返回距 1970 年 1 月 1 日之间的毫秒数
// exp.getTime() + 55 * 60 * 1000 是为了设置一个token的过期时间
      cValue += ";expires=" + exp.toLocaleString();
    }
    document.cookie = cValue;
  },
```
3.3> 在设置路由的时候，判断是否登录了，未登录或者登录未成功跳转登录页
```
route.beforeEach((to, from, next)=>{
let loginInfo = utils.getCookie('haslogin');
	if(!loginInfo  &&  to.path !== '/login'){
		return next({
			name: 'login',
			// query: {redirect: to.fullPath}
		})
	}
	next()
})
```
3.前端拦截token
```
// 登录路由设置个字段,用来记录是否已经登录
path: '/login', name:'login',
    component: resolve => require(['./login.vue'], resolve),
    meta: { noCheckSession: true }

// 路由拦截(router.js)
route.beforeEach((to, from, next)=>{
	let loginInfo = utils.getCookie('haslogin');
	// 在登录成功的时候 接口返回token相关信息 将信息记录在cookie中
	if(!loginInfo && to.path !== '/login'){
		return next({
			name: 'login',
			// query: {redirect: to.fullPath} // 跳转到登录页面
		})
	}
	next()
})
```
#### token cookie session 区别
Local / session storage 不会跨域工作，请使用一个标记 cookie
保存在 local / session storage 的 tokens，就不能从不同的域名中读取（甚至是子域名也不行）
// Cookie的有效期操作

1. cookies 可以在浏览器关闭后删除（session cookies）；
2. 通过绝对有效期或弹性有效期（sliding window expiration）；
3. Cookies 可以通过携带有有效期地保存起来。
