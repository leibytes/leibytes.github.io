---
title: '【Git版本控制】Git初始化一个仓库'
date: 2019-07-05 08:05:00
categories:
  - Git版本控制
------

1. git init //初始化一个本地库
2. git add -A//将所有的文件添加到暂存区
3. git commit -m “首次提交” //将暂存区的文件提交到版本库
4. git remote add origin + 远程仓库地址 //关联远程仓库
5. git push -u origin master //把本地仓库的文件推送到远程仓库

在第5步推送本地master分支到远程库可能出现如下错误：

![](/images/Git版本控制/Git初始化一个仓库_002.png)

 这是由于GitHub远程仓库中的`README.md`文件不在本地仓库导致的，因此需要先pull远程库的master分支再push本地分支。

`git pull origin master`后面跟上参数`--allow-unrelated-histories`

![](/images/Git版本控制/Git初始化一个仓库_001.png)

 参考博文：[git创建远程仓库并上传代码到远程仓库中](https://blog.csdn.net/liuweixiao520/article/details/78971221)
