---
title: 使用Hexo踩过的坑
date: 2019-10-16 15:54:46
description: hexo
categories:
- hexo
tags: 
- hexo
toc: true 文章目录
---

+ 整理在使用hexo建立个人博客过程中遇到的问题
<!-- more -->
<The rest of contents | 余下全文>

### 在node14+版本下hexo部署失败

终端输入`node -v`查看 node版本：`v14.2.0`

hexo 版本信息：
![](/images/hexo-node14-error.png)

执行`hexo d -g`进行部署的时候，终端报错：

![](/images/hexo-deploy-error.png)

google一查发现 hexo 貌似不支持node14+，切换成低版本的 node 来使用 Hexo 就可以了，也可以升级 hexo 版本为 hexo@4.2.1+

因为精力有限，直接降低了node版本，有时间尝试升级 hexo@4.2.1+ 看下

然后重新执行 `hexo clean && hexo deploy` 打包命令就好了
