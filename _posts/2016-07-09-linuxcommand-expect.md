---
layout: post
title: "linux-command expect"
date: 2016-07-09 09:15:17
image: '/assets/img/'
description:
main-class: "linux"
color: 
tags:
- expect
- linux command
categories:
twitter_text:
introduction:
---

项目部署过程中需要一个自动填充密码的功能，就想研究一下`expect`，本来想自己写一篇文章，后来查找资料的过程中看到了[果冻想](http://www.jellythink.com/)里的一片[文章](http://www.jellythink.com/archives/1470),觉得自己不可能写得比这个好了，于是就决定的抄过来，再加一些小东西。

----

#### 随处可见的`expect`

第一次见expect这个命令还是我第一次参加全量上线的时候，那是公司的一个牛人用Shell脚本写的一套自动部署、MD5 比对、发布的全量上线工具，没事的时候，看了下其中的几个脚本，好多的expect命令。实在是看不懂这个expect命令的用法，所以就找时间总结了这篇关于expect命令的文章。


#### 先抛出一个问题

现在有两台Linux主机A和B，如何从A主机ssh到B主机，然后在B主机上执行命令，如何使这个过程实现全程自动化？你可能会使用这种方法：

```sh
ssh admin@10.220.20.15 "ls"
```
但是这种方式比较笨拙，每次都要输入密码，同时并不能执行一些复杂的逻辑或命令。那么如何实现全程自动化呢？这就要用到今天这篇文章总结的`expect`了。

#### `expect`是什么？

`expect`是一个免费的编程工具，用来实现自动的交互式任务，而无需人为干预。说白了，expect就是一套用来实现自动交互功能的软件。

在实际工作中，我们运行命令、脚本或程序时，这些命令、脚本或程序都需要从终端输入某些继续运行的指令，而这些输入都需要人为的手工进行。而利用expect，则可以根据程序的提示，模拟标准输入提供给程序，从而实现自动化交互执行。这就是`expect`！！！

#### `expect`基础

在使用`expect`时，基本上都是和以下四个命令打交道：

|     命令           |   作用                |
| ---------         |:---------:            |
|     send          |   用于向进程发送字符串    |
|     expect        |   从进程接收字符串       |
|     spawn         |   启动新的进程          |
|     interact      |   允许用户交互          |

- `send`命令接收一个字符串参数，并将该参数发送到进程。
- `expect`命令和send命令相反，`expect`通常用来等待一个进程的反馈，我们根据进程的反馈，再发送对应的交互命令。
- `spawn`命令用来启动新的进程，`spawn`后的`send`和`expect`命令都是和使用`spawn`打开的进程进行交互。
- `interact`命令用的其实不是很多，一般情况下使用`spawn`、`send`和`expect`命令就可以很好的完成我们的任务；但在一些特殊场合下还是需要使用`interact`命令的，`interact`命令主要用于退出自动化，进入人工交互。比如我们使用`spawn`、`send`和`expect`命令完成了ftp登陆主机，执行下载文件任务，但是我们希望在文件下载结束以后，仍然可以停留在ftp命令行状态，以便手动的执行后续命令，此时使用`interact`命令就可以很好的完成这个任务。

#### 实用代码分析

上面对`expect`进行了总结，特别是对一些常用的命令进行了详细的说明。下面就通过一些常用的`expect`脚本来具体的说明如何使用`expect`来完成日常的一些工作。

```sh
#!/usr/tcl/bin/expect

set timeout 30
set host "101.200.241.109"
set username "root"
set password "123456"

spawn ssh $username@$host
expect "*password*" {send "$password\r"}
interact
```

这是一段非常简单的expect示例代码，演示了`expect`的基本使用方法。

* `#!/usr/tcl/bin/expect`：使用expect来解释该脚本；
* `set timeout 30`：设置超时时间，单位为秒，默认情况下是10秒；
* `set host "101.200.241.109"`：设置变量；
* `spawn ssh $username@$host`：`spawn`是进入`expect`环境后才可以执行的`expect`内部命令，如果没有装`expect`或者直接在默认的SHELL下执行是找不到`spawn`命令的。它主要的功能是给ssh运行进程加个壳，用来传递交互指令；
* `expect "*password*"`：__这里的`expect`也是`expect`的一个内部命令__，这个命令的意思是判断上次输出结果里是否包含“password”的字符串，如果有则立即返回；否则就等待一段时间后返回，这里等待时长就是前面设置的30秒；
* `send "$password\r"`：当匹配到对应的输出结果时，就发送密码到打开的ssh进程，执行交互动作；
* `interact`：执行完成后保持交互状态，把控制权交给控制台，这个时候就可以手工操作了。如果没有这一句登录完成后会退出，而不是留在远程终端上。

这就是对上述这段简单简单脚本的分析，在上述的示例中，涉及到expect中一个非常重要的概念——__模式-动作__；即上述`expect "*password*" {send "$password\r"}`这句代码表达出来的含义。

#### 模式-动作

结合着`expect "*password*" {send "$password\r"}`这句代码来说说“模式-动作”。简单的说就是匹配到一个模式，就执行对应的动作；匹配到password字符串，就输入密码。你可能也会看到这样的代码：

```sh
expect {
    "password" {
        send "$password\r"
        exp_continue
    }
    eof
    {
        send "eof"
    }
}
```
其中`exp_continue`表示循环式匹配，通常匹配之后都会退出语句，但如果有`exp_continue`则可以不断循环匹配，输入多条命令，简化写法。

#### 传参

很多时候，我们需要传递参数到脚本中，现在通过下面这段代码来看看如何在`expect`中使用参数：

```sh

#!/usr/tcl/bin/expect

if {$argc < 3} {
    puts "Usage:cmd <host> <username> <password>"
    exit 1
}

set timeout -1
set host [lindex $argv 0] 
set username [lindex $argv 1]
set password [lindex $argv 2]

spawn ssh $username@$host
expect "*password*" {send "$password\r"}
interact
```

在`expect`中，`$argc`表示参数个数，而参数值存放在`$argv`中，比如取第一个参数就是`[lindex $argv 0]`，以此类推。





