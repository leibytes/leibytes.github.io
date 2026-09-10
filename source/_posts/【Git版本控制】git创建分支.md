---
title: '【Git版本控制】git创建分支'
date: 2020-02-28 08:19:00
categories:
  - Git版本控制
------

开发中，经常需要从一个已有的分支拉出一个新分支，去这个新分支做一些开发改动，这里示例为：

从master分支，重新拉取出一个新的分支，名字为dev,具体命令如下：

1. 切换到被copy的分支（master），并从服务器拉取最新版本

$git checkout master  
$git pull

2. 从当前分支copy出新的开发分支 命名dev分支

$git checkout -b dev  
Switched to a new branch 'dev'

3. 把新建的分支push到远端

$git push origin dev

4. 拉取远端分支

$git pull  
There is no tracking information for the current branch.  
Please specify which branch you want to merge with.  
git branch --set-upstream-to=origin/<branch> dev

pull时发现，当前的分支并没有和本地分支关联，根据提示进行下一步：

5. 关联

$git branch --set-upstream-to=origin/dev

6. 再次拉取 验证

$git pull

参考博文：[git---从已有分支拉出新分支](https://blog.csdn.net/weixin_39800144/article/details/78973262)
