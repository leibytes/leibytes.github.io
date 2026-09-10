---
title: '【OS_Linux】统计目录下文件的个数'
date: 2021-06-08 07:26:00
categories:
  - OS_Linux
------

### **1、统计目录下文件的个数（不统计子目录）**

当前目录：ls -l | grep "^-" | wc -l

任意目录：find dir  -type  f | wc -l（dir指定对应的目录）

参考文章：<https://www.linuxprobe.com/linux-ls-grep-wc.html>
