---
layout: page
title: Linux 碎碎念
tags: [note]
---

## /etc/resolv.conf

这个文件是用来记录修改 DNS 的。其中 nameserver 最多设置3个（可以修改）。nameserver 只有在 timeout 的情况下才会查询下一个 nameserver。

> 详情参考：[RESOLV.CONF(5)](http://man7.org/linux/man-pages/man5/resolv.conf.5.html)

## shell 参数

* $0 脚本自己的名称
* $1,2,3... 第 n 个参数
* $# 参数的个数
* $@ 参数列表，只有参数，不包括 $0
* $* 同样表示参数列表
* "$*" 是一个字符串，但是 "$@" 是一个参数数组


## shell 命令

---

### man

`#man [command]`

一切的一切都要从 man 开始说起，通过它可以打印所有指令的帮助文档。

---

### help

`#[command] --help`

这不是一个指令，这是一个参数。在我所遇到的大部分指令以及公司开发的脚本中都会提供这个参数。他同样的打印帮助文档。比 man 指令要简洁，并且只要脚本开发者提供了帮助文档，基本也可以通过这个参数打印出来。类似的可以尝试：

`#[command] [-h] [-?]`

以上两者也可以打印，但感觉不像 help 更加的通用

---

### ln

`#ln -[Ffhinsv] [source_file] [target_file]`

link 指令，用来创建链接文件，链接文件并非实际存在。

连接文件分为 2 种，硬链接与软链接（符号链接）。

+ 软链接可以理解为文件的一个快捷方式。
+ 硬链接则是创建了一个指针指向源文件的内存区域

---

### ssh

`#ssh user@host`

建立 ssh 通道的指令，最简单与常用的莫过于登录服务器了。

ssh 还有其他的用法。比如建立 tunnel:

`#ssh -L [bind_address:]port:host:hostport`

通过上面的 ssh 指令可以将所有访问 port 端口的请求转发到 host 的 hostport 端口。通过 ssh 建立 tunnel 后可以经由跳板机访问公司内网。

---

### grep

`#grep [pattern] [file]`

是不是在 man 指令查看了 help 文档以后头昏眼花？在文档里面找一条记录找到放弃？试试 grep 吧！

---

### pidof 

`#pidof program`

用这个命令可以非常简单直接的查询到一个可执行程序的进程号，以前用 ps 查进程再切割的方法太落后
