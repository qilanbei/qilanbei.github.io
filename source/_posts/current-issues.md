---
title: 最近遇到的问题整理
date: 2019-07-01 15:47:34
categories:
- 总结
tags: 
- 问题总结
toc: true 文章目录
---

+ 最近遇到的问题整理

<!-- more -->
<The rest of contents | 余下全文>

1. 问题：jquery animation 方法 overflow:hidden 页面闪烁的问题 
解决：相应dom上设置overflow:visiable !important

2. 问题：element-ui 的 table组件 在 safari 浏览器上 列宽样式 错乱问题  
解决：因为table没有设置width: 100% !important 导致的精度问题）

3. 网页加载图片加载不出来
```
<meta name="referrer" content="same-origin">
<meta name="referrer" content="no-referrer">
```

4. 高德地图的问题
```
//使用loadUI
   AMapUI.loadUI(['overlay/SimpleMarker'], function(SimpleMarker){ ... });
   
   //如果使用load，路径要前缀补上'ui/'：
   AMapUI.load(['ui/overlay/SimpleMarker'], function(SimpleMarker){ ... });
```
