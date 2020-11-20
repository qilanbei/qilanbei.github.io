---
title: Vue利用keepAlive实现页面缓存
date: 2019-06-30 15:47:34
categories:
- Vue
tags: 
- Vue 页面缓存
toc: true 文章目录
---

+ keepAlive + vuex

<!-- more -->
<The rest of contents | 余下全文>

### 写在前面
在自己做项目的过程当中，遇到这样的需求：
当从列表页进入详情页，再返回到列表页的时候，保存列表页进入详情页之前的分页数据、筛选数据等，这样可以提高用户体验

起初我是利用h5的localStorage进行存储的，进入详情页之前，在localStorage保存列表页数据，后来觉得这个方法一点也不高大上，便查阅了以下资料：

在Vue中，对于这种“页面缓存”的需求，我们可以使用[keep-alive](https://cn.vuejs.org/v2/guide/components-dynamic-async.html)组件来解决

### 什么是 keep-alive
在Vue构建的单页面应用（SPA）中，路由模块一般使用vue-router。vue-router不保存被切换组件的状态，它进行push或者replace时，旧组件会被销毁，而新组件会被新建，走一遍完整的生命周期,

而 `keep-alive` 可以使被包含的组件状态维持不变，即便是组件切换了，其内的状态依旧维持在内存之中。在下一次显示时，也不会重现渲染

> `<keep-alive>` 与 `<transition>` 相似，只是一个抽象组件，它不会在DOM树中渲染(真实或者虚拟都不会)，也不在父组件链中存在，比如：你永远在 this.$parent 中找不到 keep-alive

### props：
 - include: 字符串或正则表达式。只有匹配的组件会被缓存。
 - exclude: 字符串或正则表达式。任何匹配的组件都不会被缓存。
 
### <keep-alive> 用法 

在 route 表里面设置 `meta.keepAlive` 用来标记哪些是需要缓存的页面

```javascript
// router.js:
export const routers = [
    {
        path: 'list',
        name: 'list',
        component: List,
        meta: {
            keepAlive: true
        }
    },
    {
        path: 'list/:id',
        name: 'listDetail',
        component: listDetail,
        meta: {}
    }
]
```

```
// App.vue:
<keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

有人这样处理，但是我这样使用了之后，还是有很多漏洞，比如：从不需要缓存的页面到需要缓存的列表页（这时候$route.meta.keepAlive是false），
进入列表详情页，再回来第一次并不会缓存（因为进入之前是false 所以没有缓存），
再进入详情页才可以（此时$route.meta.keepAlive才是true）等问题

上面这种方案我没有成功，有兴趣的朋友可以试试

#### 所以我接着查阅发现下面这种方案，`<keep-alive>` 结合 vuex 状态存储

1. 在 router 表里定义 `meta.keepAlive` 标记需要缓存的页面

2. 在 App.vue 页面，利用 `<keep-alive>` 的 `include` 参数 这样写：

```
<keep-alive :include="cachedViews">
	<router-view :key="key"/>
</keep-alive>

// cachedViews 利用vuex 状态存储管理
computed:{
  ...mapState({
	  cachedViews: state => state.tagsView.cachedViews
  })
}
```

* 关于mapState 是vuex辅助函数之一，不熟悉的可以自行google 还有mapActions, mapMutations用法

3. 而这个 `cachedViews` 数组需要利用vuex状态存储管理

```javascript
// 新建 tagViews.js 文件
export const tagViews = {
    state: {
        cachedViews: [] // 保存需要缓存的页面，也就是 <keep-alive :include="cachedViews"> 这里使用的参数
    },
    mutations: {
        // 这里缓存的是 route.path 也是可以的，我使用的是route.name
        ADD_CACHED_VIEW (state, view) {
            if (state.cachedViews.includes(view.name)) return
            if (view.meta.keepAlive) {
                state.cachedViews.push(view.name)
            }
        },
        DEL_CACHED_VIEW (state, view) {
            let index = state.cachedViews.indexOf(view.name)
            index > -1 && state.cachedViews.splice(index, 1)
        }

    },
    actions: {
        addCachedView ({ commit }, view) {
            commit('ADD_CACHED_VIEW', view)
        },
        delCachedView ({ commit }, view) {
            commit('DEL_CACHED_VIEW', view)
        }
    }
}
```

这样就可以实现了，是不是还是很简单的，但是这里面有几个小坑，其实有时候入坑也是好的，会让你更深入的理解一个知识点

* router表里面定义的 name 必须和组件页面 export 出来的 name 一致

```
// routers.js
export const routers = [
    {
        path: 'list',
        name: 'list',
        component: List,
        meta: {
            keepAlive: true
        }
    }
]
// list.vue
<template> 
	list
</template>
<script>
export default {
	name: 'list', // 也就是此处的name 必须和router定义里的name一致
	data () {
		return {}	
	}
}
</script>
```

* 使用了 keep-alive 之后 include 进行了缓存的页面，生命周期发生了变化：

没有进行缓存的页面，生命周期为：created mounted destroyed

进行了缓存的页面，生命周期变成：第一次进入，

created mounted activated deactivated

第二次进行了缓存之后：activated deactivated 这时就不在经历 created mounted 周期

如果有必须每次进入必须加载的数据 可以利用 activated 这个生命周期做










