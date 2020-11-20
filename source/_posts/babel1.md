---
title: Babel学习之旅(一)
date: 2019-04-30 15:58:56
categories:
- Babel
tags: 
- Babel学习笔记
toc: true 文章目录
---
+ 记录有关babel的学习旅程
<!-- more -->
<The rest of contents | 余下全文>

## What is Babel?

> Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments

这是来自[babel官网](https://new.babeljs.io/docs/en/index.html)的介绍

翻译理解：Babel 是一个工具链，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中，即：ES6/ES7/ES8 => Babel => ES5。

举个栗子：
```javascript
// 转码前
input.map(item => item + 1)

// 转码后
input.map(function (item) {
  return item + 1
})

// 箭头函数这个特性，不是所有浏览器都支持，通过babel可以转码为普通函数，这样就不用担心浏览器厂商是否支持了

```
更多了解请移步这里：

[ babel 中文文档](https://www.babeljs.cn/)
[ babel 英文文档](https://new.babeljs.io/)

## 配置 Babel

Babel 也同ESLint (.eslintrc)、Prettier (.prettierrc)等一样，是有配置文件的.

所有 Babel API [参数](https://www.babeljs.cn/docs/options) 都可以被配置

目前为止通过运行 Babel 自己我们并没能“翻译”代码，而仅仅是把代码从一处拷贝到了另一处。原因就是从Babel 6以后, 默认的插件被移除, 如果没有指定一个插件，Babel将会原样输出, 不会进行编译。

你可以通过安装插件（plugins）或预设（presets，也就是一组插件）来指示 Babel 去做什么事情。

插件只是单一的功能：

- es2015-arrow-functions
  
- es2015-classes
  
- es2015-for-of
  
- es2015-spread

如果我们每个都要引人功能单一的插件的话特别麻烦，通常我们用的更多的是预设，插件和预设通常写入到配置文件中

#### 配置文件的类型：

Babel有两种并行的配置文件格式，可以一起使用，也可以单独使用。

- Project-wide configuration

- File-relative configuration
  - .babelrc (and .babelrc.js) files
  - package.json files with a "babel" key

#### 配置文件的选择:

- 你是否希望以编程的方式创建配置文件？
- 你是否希望编译 node_modules 目录下的模块？

> [babel.config.js](https://www.babeljs.cn/docs/configuration#babelconfigjs) 文件可以满足你的的需求！

- 你是否只是需要一个简单的并且只用于单个软件包的配置？

> [.babelrc](https://www.babeljs.cn/docs/configuration#babelrc) 文件适合你！

- The Guy Fieri is your hero?

> 官网建议使用 [babel.config.js](https://www.babeljs.cn/docs/configuration#babelconfigjs) 格式的配置文件。Babel 本身使用的就是这种。

#### babel.config.js

在项目的根目录（package.json 文件所在目录）下创建一个名为 babel.config.js 的文件，如下所示：
```javascript
module.exports = function (api) {
  api.cache(true);

  const presets = [];
  const plugins = [];

  return {
    presets,
    plugins
  };
}
```
请参阅 [babel.config.js](https://www.babeljs.cn/docs/config-files#project-wide-configuration) 文档 以了解更多关于配置参数的信息


#### .babelrc

在你的项目中创建名为 .babelrc 的文件，并输入以下内容:
```json
{
  // presets字段设定转码规则
  "presets": [],
  // plugins字段可以设置需要的插件
  "plugins": []
}
```

presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装:

```
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

# react转码规则
$ npm install --save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3

```

然后，将这些规则加入.babelrc

```json
{
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
}
```

请参阅 [.babelrc](https://www.babeljs.cn/docs/config-files#file-relative-configuration) 文档 以了解更多关于配置参数的信息。

#### package.json

或者，还可以选择将 .babelrc 中的配置信息作为 babel 键（key）的值添加到 package.json 文件中，如下所示：
```json
{
  "name": "my-package",
  "version": "1.0.0",
  "babel": {
    "presets": [],
    "plugins": []
  }
}
```

#### .babelrc.js

与 .babelrc 的配置相同，但你可以使用 JavaScript 编写
```javascript 1.8
const presets = [];
const plugins = [];

// 你还可以调用 Node.js 的任何 API，例如基于进程环境进行动态配置
if (process.env["ENV"] === "prod") {
  plugins.push(...);
}

module.exports = { presets, plugins };
```
