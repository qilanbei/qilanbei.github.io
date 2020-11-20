---
title: vue项目中添加eslint
date: 2019-04-15 11:57:31
categories:
- Vue
tags: 
- eslint使用笔记
toc: true 文章目录
---
+ vue项目中eslint的使用
<!-- more -->
<The rest of contents | 余下全文>

## 写在前面

之前做项目一直不是很重视[eslint](https://cn.eslint.org/)的作用，后来随着项目的积累，觉得 eslint 是团队开发必备的工具，它能很好地减少代码错误、节约调试时间、维持团队成员之间代码风格的统一


## 关于eslint

[eslint英文文档 https://eslint.org/](https://eslint.org/)
[eslint中文文档 http://eslint.cn/](http://eslint.cn/)

ESLint最初是由Nicholas C. Zakas 于2013年6月创建的开源项目。它的目标是提供一个插件化的javascript代码检测工具。

[ESLint v5.0.0-alpha.0 released](https://cn.eslint.org/blog/2018/03/eslint-v5.0.0-alpha.0-released) 2018-03-30

这篇文章[This is a summary of the standard JavaScript rules](https://github.com/standard/standard/blob/master/RULES.md#javascript-standard-style)有详细介绍eslint的rules的使用和参数解释

[这里也有rules参数以及示例介绍](https://cloud.tencent.com/developer/doc/1078)

[learn more](https://cn.eslint.org/docs/about/)

## 安装
```angularjs
npm install eslint
```
## 配置

[详细配置介绍查看官网 configuring ](https://cn.eslint.org/docs/user-guide/configuring)

1. 在文件webpack.base.conf.js 的 rules 配置：

```angular2html
{
<!--使用eslint-loader处理vue和js文件-->
  test: /\.(js|vue)$/,
  loader: 'eslint-loader',
  enforce: 'pre',
  include: [resolve('src'), resolve('test')],
  options: {
	  formatter: require('eslint-friendly-formatter')
  }
}

```
2. 可以通过以下三种方式配置 ESLint:
- 在 package.json 中添加 eslintConfig 配置块
- 直接在代码文件中定义, 代码文件内以注释配置的规则会覆盖配置文件里的规则，即优先级要更高
- 使用 .eslintrc 文件（支持 JSON 和 YAML 两种语法）

3. package.json文件配置了lint

```angular2html
"lint": "eslint --ext .js,.vue src"
```

4. 可以通过npm run lint 进行eslint的检测

### eslint  报错 问题定位：

```angular2html
1. 问题：Parsing error: Unexpected token ...

原因：是使用了es6的扩展运算符...报错

解决：.eslintrc.js文件设置如下：
"ecmaFeatures": { 
  "experimentalObjectRestSpread": true  
}

.eslintrc.yml文件设置如下：
ecmaFeatures: 
  experimentalObjectRestSpread: true  


2. 问题：Missing semicolon  

原因：查看semi分号设置，设置了always 丢失分号就会报错

解决：根据提示位置，加上分号，如果不想在文末添加分号 就这样设置rules的semi：

"semi": [2, "never"]

3. 问题: Expected indentation of 16 spaces but found 

原因：都是缩进的问题 

解决：查看indent 缩进风格配置

"indent": [2, 4]

4. 问题: <template>  Unexpected token <

原因：The most rules of eslint-plugin-vue require vue-eslint-parser to check <template> ASTs.

解决：下载 npm i eslint-plugin-vue  
在.eslitrc 设置："extends": ["plugin:vue/base"]

根据报错提示定位相应问题，配置相应eslint的rule

```

以下是.eslintrc 文件示例:

默认情况下，ESLint 支持 ECMAScript 5 语法

```angular2html
{
	"env": {},
	"ecmaFeatures": {},
	"rules": {}
}
```
## eslint 配置参数简述


#### env - 指定脚本的运行环境。每种环境都有一组特定的预定义全局变量 常见配置项详情：

| 参数 | 描述 |
| :----- | :----- |
| "browser": true| 浏览器环境中的全局变量 |
| "node": true| Node.js 全局变量和 Node.js 作用域 |
| "commonjs": true| CommonJS 全局变量和 CommonJS 作用域 (用于 Browserify/WebPack 打包的只在浏览器中运行的代码) |

更多 env 配置[请查阅这里](https://cn.eslint.org/docs/user-guide/configuring#specifying-environments)

#### Globals - 脚本在执行期间访问的额外的全局变量

#### ecmaVersion - 默认设置为3，5（默认）来指定你想要使用的 ECMAScript 版本

#### sourceType - 设置为 "script" (默认) 或 "module"（如果你的代码是 ECMAScript 模块)

#### ecmaFeatures - 这是个对象，表示你想使用的额外的语言特性 常见配置项详情：

| 参数 | 描述 |
| :----- | :----- |
| "arrowFunctions": true | lambda表达式 |
| "destructuring": true | 解构赋值 |
| "classes": true | class |
| "defaultParams": true | http://es6.ruanyifeng.com/#docs/function#函数参数的默认值 |
| "blockBindings": true | 块级作用域，允许使用let const |
| "modules": true | 允许使用模块，模块内默认严格模式 |
| "objectLiteralComputedProperties": true | 启用计算对象文字属性名称 |
| "objectLiteralShorthandMethods": true | 允许对象字面量方法名简写 |
| "objectLiteralShorthandProperties": true,  | 对象字面量属性名简写  var foo = 'bar'; var baz = {foo}; baz // {foo: "bar"} // 等同于 var baz = {foo: foo}; |
| "restParams": true | http://es6.ruanyifeng.com/#docs/function#rest参数  |
| "spread": true | http://es6.ruanyifeng.com/#docs/function#扩展运算符 |
| "forOf": true | http://es6.ruanyifeng.com/#docs/iterator#for---of循环 |
| "generators": true | http://es6.ruanyifeng.com/#docs/generator |
| "templateStrings": true | http://es6.ruanyifeng.com/#docs/string#模板字符串   |
| "experimentalObjectRestSpread": true | [对象的扩展运算符...](http://es6.ruanyifeng.com/#docs/object) https://github.com/tc39/proposal-object-rest-spread (重要：这是一个实验性的功能,在未来可能会有明显改变。 建议你写的规则 不要 依赖该功能，除非当它发生改变时你愿意承担维护成本。)|
|  "superInFunctions": true | 构造函数内必须有super函数|

#### rules - 你可以使用注释或配置文件修改你项目中要使用的规则 常见配置项详情：

| 参数 | 描述 |
| :----- | :----- |
| no-console: 2 | 禁止使用console |
| array-bracket-spacing: [2, "never"] | 指定数组的元素之间要以空格隔开(,后面)， never参数：[ 之前和 ] 之后不能带空格，always参数：[ 之前和 ] 之后必须带空格 |
| quotes: [1, "single"] | 引号类型 `` "" '' | 
| brace-style: [2, "1tbs", { "allowSingleLine": true }] | if while function 后面的{必须与if在同一行，java风格 |
| camelcase: 2 | 双峰驼命名格式 |
| comma-dangle: [2, "never"] | 要求或禁止末尾逗号 |
| comma-spacing": [2, { "before": false, "after": true }] | 控制逗号前后的空格 |
| comma-style: [2, "last"] | [强制使用一致的逗号风格](http://eslint.org/docs/rules/comma-style) |
| computed-property-spacing: [2,"never"] | 以方括号取对象属性时，[ 后面和 ] 前面是否需要空格, 可选参数 never, always |

更多 rules 配置[请查阅这里](https://cn.eslint.org/docs/rules/)


## 通过配置.eslintignore文件忽略掉不想被检查的文件

其使用的是Glob路径书写方式，与「.gitignore」的使用方法相同

也可以在 package.json 文件中，通过 eslintIgnore 参数进行设置

可以通过在项目目录下建立.eslintignore文件，并在其中配置忽略掉对哪些文件的检查。需要注意的是，不管你有没有在.eslintignore中进行配置，eslint都会默认忽略掉对/node_modules/** 以及 /bower_components/**文件的检查

```angular2html
# Ignore built files except build/index.js
build/
!build/index.js
```

