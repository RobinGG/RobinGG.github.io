---
layout: post
title: Linux Commands
tags: [Linux]
---

记录一些自己工作生活中曾经使用过的 Linux 指令，不定期更新。

---

### man

`# man [command]`

一切的一切都要从 man 开始说起，通过它可以打印所有指令的帮助文档。

---

### help

`# [command] --help`

这不是一个指令，这是一个参数。在我所遇到的大部分指令以及公司开发的脚本中都会提供这个参数。他同样的打印帮助文档。比 man 指令要简洁，并且只要脚本开发者提供了帮助文档，基本也可以通过这个参数打印出来。类似的可以尝试：

`#[command] [-h] [-?] [help]`

以上两者也可以打印，但感觉不像 help 更加的通用

---

### ln

`# ln -[Ffhinsv] [source_file] [target_file]`

link 指令，用来创建链接文件，链接文件并非实际存在。

连接文件分为 2 种，硬链接与软链接（符号链接）。

+ 软链接可以理解为文件的一个快捷方式。
+ 硬链接则是创建了一个指针指向源文件的内存区域

---

### ssh

`# ssh user@host`

建立 ssh 通道的指令，最简单与常用的莫过于登录服务器了。

ssh 还有其他的用法。比如建立 tunnel:

`# ssh -L [bind_address:]port:host:hostport`

通过上面的 ssh 指令可以将所有访问 port 端口的请求转发到 host 的 hostport 端口。通过 ssh 建立 tunnel 后可以经由跳板机访问公司内网。

---

### grep

`# grep [pattern] [file]`

是不是在 man 指令查看了 help 文档以后头昏眼花？在文档里面找一条记录找到放弃？试试 grep 吧！

---

### pidof 

`# pidof program`

用这个命令可以非常简单直接的查询到一个可执行程序的进程号，以前用 ps 查进程再切割的方法太落后

---

### pgrep

`# pgrep [options] pattern`

通过名称查找进程，是支持模糊查询的。
* 加上 `-x` 参数以后就和 `pidof` 是一样的了
* 加上 `-l` 参数可以显示程序名称
* 加上 `-a` 参数可以显示整个程序运行的指令，等价于 `ps -ef | grep pattern`

---

### su

`# su [options] [-] [user]`

`su` 我最常用来切换用户，一般来说是为了避免权限问题。不指定用户的话默认是切换到 root 用户。中间的短横线 '-' 是有意义的，等同于 `--login`。加上以后是一个 `login shell`。这中间的知识点又可以单独拎出来讲了，大家可以自己查一下。建议的话加上 `-`，比如 `su - fakeUser`

### diff

`# diff [OPTION]... FILES`

`diff` 用来进行文件的比较，有三种模式：normanl, context(-c), unified(-u)。关于三种模式的不同，阮一峰的博客[《读懂diff》](http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html)讲得挺清楚的。

### sed

`# sed [OPTION]... {script-only-if-no-other-script} [input-file]...`

`sed` 是神技之一了，可以用来在不打开文件的情况下操作文件内容，包括替换，删除，增加内容等。

- 替换： sed "s/origin content/new content/g" /some/file
- 插入： sed "1iFirst Line" /some/file
- 删除： sed "1d" /some/file

在真正使用的时候建议加上 `-i.bak`，保存一份先前的备份，这样出了问题也好回滚。**注意**：如果这个文件在一个配置目录下面，会导致两份配置都被加载
