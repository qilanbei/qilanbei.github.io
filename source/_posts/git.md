---
title: Git学习笔记
date: 2016-06-27 01:00:25
description: 关于git的学习笔记
categories:
- Git相关
tags: 
- git学习
toc: true 文章目录
---
+ 关于git的学习笔记
<!-- more -->
<The rest of contents | 余下全文>

### 1.  版本控制
#### 1.1 关于版本控制
##### 版本控制（Version Control Systems）是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。这个系统可以自动帮我们备份文件的每一次更改，并且可以非常方便的恢复到任意的备份（版本）状态。
### 2. Git工作原理
#### 2.1 为了更好的学习Git，我们们必须了解Git管理我们文件的3种状态，分别是已提交（committed）、已修改（modified）和已暂存（staged），由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。实现版本控制的软件有很多种类，大致可以分为本地版本控制系统、集中式版本控制系统、分布式版本控制系统。
#### 2.2  本地版本控制系统
##### 2.2.1 借助软件我们可以记录下文件的每一次修改，如下图所示，文件被修改后，记录下了3个版本，这样我们通过版本控制系统（软件）便可以非常方便的恢复到任意版本。缺点：这种类型的版本控制系统，功能比较单一，比如很难实现多人协同开发，所以现在几乎很少使用了。
#### 2.3 集中式版本控制系统
##### 2.3.1 实际开发环境，一个项目通常是由多人协作共同完成的，如何让在不同终端上的开发者协同工作成了亟待解决的问题，集中式版本控制系统便应运而生了。它通过单一的集中管理的服务器，保存所有文件的修订版本，协同工作的开发者都通过客户端连到这台服务器，取出最新的文件或者提交更新。其代表为SVN。缺点：这种方式很好解决了多人协同开发的问题，但是也有一个弊端，如果集中管理的服务器出现故障，将会导致数据（版本）丢失的风险，另外协同开发者从集中服务器中更新数据时，严重依赖网络，如果网络不佳，也给开发带来诸多不便。
#### 2.4分布式版本控制系统
##### 2.4.1 分布式版本控制系统，则不需要中央服务器，每个协同开发者都拥有一个完整的版本库，这么一来，任何协同开发者用的服务器发生故障，事后都可以用其它协同开发者本地仓库恢复。由于版本库在本地计算机，也便不再受网络影响了。如果要将本地的修改，推送给其它协同开发者，还需要一台共享服务器，所有开发者通过这台共享服务器同步和更新数据。分布式版本控制系统弥补了前面两种版本控制系统的缺陷，成为了版本控制的首选方案。其代表就是Git。
### 3. Git
#### 3.1 Git工作原理
##### 3.1.1 为了更好的学习Git，我们们必须了解Git管理我们文件的3种状态，分别是已提交（committed）、已修改（modified）和已暂存（staged），由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。
1.  Git仓库目录是Git用来保存项目的元数据和对象数据库的地方。 这是Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。
2. 工作目录是对项目的某个版本独立提取出来的内容。这些从Git仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
3. 暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在Git仓库目录中。有时候也被称作“索引”（Index），不过一般说法还是叫暂存区域。
##### 3.1.2基本的Git工作流程如下

	1. 在工作目录中修改文件。
	2. 暂存文件，将文件的快照放入暂存区域。
	3. 提交文件，找到暂存区域的文件，将快照永久性存储到Git仓库目录。
	
#### 3.2 Git本地仓库
##### Git本地仓库指的是开发者开发设备中的仓库。
##### 3.2.1 Git基础
###### 命令行方式：任意目录（建议开发目录）右键 > Git Bash Here
###### 1.  配置用户:配置用户的意义在于记录开发者信息，以便在版本控制记录开发者的操作行为，如lion于2016-08-24解决了一个bug。
```	 
	 git config --global user.name "自已的名字"
	git config --global user.email "自已的邮箱地址"
	--global 配置当前用户所有仓库
	--system 配置当前计算机上所有用户的所有仓库
	注：配置用户只需要执行1次，可以重复使用。
```
###### 2. 初始化仓库: 我们如果想要利用git进行版本控制，需要将现有项目初始化为一个仓库，或者将一个已有的使用git进行版本控制的仓库克隆到本地。
```
a) git init
git init只是创建了一个名为.git的隐藏目录，这个目录就是存储我们历史版本的仓库，ls -al 可以查看。
b) git clone 仓库地址
假如公司已有项目用了Git，那我们就利用克隆.
执行完这个命令，会在当前目录下生成一个Monment目录（默认和仓库名称相同），这个便是已有一个使用Git管理的项目。

```
###### 3. 查看文件状态:初始化仓库后便可以进行开发了，进入到刚刚创建好并初始为仓库的目录，添加我们开发需要的文件。
```
使用 git status:

NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        modified:   index.txt
```
###### 4. 添加文件到暂存区: 放到暂存区的文件被标记成了绿色，等待提交。
注：颜色是工具给添加的，目的是增加可读性并不是git统一的。
```
git add 文件名 “*”或-A代表所有
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git add --all
```
4.1 如果需要撤销更改
4.1.1 再次git status可以再次查看仓库状态
```
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git status
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   index.txt
```
说明index.html再次被修改了并被标记了红色。\
	4.1.2 又经过一段时间后发现新开发的部分有Bug，想要回到之前状态，可以使用git checkout 文件名。从暂存区还原原到工作区:
```
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git checkout index.txt
```

###### 5. 提交文件：经过一个相对较长阶段开发或者一个功能开发完成了，就可以提交到本地仓库了，永久保存了。
```
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git commit -m '编辑了index.txt'
[master d52e561] 编辑了index.txt
 1 file changed, 1 insertion(+)

```
这时git status查看状态:
```
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git status
On branch master
nothing to commit, working tree clean

```
没有什么可提交的，变的很干净。

###### 6. 查看提交历史:反反复复开发了很多的功能了，通过git log查看一下提交的历史
```
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git log
commit d52e561f4399593a236314c7266ca364c8968eac
Author: NW <1011269522@qq.com>
Date:   Fri Oct 28 20:05:45 2016 +0800

    编辑了index.txt

commit 7c7a324c760b99a05a51d6003cf41d4a9c1a8f68
Author: NW <1011269522@qq.com>
Date:   Fri Oct 28 17:07:07 2016 +0800

    添加了index.txt文件
```

我们可以查看到一次次提交记录:
```
commit 7c7a324c760b99a05a51d6003cf41d4a9c1a8f68
```
代表一次提交的唯一ID，一般称为SHA值。傻？
注：按键盘q键退出。

###### 7. 再次检测仓库文件状态:隔了好些天后，继续开发
git status 查看状态:
这时关掉所有目录甚至关机！

###### 8. 恢复上一次提交的状态
通过SHA值可以回到之前某一次的提交（时光倒流）
```

NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git reset --hard 7c7a324c760b99a05a51d6003cf41d4a9c1a8f68
HEAD is now at 7c7a324 添加了index.txt文件

```
这样便回到了添加了index.txt文件功能的状态
git log再次查看发现最后一次提交成功能
```
NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git log
commit 7c7a324c760b99a05a51d6003cf41d4a9c1a8f68
Author: NW <1011269522@qq.com>
Date:   Fri Oct 28 17:07:07 2016 +0800

    添加了index.txt文件

```
##### 3.2.2 Git分支
> 在我们的现实开发中，需求往往是五花八门的，同时开发个需求的情况十分常见，比如当你正在专注开发一个功能时，突然有一个紧急的BUG需要你来修复，这个时候我们当然是希望在能够保存当前任务进度，再去修改这个BUG，等这个BUG修复完成后再继续我们的任务。如何实现呢？

###### 1. Git创建分支来解决实际开发中类似问题
在Git的使用过程中一次提交称为历史记录（版本），并且会生成一个唯一的字符串
```
commit 7c7a324c760b99a05a51d6003cf41d4a9c1a8f68
Author: NW <1011269522@qq.com>
Date:   Fri Oct 28 17:07:07 2016 +0800
```
###### 这个串可以代表某一个历史版本（实际使用只取前面几位就可以），值得注意的是所有的提交（commit）实际上都是在分支（branch）的基础上进行的。
###### 当我们在初始化仓库的时候（实际上是产生第1次提交时），Git会默认帮我们创建了一个master的分支，并且有指针（HEAD）指到了末端。
###### 指针（HEAD）用来标明当前处于哪个分支的哪个版本，如上图指的处于master分支的最后1个版本。
###### 2. 我们也可以创建自已的分支
2.1 创建分支:新的分支会在当前分支原有历史版本的结点上进行创建，我称其为子分支 , 新建的子分支会继承父分支的所有提交历史。
```
 git branch hotfix
 
```
 
2.2 切换分支 

```

NW@NW-PC MINGW64 ~/Desktop/gitStudy (master)
$ git checkout hotfix
Switched to branch 'hotfix'

NW@NW-PC MINGW64 ~/Desktop/gitStudy (hotfix)
```
** 原来末端是master 切换分支后变成hotfix **
 ** HEAD现在又指向了hotfix的末端。**
 
2.3 再次提交操作
* 这次的提交历史版本就会记录在hotfix这个分支上了，并且HEAD伴随hotfix在移动。

2.4 回到master
当我们再次切回到master时 , HEAD指向了master分支的末端，并且我们观察发现我们的文件内容还是原来的“模样”。
###### 这个时候我们就在hotfix这个分支上修复了这个BUG，而我们原来在master分支上的操作并未受到影响。修改Bug后的文件暂存在hotfix分支的暂存区了。这时的master分支并没有包含有hotfix的修复。##

2.5 继续之前的开发

总结：当我们git checkout branchname时，HEAD会自动指向对应分支的末端，工作目录中的源码也会随之发生改变。

2.6 合并（融合）分支

这时master会有两个父结点了，master便包含了hotfix里的修复了

2.7 删除分支
```
git branch -d hotfix
```
###### 这时用来修复BUG创建的hotfix分支已经没有用处了，我们可以将它删除。
###### 3. Git帮我们解决了什么问题
- 版本管理 git add->git commit =新版本（可以在将来恢复到某版本）
-  多任务开发  一个分支代表一个任务 分支之间不会影响  
git branch->git checkout->git add->git commit=>新版本
- 协同开发 通过共享仓库实现(多人共同开发)
- Git共享：xxx.git ->git init  --bare(创建一个共享(裸)仓库)
  git push->xxx.git
  git pull ->xx.git

###### 4. ssh
