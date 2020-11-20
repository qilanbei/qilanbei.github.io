---
title: 分享一些实用的终端命令
date: 2019-03-27 10:29:34
categories:
- 终端命令
tags: 
- 终端命令
toc: true 文章目录
---
+ 大大提高你的工作效率的终端命令
<!-- more -->
<The rest of contents | 余下全文>

写在前面

无论是后端还是前端，都会或多或少的接触到linux命令的操作，知道一些实用的linux命令技巧，真的能大大提高我们的工作效率

#### 命令编辑及光标移动

这里有很多快捷键可以帮我们修正自己的命令。接下来使用[光标]代替光标的位置

删除从开头到光标处的命令文本: ctrl + u
```angular2html
cd /text/demo;ls -al[光标]

小提示：cd 路径 它用于切换当前目录，它的参数是要切换到的目录的路径，可以是绝对路径，也可以是相对路径

如果此时使用ctrl + u快捷键，那么该条命令都会被清除，而不需要长按backspace键。

```
删除从光标到结尾处的命令文本: ctrl + k
```angular2html
cd /proc/tty光标;ls -al

如果此时使用ctrl + k快捷键，那么从光标开始处到结尾的命令文本将会被删除。

```

还有其他的操作，不再举例，例如：

- ctrl + a:光标移动到命令开头
- ctrl + e：光标移动到命令结尾
- alt  f:光标向前移动一个单词
- alt  b：光标向前移动一个单词
- ctrl w：删除一个词（以空格隔开的字符串）

#### 历史命令快速执行

记录执行的历史命令: history

搜索执行过的命令: ctrl + r

![](/images/linux_1.png)

实时查看日志: tail -f 加文件名
```angular2html
tail -f filename.log

可以实时显示日志文件内容，当然，使用less命令查看文件内容，并且使用shift+f键，也可达到类似的效果mode

```

#### 磁盘或内存情况查看

怎么知道当前磁盘是否满了呢: df -h

![](/images/linux_2.png)

使用df命令可以快速查看各挂载路径磁盘占用情况

当前内存使用情况: free -h

```angular2html
想看内存使用情况, linux上使用free命令, mac上没这命令，可以这样查询：

$ top -l 1 | head -n 10 | grep PhysMem

可以在.bashrc文件中添加一个别名叫free:

alias free='top -l 1 | head -n 10 | grep PhysMem'

以后直接使用free来查看就可以了
```

使用-h参数

不知道你是否注意到，我们在前面几个命令中，都使用了-h参数，它的作用是使得结果以人类可读的方式呈现

#### 查看文件夹以及文件大小

du命令可以显示文件占用的磁盘大小：`du -- display disk usage statistics`

语法：`du [-H | -L | -P] [-a | -s | -d depth] [-c] [-h | -k | -m | -g] [-x] [-I mask] [file ...]`

比较常用的几个选项：

- -d：指定目标文件夹的统计层数，-d 0统计整个文件夹大小，-d 1统计文件夹下第一层的文件大小，以此类推

- -h：显示人类可以读懂的单位（K、M、G）

- -s：统计单个文件、文件夹的大小，等同于-d 0

不指定file参数，会统计当前文件夹下的所有文件的大小

```
举例 ：比如你想查看当前某个文件夹的大小
du -h -s [dir_url]
```


#### Mac_查看开关机记录

查看开机时间记录: last | grep reboot

查看关机时间记录: last | grep shutdown

查询Mac一共运行了多久: uptime

```angular2html
14:03  up  3:51, 4 users, load averages: 1.89 1.80 2.00

参数说明: 14:03->当前时间；up  3:51: 系统运行时长; 4 users -> 登录用户（第二个是串行端口终端）

load averages ->  后面的三个数字则显示了系统最近1分钟、5分钟、15分钟的系统平均负载情况
(例如，本输出中系统有4个逻辑CPU，如果load average的三个值长期大于4时，说明CPU很繁忙，负载很高，可能会影响系统性能，但是偶尔大于4时，倒不用担心，一般不会影响系统性能。
相反，如果load average的输出值小于CPU的个数，则表示CPU还有空闲)

```

终端输入w（类似uptime可推算出开机时间，可查看登录用户信息）
```angular2html
14:17  up  4:05, 3 users, load averages: 2.05 2.63 2.45
USER     TTY      FROM              LOGIN@  IDLE WHAT
username    console  -                10:12    4:04 -
username    s000     -                14:17       - w
username    s001     -                10:19       9 hexo  
```

#### 多条命令执行

使用分号隔开可以执行多条命令

```angular2html

cd /temp/log/;rm -rf *

但是如果当前目录是/目录，并且/temp/log目录不存在，那么就会发生激动人心的一幕：

cd: /temp/log: No such file or directory
 
因为;可以执行多条命令，但是不会因为前一条命令失败，而导致后面的不会执行，
因此，cd执行失败后，仍然会继续执行rm -rf *，由于处于/目录下，结果就可以删库跑路啦
 
如果解决呢？很简单，使用&&，例如:
cd /temp/log/&&rm -rf *

```
#### 根据进程名称进行操作

