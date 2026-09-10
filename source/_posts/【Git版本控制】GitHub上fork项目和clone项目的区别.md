---
title: '【Git版本控制】GitHub上fork项目和clone项目的区别'
date: 2019-05-21 03:23:00
categories:
  - Git版本控制
------

fork：在github页面，点击fork按钮，将别人的仓库复制一份到自己的仓库。

clone：直接将github中的仓库克隆到自己本地电脑中

问题1：pull request的作用

比如在仓库的主人（A）没有把我们添加为项目合作者的前提下，我们将A的某个仓库名为“B”的仓库clone到自己的电脑中，在自己的电脑进行修改，但是我们会发现我们没办法通过push将代码贡献到B中。

所以要想将你的代码贡献到B中，我们应该：

1. 在A的仓库中fork项目B （此时我们自己的github就有一个一模一样的仓库B，但是URL不同）
2. 将我们修改的代码push到自己github中的仓库B中
3. pull request ，主人就会收到请求，并决定要不要接受你的代码
