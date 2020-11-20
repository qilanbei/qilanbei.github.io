---
title: 如何发布自己模块到 NPM
date: 2019-04-17 18:06:00
categories:
- 前端辅助工具
tags: 
- NPM
toc: true 文章目录
---
+ 手把手教你用npm发布一个package
<!-- more -->
<The rest of contents | 余下全文>

## npm包管理

npm其实是Node.js的包管理工具（node package manager）

在我们工作开发当中，为了开发效率，会用到很多开源的代码，大家也都把自己开发的模块打包后放到[npm官网](https://www.npmjs.com/)上，如果要使用，直接通过npm安装就可以直接用

而且npm可以根据依赖关系，把所有依赖的包都下载下来并管理起来。不然，靠我们自己手动管理会很繁琐

## 准备工作

去[官网](https://www.npmjs.com/)注册一个属于自己的账号

安装nodejs后 自动就有npm了，请自己google安装配置吧

Tip: 关于node版本管理工具，除了[nvm](https://github.com/creationix/nvm)  还有个工具叫做 [n](https://github.com/tj/n) , 对，人家就叫这个名字，简单时尚

## npm的基础使用

如果之前你设置了淘宝镜像

将npm包的镜像换成  https://www.npmjs.com/

`npm config set registry https://www.npmjs.com/`

由于我们这里是往 Npm 仓库 发布包的，这里设置的下载地址是 淘宝镜像仓库，二者不一致会导致错误

如果这个官方npm加载速度慢，之后再切换回来

`npm config set registry https://registry.npm.taobao.org`


总结一些常用的npm指令：

- login: 登录npm

- access：设置已发布包的访问级别

- adduser： 添加一个注册用户账号

- audit：运行一个安全审核

- bin： 显示bin文件夹

- build：Build 一个包

- cache: 操作程序包缓存

- publish: 发布一个package

- updated: 更新一个package

- unpublish: 撤销一个package

[更对命令操作查看官网](https://docs.npmjs.com/cli-documentation/cli)

## 如何开发一个package

**1. 初始化项目**

**新建目录**

`mkdir packageDemo && cd packageDemo`

**生成package.json文件**

`npm init`

本地通过npm init 来创建 package.json 文件，需要数据一些模块的信息，如名称、版本、描述、作者、license等

具体的目录结构如下：

```javascript
.
├── .eslintrc.json   // eslint 文件
├── .git
├── .gitignore      // .gitignore 可忽略上传文件和一些默认的编辑器配置文件
├── .npmignore      // .npmignore 不需要publish的文件
├── .travis.yml     // travis CI
├── LICENSE
├── README.md
├── index.js       // 入口文件
├── lib            // 主要逻辑代码
├── node_modules
├── package-lock.json
├── package.json
└── test          //测试用例
```

**开发自己的功能**

**根据需要增加 eslint 和 代码检查**

## 发布一个 package 到 npm

**登录npm**

首先，我们需要到 [npm官网](https://www.npmjs.com/) 注册一个账号。然后添加到本地配置

`npm adduser`

```javascript
npm adduser
Username:
password:
Email: 
```

为了安全起见，输入密码不会显示任何东西，你只需输入完成按回车即可，不用担心自己是否没有输入上

然后验证自己的配置: 
`npm whoami`

如果已经添加过，直接login

`npm login`

```javascript
npm login
Username:
password:
Email: (this IS public) 
```

输入自己在npm上面的用户名、密码、邮箱，完成登录之后，显示：
![](/images/npm-1.png)


**发布公有npm包**

项目完成之后，我们需要将模块发布到npmjs

最后，在package.json 定义好版本，通过 npm publish 发布模块即可

```javascript
npm publish

//返回下面的信息就是发布成功了 
+ packageName@0.0.1
```

- 发布过程会把整个目录发布,不想发布的内容模块,可以通过 .gitignore 或 .npmignore 文件忽略

- 发布成功后 你的包名就是 package.json中的name

- 发布成功之后可以去npm官网搜索一下是否已经发布成功到npm了

**发布组织下的一个package**

首先，你要被加到一个组织下，如图所示：

![](/images/npm-2.png)

package.json上面的name 需要要这样命名：

`@组织名称/包名称`

[scope 是 npm 的新特性](https://www.npmjs.com.cn/misc/scope/)。如果一个模块的名字以 "@" 开始，那么他就是一个scoped package。scope 就是"@"与"/"之间的部分。

```javascript 1.8
1. 当你注册私有模块到一个组织下时，你的 scope 就是当前组织名

@organizationName/packageName

2. 当你注册私有模块到一个用户下时，你的 scope 就是当前用户的用户名

@username/packageName

3. 如果要使用 npm init 初始化一个软件包，你可以通过自定义 --scope 选项设置你的 scope

npm init --scope=<your_scope>

4. 如果你在大多数时候使用的 scope 都是相同的，可以设置一个默认的scope，这样在我们初始化的时候会自动使用该 scope

npm config set scope <your_scope>
```

这样发布的时候，就属于组织下的package了

发布，要告诉这是一个组织下的公有包

`npm publish --access public`

**发布私有包**

默认情况下，scoped package 会发布为私有模块，发布为私有模块是需要付费的，费用是每个月 $7 

一旦完成发布，你将会在npm库站点上看到你的 scoped package，有 private 标志，说明是非公共的模块，他人无法使用。

如果你要授权给其他人使用你的模块，你可以在 package 的权限设置页面设置哪些用户可以拥有 只读或读写 权限。也可以通过命令行进行相关设置

`npm owner add <user> <package name>`

**如何更新npm包**

###### 使用命令行 `npm version patch` 更新package.json里面的版本号

npm version 0.0.2 //package.json修改为 0.0.2版本

[ 注：patch会自动更新package.json里面的版本号 ]

使用 npm publish,更新就会完成

npm version后面参数说明：

```
patch：小变动，比如修复bug等，版本号变动 v0.0.1 -> v0.0.2
minor：增加新功能，不影响现有功能,版本号变动 v0.1.0->v0.2.0
major：破坏模块对向后的兼容性，版本号变动 v1.0.0->v2.0.0
```

**使用npm包**

1. 下载公有模块

通过 `npm install packageName` 下载想要的package

下载之前，也可以通过 `npm search packageName` 查询一下

2. 使用更新后的NPM包

```javascript 

针对patch： npm install packageName
针对minor： npm install packageName
针对major： npm install packageName@2.0.0

```

3. 下载私有模块

`npm install @scope/packageName`

## 总结

发布流程应该是这样的：

1. 第一次发布

- 创建目录，使用 `npm init` 添加 package.json 文件，开发自己的功能

- 执行 git push origin master，将改动同步到远程代码仓库

- 开发完成后，在本地添加用户配置 `npm adduser`

- 定义好 package.json 文件中的版本号version

- 根据自己需求发布到相应的公有私有模块，执行 npm publish ，将模块发布到 npm 仓库

2. 更新发布

- 根据此次变更执行 npm version [new version] 命令（npm 会根据 new version 指定的类型更新 package.json 中的 version 字段，然后进行一次 commit
  
- 执行 git push origin master，将改动同步到远程代码仓库

- 开发完成后，登录 npm  `npm login`
  
- 根据自己需求发布到相应的公有私有模块，执行 npm publish ，将模块发布到 npm 仓库
