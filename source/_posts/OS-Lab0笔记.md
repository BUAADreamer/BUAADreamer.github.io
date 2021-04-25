---
title: OS-Lab0笔记
date: 2021-03-10 10:39:03
tags: BUAA-OS-2021
---

OS课程Lab0学习笔记

<!--more-->

# 基础操作

## 命令行

命令格式：`命令名 [选项] [参数]`

Linux命令在系统中有两种类型：`内置Shell（外壳）命令`和`Linux命令`

```shell
rm -rf #删除所有文件
```

### OS常用命令

```shell
find ./ test.md 	#查找文件
grep -r printf ./ 	#查找函数变量。。
```

## 运行小操作系统

```shell
/OSLAB/gxemul -E testmips -C R3000 -M 64 gxemul/vmlinux
```

