---
title: '【Java_基础】cmd下使用java命令运行class文件提示“错误：找不到或无法加载主类“的问题分析'
date: 2019-02-27 06:54:00
categories:
  - Java_基础
------

# 1.问题如下

当在命令行使用java命令执行字节码文件时提示“错误：找不到或无法加载主类”

![](/images/Java_基础/cmd下使用java命令运行class文件提示“错误：找不到_001.png)

# 2. 问题分析

这是由于在运行时类的全名应该是包名+类名，例如在包net.xsoftlab.baike下的类SelfDefineClassLoader的全名应为net.xsoftlab.baike.SelfDefineClassLoader。

# 3.问题的解决

java命令后跟类的全名，执行时会根据字节码文件的路径加上类的全名来寻找class文件的位置，需注意路径中不能再含有类全名中包路径部分。

当我们以java命令后跟类的全名执行字节码文件时依然出错，例如：

![](/images/Java_基础/cmd下使用java命令运行class文件提示“错误：找不到_003.png)

原因在于，我们在main目录下让java命令去执行org.will.app.main.NewsManager,其实它会以为类的路径是：

D:\HelloWorld\src\org\will\app\main\org\will\app\main\NewsManager，路径重复了。

所以，我们应该这样执行：

![](/images/Java_基础/cmd下使用java命令运行class文件提示“错误：找不到_002.png)

具体可参考博文：[使用java命令运行class文件提示“错误：找不到或无法加载主类“的问题分析](https://www.cnblogs.com/wangxiaoha/p/6293340.html)
