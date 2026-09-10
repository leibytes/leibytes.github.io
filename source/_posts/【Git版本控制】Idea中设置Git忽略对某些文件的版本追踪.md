---
title: '【Git版本控制】Idea中设置Git忽略对某些文件的版本追踪'
date: 2019-09-04 03:48:00
categories:
  - Git版本控制
------

在Idea中有些本地文件无需与远程库同步，仅是本地使用。此时就需要将这些文件加入到Git的版本忽略中来。

### 设置步骤

1、搜索插件 .ignore,并安装

![](/images/Git版本控制/img_fail_b92c8fd6.png)

2、增加.gitignore文件

![](/images/Git版本控制/img_fail_e37a3386.png)

3、配置相应过滤文件

![](/images/Git版本控制/img_fail_471453db.png)

### 解决加入到.gitignore的文件不能被忽略的问题

不能被忽略的原因是：对应的目录或者文件已经被git跟踪，此时再加入.gitignore后就无效了。

执行

[文件夹]  git rm -r --cached  文件夹

[文件]     git rm --cached 文件
