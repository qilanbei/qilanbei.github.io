---
title: nginx使用笔记
date: 2019-04-16 19:16:15
categories:
- nginx
tags: 
- nginx使用笔记
toc: true 文章目录
---
+ Mac OS系统下，Nginx的配置和常用命令
<!-- more -->
<The rest of contents | 余下全文>

## 为什么选择Nginx

Nginx 是一个高性能的 Web 和反向代理服务器, 它具有有很多非常优越的特性:

**作为 Web 服务器**：相比 Apache，Nginx 使用更少的资源，支持更多的并发连接，体现更高的效率，这点使 Nginx 尤其受到虚拟主机提供商的欢迎。能够支持高达 50,000 个并发连接数的响应，感谢 Nginx 为我们选择了 epoll and kqueue 作为开发模型.

**作为负载均衡服务器**：Nginx 既可以在内部直接支持 Rails 和 PHP，也可以支持作为 HTTP代理服务器 对外进行服务。Nginx 用 C 编写, 不论是系统资源开销还是 CPU 使用效率都比 Perlbal 要好的多。

**作为邮件代理服务器**: Nginx 同时也是一个非常优秀的邮件代理服务器（最早开发这个产品的目的之一也是作为邮件代理服务器），Last.fm 描述了成功并且美妙的使用经验。

Nginx 安装非常的简单，配置文件 非常简洁（还能够支持perl语法），Bugs非常少的服务器: Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够在 不间断服务的情况下进行软件版本的升级。

## 正向代理与反向代理

代理是在服务器和客户端之间假设的一层服务器，代理将接收客户端的请求并将它转发给服务器，然后将服务端的响应转发给客户端

**正向代理**

是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。

客户端可以根据正向代理访问到它本身无法访问到的服务器资源。

正向代理对我们是透明的，对服务端是非透明的，即服务端并不知道自己收到的是来自代理的访问还是来自真实客户端的访问

**反向代理**

是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

反向代理可以帮助服务器接收来自客户端的请求，帮助服务器做请求转发，负载均衡等。

反向代理对服务端是透明的，对我们是非透明的，即我们并不知道自己访问的是代理服务器，而服务器知道反向代理在为他服务



## 安装

mac 必备软件包管理工具[homebrew](https://brew.sh/)，通过brew可以很方便的在Mac中安装软件或者是卸载软件.

常用指令如下:
- brew 搜索软件:  
`brew search nginx`

- brew 安装软件: 
 `brew install nginx`

- brew 卸载软件: 
`brew uninstall nginx`

- brew 升级: 
`brew update`

- 查看安装信息(经常用到, 比如查看安装目录等): 
`brew info nginx`

- 查看已经安装的软件: 
`brew list`

#### brew安装nginx
```angular2html
brew install nginx
```

安装完以后，可以在终端输出的信息里看到一些配置路径：

`/usr/local/etc/nginx/nginx.conf` - 配置文件路径

`/usr/local/Cellar/nginx/1.15.9` - 安装路径

nginx默认使用8080端口 如果发现端口被占用了，可以杀掉使用使用改端口的进程，也可以修改/usr/local/etc/nginx/nginx.conf 下的:

```angular2html
http {
    server {
        listen       8080; 
        <!--8080端口被占用，此处的listen的端口配置 可以改为8081或者其他-->
        server_name  localhost;  
        #charset koi8-r;
        .....
        }
    }
```

测试nginx是否安装成功，因为默认配置文件监听的是8080端口，所以先对8080端口发起请求：

`curl -ILhttp://127.0.0.1:8080`

curl 命令见 [分享一些实用的Linux技巧文章总结的 - curl 命令](http://localhost:4000/2019/03/27/linux-skill/)

如果安装成功，打开浏览器输入：localhost:8080 会有一个默认欢迎界面

![](/images/nginx-1.png)

## 基本配置

- main:nginx的全局配置，对全局生效。
- events:配置影响nginx服务器或与用户的网络连接。
- http：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。
- server：配置虚拟主机的相关参数，一个http中可以有多个server。
- location：配置请求的路由，以及各种页面的处理情况。
- upstream：配置后端服务器具体地址，负载均衡配置不可或缺的部分。


**nginx常用命令：**
- 打开 nginx
`nginx 或者 sudo nginx`

- 关闭nginx服务
`sudo brew services stop nginx`

- 重新启动nginx
`nginx -s reload`

- 停止nginx
`nginx -s stop`

- 查看nginx进程
`ps -ef|grep nginx`

这段命令可以查看本身的进程

ps 命令见 [分享一些实用的Linux技巧文章总结的 - ps 命令](http://localhost:4000/2019/03/27/linux-skill/)

## nginx 代理本地到服务器接口

查看配置文件 /usr/local/etc/nginx/nginx.conf

https server设置

```javascript 1.8

server {
	server_name   ds.local;

	location / {
		root path;
		// path: 需要代理的项目目录
		try_files $uri /index.html;
		// 自动找index.html 入口文件
		index index.html;
	}

	location "/api/1" {
		proxy_pass   URL
		// "/api/1"： 为接口前缀
		// URL: 需要代理的服务器地址
	}
}

```

如果有多个项目想要ngin代理

配置文件 /usr/local/etc/nginx/nginx.conf 中 https server设置：

```javascript 1.8
// 将上述https server设置 注释 或者删除，修改为：

include servers/*;
```

然后 /usr/local/etc/nginx 路径下新建 servers 文件夹

比如project1 和 project2 需要代理

则在servers 文件夹 新建文件 host.project1.conf 和 host.project2.conf

然后每个项目的nginx文件添加相应的配置

host.project1.conf 文件：
```javascript 1.8
server {
	server_name   project1.local;

	location / {
		root /project1/path;
		// path: 需要代理的项目目录
		try_files $uri /index.html;
		// 自动找index.html 入口文件
		index index.html;
	}

	location "/api/1" {
		proxy_pass   URL
		// "/api/1"： 为接口前缀
		// URL: 需要代理的服务器地址
	}
}
```



host.project2.conf 文件：
```javascript 1.8
server {
	server_name   project2.local;

	location / {
		root /project2/path;
		// path: 需要代理的项目目录
		try_files $uri /index.html;
		// 自动找index.html 入口文件
		index index.html;
	}

	location "/api/1" {
		proxy_pass   URL
		// "/api/1"： 为接口前缀
		// URL: 需要代理的服务器地址
	}
}
```
