---
layout: post
title: Linux Commands
---

# Linux Commands

记录一些自己工作生活中曾经使用过的 Linux 指令，不定期更新。

---

### man

`#man [command]`

一切的一切都要从 man 开始说起，通过它可以打印所有指令的帮助文档。

### --help

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

建立 ssh 通道的指令，最简单与常用的莫过于登录服务器了。但是 ssh 还有其他的用法。比如建立 tunnel:

`#ssh -L [bind_address:]port:host:hostport`

通过上面的 ssh 指令可以将所有访问 port 端口的请求转发到 host 的 hostport 端口。通过 ssh 建立 tunnel 后可以经由跳板机访问公司内网。
