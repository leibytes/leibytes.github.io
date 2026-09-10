---
title: '【Git版本控制】git中reset命令的详解'
date: 2019-05-23 09:31:00
categories:
  - Git版本控制
------

# [git reset 命令详解（一）](https://blog.csdn.net/longintchar/article/details/81843048)

# [git reset 命令详解（二）](https://blog.csdn.net/longintchar/article/details/81952043)

reset命令的语法：git reset [选项]  [版本号]  [要回退的目标]

选项：--soft仅将head指针指向历史版本

          --mixed将head指针指向历史版本，且用历史版本的暂存区覆盖当前暂存区（默认的选项）

　　  --hard将head指针指向历史版本，同时覆盖工作区和暂存区的内容（比较危险慎用）

版本号：可以是某个绝对的版本号，也可以是用head指针指向的相对版本号（默认head所指的当前版本）

要回退的目标:可以是分支，也可以是文件（默认当前分支），当为文件时选项参数不能为hard。