列出当前系统运行中的进程的状态信息：ps

它只显示命令执行时的进程状态，如果想要动态列出状态信息，可以选择使用top命令

```angular2html
ps 参数：
a  显示所有进程
-a 显示同一终端下的所有程序
-A 显示所有进程
c  显示进程的真实名称
-N 反向选择
-e 等于“-A”
e  显示环境变量
f  显示程序间的关系
-H 显示树状结构
r  显示当前终端的进程
T  显示当前终端的所有程序
u  指定用户的所有进程
-au 显示较详细的资讯
-aux 显示所有包含其他使用者的行程 
-C<命令> 列出指定命令的状况
--lines<行数> 每页显示的行数
--width<字符数> 每页显示的字符数
--help 显示帮助信息
--version 显示版本显示

```

进程太多时分页显示: ps -aux|more


快速直接查找进程id: pgrep + 进程名称；或者： pidof + 进程名称

根据名称杀死进程: killall + 进程名称；或者：pkill + 进程名称

```angular2html
一般我们可以使用kill　-9 pid方式杀死一个进程，但是这样就需要先找到这个进程的进程id，实际上我们也可以直接根据名称杀死进程

```

查看进程运行时间: ps -p 进程id -o lstart,etime 

#### 查看压缩日志文件

有时候文件是压缩的, 不解压查看: zcat test.gz；或者：zless test.gz

#### 命令行下的复制粘贴
在命令行下，复制不能再是ctrl + c了，因为它表示终止当前进程，而控制台下的复制粘贴需要使用下面的快捷键：

```angular2html
ctrl +  insert
shift + insert

mac 系统：cmd + c; cmd + v

```

#### 搜索包含某个字符串的文件

当前目录下查找包含test字符串的文件: grep -rn "搜索的关键字"


#### 查看端口号进程占用

lsof -i tcp:8080 （想查阅的端口号）

![](/images/linux_3.png)

接下来继续在命令行输入 kill pid (即：杀死进程 图中的 PID: 351 或者 379)

#### 指定目录下的全部文件复制到另一个目录中 copy

```angular2html
语法： cp [选项] 源文件或目录 目标文件或目录 
说明：该命令把指定的源文件复制到目标文件或把多个源文件复制到目标目录中。
示例：cp -rf /source_demo/* /newFile

source_demo文件夹的所有子文件都会强制复制到newFile目录下
```
该命令的各选项含义如下：
- a 该选项通常在拷贝目录时使用。它保留链接、文件属性，并递归地拷贝目录，其作用等于dpR选项的组合。
- d 拷贝时保留链接。
- f 删除已经存在的目标文件而不提示。
- i 和f选项相反，在覆盖目标文件之前将给出提示要求用户确认。回答y时目标文件将被覆盖，是交互式拷贝。
- p 此时cp除复制源文件的内容外，还将把其修改时间和访问权限也复制到新文件中。
- r 若给出的源文件是一目录文件，此时cp将递归复制该目录下所有的子目录和文件。此时目标文件必须为一个目录名。
- l 不作拷贝，只是链接文件。

#### 指定目录下的全部文件移动到另一个目录中 move

用法同copy

#### 文件传输工具 curl

curl 是一个利用URL规则在命令行下工作的文件传输工具，可以说是一款很强大的http命令行工具，它支持文件的上传和下载，是综合传输工具，但按传统，习惯称url为下载工具。

常见参数：

-A/--user-agent <string>              设置用户代理发送给服务器
-b/--cookie <name=string/file>    cookie字符串或文件读取位置
-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中
-C/--continue-at <offset>            断点续转
-D/--dump-header <file>              把header信息写入到该文件中
-e/--referer                                  来源网址
-f/--fail                                          连接失败时不显示http错误
-o/--output                                  把输出写到该文件中
-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名
-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围
-s/--silent                                    静音模式。不输出任何东西
-T/--upload-file <file>                  上传文件
-u/--user <user[:password]>      设置服务器的用户和密码
-w/--write-out [format]                什么输出完成后
-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理
-#/--progress-bar                        进度条显示当前的传送状态

基本用法: 
```angular2html
<!--执行后，www.linux.com 的html就会显示在屏幕上了-->

curl http://www.linux.com

```

**常见用处：**

- 获取页面内容: 当我们不加任何选项使用 curl 时，默认会发送 GET 请求来获取链接内容到标准输出。

```angular2html
curl http://www.example.com
```

- 显示 HTTP 头: 如果我们只想要显示 HTTP 头，而不显示文件内容，可以使用 -I 选项

```angular2html
curl -I http://www.example.com
```

