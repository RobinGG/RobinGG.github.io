---
layout: post
title: git remote 命令
tags: [git]
---

公司工程项目非常之多，有时候会想要看别的模块的源代码。如果已经有同事 pull 过代码，那就可以通过 `git remote -v` 直接查看项目的地址了。例如查看这个博客的 github 地址：

```zsh
⇒  git remote -v
origin	git@github.com:RobinGG/RobinGG.github.io.git (fetch)
origin	git@github.com:RobinGG/RobinGG.github.io.git (push)
```

执行 `git remote --help` 你能看到一些关于 remote 命令的说明。这个命令是用来帮助你管理你所追踪的远端仓库的。

在你把项目代码从 git 服务器上面 clone 下来的时候，就已经默认将服务器上面的地址加到了管理列表里面。所以同事下载了代码，我就能通过他找到代码仓库的地址。

git remote 允许你再自己添加其他的仓库地址。比如你 fork 了一个项目，然后过了一段时间，代码跟不上最新的原代码了。你想要更新你 fork 的项目。这个时候就可以通过 `git remote` 来实现。
1. 通过 `git remote add <name> <url>` 把原来代码的服务器地址加进来
2. 通过 `git fetch <name>` 把最新的代码拉取到本地
3. merge 代码：`git merge <name>/master`
4. push 代码：`git push`

