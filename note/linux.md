---
layout: page
title: Linux 碎碎念
---

## /etc/resolv.conf

这个文件是用来记录修改 DNS 的。其中 nameserver 最多设置3个（可以修改）。nameserver 只有在 timeout 的情况下才会查询下一个 nameserver。

> 详情参考：[RESOLV.CONF(5)](http://man7.org/linux/man-pages/man5/resolv.conf.5.html)