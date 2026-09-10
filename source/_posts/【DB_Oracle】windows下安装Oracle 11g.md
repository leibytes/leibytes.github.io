---
title: '【DB_Oracle】windows下安装Oracle 11g'
date: 2020-08-13 09:33:00
categories:
  - DB_Oracle
------

# 1.下载Oracle 11g安装包

安装之前要先确定自己的电脑配置，以windows为例，如果是win7以下系统如xp等可以选择Oracle 10g,因为10g的程序文件只有200多兆，而11g及达到了2G。具体文件官方下载路径已经找不到了，只能去一些软件下载平台下载。如果是win7最好采用11g，否则很可能10g出现不兼容安装不成功的情况，本文以win7为例。

Oracle 11g官方下载地址如下：<http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html>

下载首先需要同意协定，即选择“Accept License Agreement”选项。

![](/images/DB_Oracle/windows下安装Oracle_11g_018.png)

然后选择Oracle版本及相应操作系统，本处选择11g windows 64位系统，并下载File1和File2两个安装文件（本处需要先在Oracle官网注册账户，到邮箱进行激活）：

![](/images/DB_Oracle/windows下安装Oracle_11g_014.png)

下载完成后分别解压两个文件，一定要将解压后的两个文件夹合并成一个，否则在安装时会出现缺少文件的提示，可以将较小的文件剪切进较大的文件内，提示是否覆盖，选择是即可，因为只有文件夹名相同，内容并不同的。

# ****2.安装**Oracle 11g**

1.进入解压双击setup.exe即可开始安装。

2.把下图中的对勾取消，点击下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_020.png)

 弹出下图这个后点‘是'

![](/images/DB_Oracle/windows下安装Oracle_11g_013.png)

 3.下图中选择创建和配置数据库，点击下一步。

![](/images/DB_Oracle/windows下安装Oracle_11g_003.png)

 4.下图中选择服务器类，因为桌面类没有高级选项的功能，下一步。

![](/images/DB_Oracle/windows下安装Oracle_11g_021.png)

 5.下图中选择单实例数据库，下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_002.png)

 6.下图中选择高级安装，下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_025.png)

 7.下图选择语言，如果数据库内没有用到其他的语言，就这样默认的就可以了，下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_011.png)

  8.下图中选择安装企业版，下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_004.png)

  9.下面，选择基目录的时候要注意了，最好去新建一个自定义的目录，以后方便自己查询和更改，不要用这个默认的，卸载的时候麻烦。下面说一下怎么新建一个自定义目录。

![](/images/DB_Oracle/windows下安装Oracle_11g_026.png)

 10.新建自定义目录：在d盘下新建一个文件夹，取名为myoracle,进入这个文件夹，在里面新建一个文件夹，取名oracle. 注意：这两个文件夹的名字可以自己定义，但是最好不要用中文名作文件夹的名字，容易出问题，最好是如下这样的目录结构。如图，就是这个样子的：

![](/images/DB_Oracle/windows下安装Oracle_11g_019.png)

  11.继续安装，点击‘oracle基目录'后面的那个浏览：

![](/images/DB_Oracle/windows下安装Oracle_11g_027.png)

  12.选择刚新建的安装目录：

![](/images/DB_Oracle/windows下安装Oracle_11g_012.png)

 13.上面的选择好后，就应该是下面这个样子的：基目录选择好后，软件位置就会自动选择好了，不用管下面的那个浏览了，直接点击下一步。

![](/images/DB_Oracle/windows下安装Oracle_11g_008.png)

14.下图，选择‘一般用途事务处理'直接点击下一步：

![](/images/DB_Oracle/windows下安装Oracle_11g_024.png)

 15.下图，‘全局数据库名'，可以自己定义。下面的服务标识符也是可以自己定义。一般这两个就使用默认的了。下一步。

![](/images/DB_Oracle/windows下安装Oracle_11g_028.png)

 16.下图，就按照默认的来吧，点击下一步。

![](/images/DB_Oracle/windows下安装Oracle_11g_007.png)

 17.下图也选择默认，下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_010.png)

 18.下图，还是默认的，点击下一步。

![](/images/DB_Oracle/windows下安装Oracle_11g_023.png)

 19.下图，直接下一步

![](/images/DB_Oracle/windows下安装Oracle_11g_005.png)

 20.下图，配置口令，如果怕麻烦的话，如果仅仅是做学习用的话，统一口令就行。如果以后高级了，可以分别设置口令。这几个用户权限是不同的，我就用统一口令oracle。注意，口令必须以字母开头，数字开头的话后面会有很多问题的。

![](/images/DB_Oracle/windows下安装Oracle_11g_030.png)

 21.输入上面那个oracle后，会弹出如下：在这里要注意了，oracle的密码标准是要字母数字大小写组合的大于八位才行，一般要是仅供学习用的话不用在意这个。直接点‘是'就行。

![](/images/DB_Oracle/windows下安装Oracle_11g_001.png)

 22.下图，检测环境：

![](/images/DB_Oracle/windows下安装Oracle_11g_015.png)

 23.环境检测完后，有的是可以直接点击下一步的，有的会出现如下图：这个时候，一般是下图这样的，那么就应该没多大问题，选择全部忽略，再点击下一步就行了。

![](/images/DB_Oracle/windows下安装Oracle_11g_022.png)

 24.会出现如下：点击完成，就可以开始安装了。

![](/images/DB_Oracle/windows下安装Oracle_11g_029.png)

 25.开始安装了：安装比较慢，安装完数据库软件后，还会接着创建一个数据库实例，所以要等上好一会儿。

![](/images/DB_Oracle/windows下安装Oracle_11g_017.png)

 26.安装完后会弹出下图，点击确定就可以完成了。

![](/images/DB_Oracle/windows下安装Oracle_11g_006.png)

 这样就完成oracle的安装了，测试一下，打开oracle自带的sql plus。

![](/images/DB_Oracle/windows下安装Oracle_11g_016.png)

 用户名：system.密码口令：oracle.(就是在20步自己定义的那个口令)。顺利进入：注意oracle这里输入的口令是不显示的，用了像linux登录的方式，在这 里就可以输入sql语句了。

![](/images/DB_Oracle/windows下安装Oracle_11g_009.png)

 参考博文：[windows下oracle 11g r2 安装过程与卸载详细图解](https://www.cnblogs.com/activiti/p/7575015.html)     [Oracle 11g数据库安装与卸载的方法图解（windows）](https://www.cnblogs.com/activiti/p/7575005.html)