也可以同时显示 HTTP 头和文件内容，使用 -i 选项：

```angular2html
curl -i http://www.example.com
```

- 将链接保存到文件: 我们可以使用 > 符号将输出重定向到本地文件中

```angular2html
curl http://www.example.com > index.html
```

也可以通过 curl 自带的 -o/-O 选项将内容保存到文件中

**-o（小写的 o）**：结果会被保存到命令行中提供的文件名 
**-O（大写的 O）**：URL 中的文件名会被用作保存输出的文件名

```angular2html
curl -o index.html http://www.example.com
curl -O http://www.example.com/page/2/
```

**注意**：使用 -O 选项时，必须确保链接末尾包含文件名，否则 curl 无法正确保存文件。如果遇到链接中无文件名的情况，应该使

用 -o 选项手动指定文件名，或使用重定向符号。

- 同时下载多个文件: 我们可以使用 -o 或 -O 选项来同时指定多个链接，按照以下格式编写命令：

```angular2html
curl -O http://www.example.com/page/2/ -O http://www.example.com/page/3/
<!--或者：-->
curl -o page1.html http://www.example.com/page/1/ -o page2.html http://www.example.com/page/2/

```
- 使用 -L 跟随链接重定向: 如果直接使用 curl 打开某些被重定向后的链接，这种情况下就无法获取我们想要的网页内容。例如：

```angular2html
curl http://www.example.com
```
而当我们通过浏览器打开该链接时，会自动跳转到 http://www.example.com。此时我们想要 curl 做的，就是像浏览器一样跟随链接的跳转，获取最终的网页内容。我们可以在命令中添加 -L 选项来跟随链接重定向：

```angular2html
curl -L http://www.example.com
```

- 使用 -A 自定义 User-Agent: 我们可以使用 -A 来自定义用户代理，例如下面的命令将伪装成安卓火狐浏览器对网页进行请求：

```angular2html
curl -A "Mozilla/5.0 (Android; Mobile; rv:35.0) Gecko/35.0 Firefox/35.0" http://www.baidu.com
```

- 使用 -H 自定义 header: 当我们需要传递特定的 header 的时候，可以仿照以下命令来写：

```angular2html
curl -H "Referer: www.example.com" -H "User-Agent: Custom-User-Agent" http://www.baidu.com

```
可以看到，当我们使用 -H 来自定义 User-Agent 时，需要使用 "User-Agent: xxx" 的格式。

我们能够直接在 header 中传递 Cookie，格式与上面的例子一样：

```angular2html
curl -H "Cookie: JSESSIONID=D0112A5063D938586B659EF8F939BE24" http://www.example.com

```

- 使用 -c 保存 Cookie: 当我们使用 cURL 访问页面的时候，默认是不会保存 Cookie 的。有些情况下我们希望保存 Cookie 以便下次访问时使用

```angular2html
<!-- -c 后面跟上要保存的文件名。-->
curl -c "cookie-example" http://www.example.com

```

- 使用 -b 读取 Cookie: 前面讲到了使用 -H 来发送 Cookie 的方法，这种方式是直接将 Cookie 字符串写在命令中。如果使用 -b 来自定义 Cookie，命令如下：

```angular2html
curl -b "JSESSIONID=D0112A5063D938586B659EF8F939BE24" http://www.example.com

```
如果要从文件中读取 Cookie，-H 就无能为力了，此时可以使用 -b 来达到这一目的：
```angular2html
curl -b "cookie-example" http://www.example.com
```
即 -b 后面既可以是 Cookie 字符串，也可以是保存了 Cookie 的文件名。

- 使用 -d 发送 POST 请求: 我们以登陆网页为例来进行说明使用 cURL 发送 POST 请求的方法。假设有一个登录页面 www.example.com/login，只需要提交用户名和密码便可登录。我们可以使用 cURL 来完成这一 POST 请求，-d 用于指定发送的数据，-X 用于指定发送数据的方式：

```angular2html
curl -d "userName=tom&passwd=123456" -X POST http://www.example.com/login

```
在使用 -d 的情况下，如果省略 -X，则默认为 POST 方式：
```angular2html
curl -d "userName=tom&passwd=123456" http://www.example.com/login

```
强制使用 GET 方式: 发送数据时，不仅可以使用 POST 方式，也可以使用 GET 方式，例如：
```angular2html
curl -d "somedata" -X GET http://www.example.com/api
```
或者使用 -G 选项：
```angular2html
curl -d "somedata" -G http://www.example.com/api
```
从文件中读取 data
```angular2html
curl -d "@data.txt" http://www.example.com/login
```
带 Cookie 登录
```angular2html
curl -c "cookie-login" -d "userName=tom&passwd=123456" http://www.example.com/login

```
再次访问该网站时，使用以下命令：
```angular2html
curl -b "cookie-login" http://www.example.com/login
```
这样，就能保持访问的是登录后的页面了。
