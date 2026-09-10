---
title: '【OS_Windows】windows下删除指定日期前的文件'
date: 2020-09-10 08:49:00
categories:
  - OS_Windows
------

```
@echo off
rem 设置要删除文件的目录
set SrcDir=D:\ERP_SERVER\JBOSS_BH\appserver_8027\server\default\log
rem 设置要删除几天前的文件
set DaysAgo=7
rem /p指定搜索文件的路径  /s 在子目录中搜索  /m 指定格式，默认为*.*   /d  选择日期（+大于 - 小于）   /c 指定执行的命令   del /f 强制删除  /q 不询问是否删除   /a 按指定属性删除
forfiles /p %SrcDir% /s /m *.log.* /d -%DaysAgo% /c "cmd /c del /f /q /a @path"
exit
```

 【注意】Windows环境下需以反斜杠“\”作为路径的分隔符

参考博文：[windows下删除指定日期前的文件](https://blog.csdn.net/woshiji594167/article/details/80653656)
