---
title: '【Git版本控制】git将单个文件回退到某一版本'
date: 2019-05-25 07:35:00
categories:
  - Git版本控制
------

暂定此文件为a.jsp

1.进入到a.jsp所在目录，通过 git  log a.jsp查看a.jsp的更改记录

![](/images/Git版本控制/git将单个文件回退到某一版本_001.png)

2.找到想要回退的版本号：例如 fcd2093

通过 git reset   fcd2093  a.jsp先将本版库和暂存区中的该文件回退到历史版本fcd2093

3.再用暂存区中该文件的历史版本（fcd2093）覆盖工作区中对应的文件，达到工作区、暂存区和版本库三者间的统一。

git checkout --a.jsp

【注】git reset [选项]  [版本号]  [回退对象]命令，当回退对象是文件时选项不能为hard
