---
title: '【OS_Linux】centos与ubuntu的区别'
date: 2020-07-28 04:04:00
categories:
  - OS_Linux
------

CentOS（Community ENTerprise Operating System）是Linux发行版之一，它是来自于Red Hat Enterprise Linux依照开放源代码规定释出的源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以CentOS替代商业版的Red Hat Enterprise Linux使用。

Ubuntu是一个以桌面应用为主的Linux操作系统，其名称来自非洲南部祖鲁语或豪萨语的“ubuntu”一词（译为吾帮托或乌班图），意思是“人性”、“我的存在是因为大家的存在”，是非洲传统的一种价值观，类似华人社会的“仁爱”思想。Ubuntu基于Debian发行版和GNOME桌面环境，与Debian的不同在于它每6个月会发布一个新版本。Ubuntu的目标在于为一般用户提供一个最新的、同时又相当稳定的主要由自由软件构建而成的操作系统。Ubuntu具有庞大的社区力量，用户可以方便地从社区获得帮助。

## centos与ubuntu的区别

两者同为目前版本中个人和小团队常用的服务级操作系统，在线提供的软件库中可以很方便的安装到很多开源的软件及库。 两者都使用bash作为基础shell，所以在很多基础命令上，ubuntu与centos的差别不是很明显，而ubuntu在桌面界面上要做的更为出色，很多人如果是从兴趣出发而学习linux的首选一般都是ubuntu，毕竟很多人还是很习惯在桌面图形下操作的。Centos与Ubuntu的使用习惯和命令上还是有很多的不同，下面简单列举一下：

* centos中新建的非root用户是没有sudo的权限的，如果需要使用sudo权限必须在/etc/sudoers 中加入账户和权限，所以切换到root账号的时候只需要输入：su后输入root账号的密码即可。在Ubuntu中，一般使用sudo+命令，如果是第一次使用会提示输入当前用户的密码（而不是root的密码），如果要切换到root用户，需要输入sudo passwd 设置root用户密码。然后再su root。
* 在线安装软件中，centos使用的是yum命令，而ubuntu中使用的是apt-get命令。除此之外yum中还有一个从软件源中搜索摸个软件的方法：yum search +软件名
* centos是来自于redhat，所以centos支持rpm格式的安装，而ubuntu显然是不支持的。
* 毕竟是不同的公司做的不同的发行版，很多配置文件的位置和默认的文件路径都有很大区别，这个需要使用过程中慢慢体会了。
* Centos是基于Redhat开源构建的，服务器系统用的最多，Ubuntu是程序员开发环境，桌面环境用的最多得。
* 对于ubuntu而言，就是linux操作系统的具体，而linux对于ubuntu来说就是他的抽象；在linux操作系统中，因为应用程序和管理策略的不同，有多个版本，例如：ubuntu,fedora,redhat,centos等；
* Linux是开放源代码的，所以网上会出现各种各样的发行版本，Ubuntu Linux就是其中一种。Ubuntu采用Linux内核，图形界面采用GNOME（Kubuntu使用KDE）。简而言之，Linux系统是个统称，它有Red Hat、Debian、Suse、Ubuntu等发行版本，它们都是用的Linux内核，都是Linux系统。
* 非常多的商业公司部署在生产环境上的服务器都是使用CentOS系统，Centos是从Redhat源代码编译重新发布版，Centos去除很多与服务器功能无关的应用，系统简单但非常稳定，命令行操作可以方便管理系统和应用，并且有帮助文档和社区的支持。
* Ubuntu系统有着靓丽的用户界面，完善的包管理系统，强大的软件源支持，丰富的技术社区，并且Ubuntu对计算机硬件的支持优于centos和Debian，兼容性强，Ubuntu应用非常多，但是对于服务器操作系统来说，并不需要太多的应用程序，需要的是稳定，操作方便，维护简单的系统。如果你需要在服务器端使用图形界面，Ubuntu是一个不错的选择，你需要注意的是，图形界面占用的内存非常大，而内存越大的vps 价格也越高。

参考博文：[centos系统与ubuntu系统的区别](https://www.cnblogs.com/shareAndStudy/p/12547491.html "centos系统与ubuntu系统的区别")
