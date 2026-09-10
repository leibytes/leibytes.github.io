---
title: '【Java_基础】windows同时安装jdk7和jdk8'
date: 2020-07-15 03:12:00
categories:
  - Java_基础
------

有时候某个插件只支持jdk7，而电脑又安装的是jdk8，这时需要卸载jdk8再装jdk7，当想用jdk8时又得切换回去。能不能同时安装jdk7和jdk8呢？当然可以，步骤如下：

1.下载并安装jdk7和jdk8  
##下载地址  
jdk7百度网盘链接：https://pan.baidu.com/s/1Y6kfNM6s4xuO7tor0D6m1g 密码：ptja  
jdk8百度网盘链接：https://pan.baidu.com/s/1onM27vIM6HYjvyilT9MSgQ 密码：iisy

##我的jdk安装目录

![](/images/Java_基础/windows同时安装jdk7和jdk8_003.png)

  2.jdk配置修改

在安装JDK时，会将java.exe、javaw.exe、javaws.exe三个文件copy到了C:\Windows\System32，若先安装jdk1.7再安装的jdk1.8，则C:\Windows\System32中的是jdk1.8。这个目录在win环境变量中的优先级高于JAVA\_HOME设置的环境变量优先级。所以我们在JAVA\_HOME中如何配置都会显示出java8的信息，解决方案:

(1).删除C:\Windows\System32目录下的java.exe

(2).同时我们在我们环境变量中的path上自动配置了C:\ProgramData\Oracle\Java\javapath;我们需要把它删掉

3.JAVA\_HOME的配置  
(1).配置JAVA7\_HOME和JAVA8\_HOME变量，然后在JAVA\_HOME变量中引用JAVA7\_HOME或JAVA8\_HOME。 *![](/images/Java_基础/windows同时安装jdk7和jdk8_002.png)*

 (2).然后我们接着进入我们系统变量的path变量,添加%JAVA\_HOME%\bin和%JAVA\_HOME%\jre\bin

*![](/images/Java_基础/windows同时安装jdk7和jdk8_001.png)*

  (3).切换JDK7和JDK8

如果需要用到jdk7我们只需要把我们的JAVA\_HOME 引用JAVA7\_HOME，命令行输入java -version出现如下java version “1.7.0\_80”，切换jdk7成功

![](/images/Java_基础/windows同时安装jdk7和jdk8_004.png)

 如果需要用到jdk8我们只需要把我们的JAVA\_HOME引用JAVA8\_HOME，命令行输入java -version出现如下java version “1.8.0\_144”，切换jdk8成功

![](/images/Java_基础/windows同时安装jdk7和jdk8_005.png)

 参考博文：[windows同时安装jdk7和jdk8](https://blog.csdn.net/tzhenxiong/article/details/81869641)
