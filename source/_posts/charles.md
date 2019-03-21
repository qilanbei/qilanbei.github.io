---
title: 有关Charles抓包
date: 2019-02-22 17:56:14
description: Charles抓包工具的破解和 Https 配置
categories:
- 前端工具
tags: 
- Charles
toc: true 文章目录
---

+ Charles抓包工具的破解和 Https 配置

<!-- more -->
<The rest of contents | 余下全文>



#### 写在前面
由于之前查了好多无良的帖子，看了那么多字，发现好几篇都是一样的，有的复制的还不是很全，真的很坑，emmmm.....

#### Charles 软件破解方式
1. 去[官网](https://www.charlesproxy.com/download/) 下载适合自己系统的版本并安装
2. 去[破解网站](https://www.zzzmode.com/mytools/charles/)根据自己Charles的版本下载对应的jar包
3. 简单的替换方法：mac版本在Applications(应用程序)里找到Charles.app然后右击，选择show package content(显示包内容)，找到Contents/Java的路径，如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190314111100438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
将刚才下载的charles.jar包替换Contents/Java中的charles.jar，很简单的操作了吧

Windows版本替换方式同mac，只是路径为: C:\Program Files\Charles\lib\charles.jar
这时候你就能够美美的使用破解版了，不用每过30分钟就提示你退出了

#### Charles https 抓包
###### 1. pc 浏览器 https 抓包,
打开Charles 软件，下载Charles Root Certificate，勾选macOS Proxy
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019031411250517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
会弹出到钥匙串这里，如果证书没有被信任，双击Charles证书，设置Always Trust(始终信任)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190314112815810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
###### 2. 手机端 https抓包
下载移动端的证书，去掉macOS Proxy的选择
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190314114307249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
会弹出这个提示，让你用手机浏览器访问"chls.pro/ssl"，会下载一个证书安装就行了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190314114408221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
#### 拦截指定地址的数据包
选择proxy的Recording Setting 设置，找到include的tab,添加你想拦截的地址

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190314115058691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
#### 注意事项
1. 使用Charles的时候，尽量不要开小飞机翻墙
2. Chrome 和 Firefox 浏览器默认并不使用系统的代理服务器设置，而 Charles 是通过将自己设置成代理服务器来完成封包截取的，所以在默认情况下无法截取 Chrome 和 Firefox 浏览器的网络通讯内容
```
2.1. 访问: chrome://settings/
2.2. 然后下拉到最后的高级，下来在 “系统”（倒数第二个）的条目下找到“打开代理设置”
2.3. 然后双击打开之后，打开之后找到代理的 tab 点开，点开之后可以看到请选择一个协议进行配置，这个时候找到“网页代理(http)” 和“安全网页代理 (https)”，进行相应的配置就可以了，
2.4. 一般来说自己不做其他处理，直接配置代理服务器为“127.0.0.1”，端口(就是冒号:) 后是“8888”。
```
3. 有的提示这里也要注意，这个地方你开小飞机，就自动勾选，不开就不会勾选，所以用Charles的时候，尽量不要用小飞机翻墙了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190314115700109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbGFuYmVp,size_16,color_FFFFFF,t_70)
