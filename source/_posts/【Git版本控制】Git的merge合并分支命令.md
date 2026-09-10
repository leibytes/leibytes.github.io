---
title: '【Git版本控制】Git的merge合并分支命令'
date: 2019-05-24 10:13:00
categories:
  - Git版本控制
------

1.实例

git checkout master

git merge dev

merge合并分支只对当前分支master产生影响，被合并的分支dev不受影响。

假设你有两个分支，“stable” 和 “new-idea”, 它们的顶端版本 为E 和 F

**合并前：**

```
A-----C----E ("stable")
   \
    B-----D-----F ("new-idea")
```

查看分支stable的提交历史将显示A、C、E

查看分支new-idea的提交历史将显示B、D、F

**将new-idea分支合并到stable分支后：**

```
A-----C----E----G ("stable")
   \             /
    B-----D-----F ("new-idea")
```

stable分支的提交历史将会包含new-idea分支的提交历史，即为A, B, C, D, E, F, G（各次提交都有时间记录），但分支new-idea的提交历史不受影响。

【注意】每次开发新功能前都要先拉取远程master分支与本地开发分支合并，尽量保证是在最新版本基础上的修改，避免不同步导致修改冲突。
