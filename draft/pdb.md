---
layout: page
title: 使用 PDB 调试 python 代码
tags: [python]
---

## 简介
PDB 是 Python 里面的一个模块，可以用来对 Python 代码进行调试

## 运行 PDB

在官网文档中，提到了 3 种方式可以进入 PDB 的调试窗口

### 调试文件

可以在终端通过直接调用下面这行命令来调试简单的脚本：  
`python -m pdb file_to_debug.py`  
使用这个这种方式调试脚本会在脚本运行完毕后重新继续运行，无论脚本是正常退出还是抛错

### 调试正在运行的程序

 可以通过在代码中插入 PDB 代码的方式调试正在运行中的程序。  
 `import pdb; pdb.set_trace()`

### 调用 PDB 的函数

比如 run, runcall, pm
- `pdb.run(statement[, globals[, locals]])`: 

