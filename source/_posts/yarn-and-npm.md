---
title: Yarn vs npm 你需要知道的一切
date: 2019-06-29 14:33:17
categories:
- 前端辅助工具
tags: 
- yarn
toc: true 文章目录
---

+ Yarn 和 npm 具体区别
<!-- more -->
<The rest of contents | 余下全文>

大家可能都已经比较熟悉npm了，对于yarn可能还不是特别熟悉，我就是这样的，所以有了这篇文章，总结记录学习的过程。
## 1. What is yarn?

[Yarn](https://github.com/yarnpkg/yarn)发布于2016年10月，目前为止在Github上迅速拥有了36,691个Star。
而[npm](https://github.com/npm/cli)只有17,165个Star，从star的数量上就让我更充满对yarn探索的欲望。

### 那么什么是yarn呢？

[yarn官网](https://yarnpkg.com/zh-Hans/docs/getting-started)介绍：

>Yarn 对你的代码来说是一个包管理器， 你可以通过它使用全世界开发者的代码，或者分享自己的代码。 Yarn 做这些快捷、安全、可靠，所以你不用担心什么。
 通过Yarn你可以使用其他开发者针对不同问题的解决方案，使自己的开发过程更简单。 使用过程中遇到问题，你可以将其上报或者贡献解决方案。一旦问题被修复，Yarn会更新保持同步。 
 代码通过包（package）（或者称为模块（module））的方式来共享。 一个包里包含所有需要共享的代码，以及描述包信息的文件，称为package.json

Yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具，正如官方文档中写的，Yarn 是为了弥补 npm 的一些缺陷而出现的：

- npm 安装包（packages）的速度不够快，拉取的 packages 可能版本不同

- npm 允许在安装 packages 时执行代码，这就埋下了安全隐患

但是Yarn 没想要完全替代 npm，它只是一个新的 CLI 工具，拉取的 packages 依然来自 npm 仓库。仓库本身不会变，所以获取或者发布模块的时候和原来一样。

## 2. Yarn vs npm

npm 和 Yarn 都是通过 `package.json` 记录项目需要拉取的依赖模块，不过在使用时，往往 `package.json` 中模块的版本号不太会写得非常确切，通常是定个版本范围。
比如：
`package: "4.17.4"`  意思是安装指定版本4.17.4的package包
`package: "^4.17.4"` 意思是安装在4.X.X中最高版本号的package包
`package: "~4.17.4"` 意思是安装在4.17.X中最高版本号的package包

理论上，次版本号的变化并不会影响向后兼容性。因此安装最新版的依赖库应该是能正常工作的，而且能引入自4.17.4版本以后的重要错误和安全方面的修复。
但是，另一方面，不同的开发人员即使使用了相同的package.json文件，在他们自己的机器上也可能会安装同一个库的不同种版本，这样就会存在潜在的难以调试的错误，
在我们团队中就经常出现"我的电脑上没有问题"和"为什么只有我的电脑上出现这个问题"的情形。

每次执行npm install的时候，不是都从网络上搜索下载，npm是有本地缓存的，它保存了已经下载的每个版本的压缩包。本地缓存的内容可以通过npm cache ls命令进行查看。
本地缓存的设计有助于减少安装时间，但是下载过程还是很慢，因为npm遍历了整个依赖树，然后再决定如何生成扁平的node_modules目录结构。npm必须为所有使用到的模块构建一个完整的依赖关系树，这是一个耗时的操作。

### Yarn vs npm lock文件的不同 （yarn.lock文件保证在不同的设备上也能安装相同版本的 packages）

而为了防止拉取到不同的版本，Yarn 有一个锁定文件 (lock file) 记录了被确切安装上的模块的版本号。
每次只要新增了一个模块，Yarn 就会创建（或更新）yarn.lock 这个文件。这么做就保证了，每一次拉取同一个项目依赖时，使用的都是一样的模块版本。

npm 其实也有办法实现处处使用相同版本的 packages，但需要开发者执行 npm shrinkwrap 命令。
这个命令将会生成一个锁定文件，在执行 npm install 的时候，该锁定文件会先被读取，和 Yarn 读取 yarn.lock 文件一个道理。
npm 和 Yarn 两者的不同之处在于，Yarn 默认会生成这样的锁定文件，而 npm 要通过 shrinkwrap 命令生成 npm-shrinkwrap.json 文件，只有当这个文件存在的时候，packages 版本信息才会被记录和更新


### Yarn vs npm 在执行包的安装过程中的不同

无论 npm 还是 Yarn 在执行包的安装时，都会执行一系列任务，npm 是按照队列执行每个 package，也就是说必须要等到当前 package 成功安装之后，才能继续后面的安装。而 Yarn 是同步执行所有任务，提高了性能；

Yarn安装过程分为三个步骤：

- 解决方案： Yarn通过向注册表发出请求并以递归方式查找每个依赖项来开始解析依赖项。
- 获取：接下来，Yarn查找全局缓存目录以查看是否已下载所需的包。如果没有，Yarn会获取包的tarball并将其放在全局缓存中，这样它就可以脱机工作，不需要多次下载依赖项。依赖关系也可以作为完整脱机安装的tarball放在源代码控制中。
- 链接：最后，Yarn通过将全局缓存中所需的所有文件复制到本地node_modules目录来将所有文件链接在一起。

通过彻底打破这些步骤并获得确定性结果，Yarn能够并行化操作，从而最大限度地提高资源利用率并使安装过程更快。
在一些Facebook项目中，Yarn将安装过程减少了一个数量级，从几分钟到几秒钟。Yarn还使用互斥锁来确保多个正在运行的CLI实例不会相互冲突和相互污染。
在整个过程中，Yarn对包装安装提出了严格的保证。您可以控制为哪些包执行哪些生命周期脚本。包校验和也存储在锁文件中，以确保每次都获得相同的包。

### Yarn vs npm 更简洁的输出

npm 的输出信息比较冗长。在执行 npm install <package> 的时候，命令行里会不断地打印出所有被安装上的依赖。
相比之下，Yarn 简洁太多：默认情况下，结合了 emoji （Windows 上 emoji 不可见）直观且直接地打印出必要的信息，也提供了一些命令供开发者查询额外的安装信息。

## 3. 关于 Yarn 的总结

yarn优势：

- 1. Yarn缓存它下载的每个包，因此它永远不需要再次下载相同的包。它还并行执行几乎所有操作以最大限度地利用资源。这意味着更快的安装速度;
- 2. 使用详细但简洁的锁文件格式和确定性算法进行安装操作，Yarn能够保证在一个系统上运行的任何安装在另一个系统上都能完全相同;
- 3. Yarn在执行代码之前使用校验和来验证每个已安装软件包的完整性.

yarn特征：

- 1. 离线模式: 如果您之前已安装过软件包，则无需连接互联网即可重新安装。
- 2. 确定性: 无论安装顺序如何，相同的依赖关系都将以相同的方式安装在任何计算机上
- 3. 网络性能: yarn有效地对请求进行排队并避免请求瀑布以最大化网络利用率
- 4. 网络恢复力: 单个失败的请求不会导致整个安装失败。 
- 5. 失败时会自动重试请求
- 6. 平面模式
- 7. Yarn将不匹配的依赖项版本解析为单个版本，以避免创建重复项。
- 8. 更多表情符号

## 4. npm 与 yarn 常用命令对比

| Npm | Yarn | 功能描述 |
| :----- | :----- | :----- |
| npm install (npm i) | yarn install (yarn i) / yarn | 根据package.json文件下载相应依赖，安装是默认的行为 |
| npm i --save [package] | yarn add [package] | 添加一个依赖包 |
| npm i [package] --save-dev | yarn add [package] -dev | 添加一个依赖包到 devDependencies |
| npm uninstall [package] | yarn remove [package] | 删除一个依赖包 |
| npm update --save | yarn upgrade | 更新依赖包 |
| npm install --global [package] | yarn global add [package] | 安装依赖包到全局 |
| npm install -g license-checker; license-checker| yarn licenses [list|generate-disclaimer] | yarn license ls()列出所有安装包的授权信息 |
