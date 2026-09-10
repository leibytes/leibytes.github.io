---
title: '【OS_Linux】VMware 中安装CentOS7'
date: 2020-07-31 04:43:00
categories:
  - OS_Linux
------

## 1、CentOS7安装包的下载

Centos7的安装包可以去官网（<https://www.centos.org/>）下载，但速度比较慢。

C语言技术网（www.freecplus.net）的资源下载栏目，收集了国内Linux的镜像站点信息，国内镜像站点的安装包文件与官网同步，下载的速度非常快。

## 2、在VMWare中创建新的虚拟机

![](/images/OS_Linux/VMware_中安装CentOS7_004.png)

## 3、新建虚拟机向导

类型配置选择“典型”。

![](/images/OS_Linux/VMware_中安装CentOS7_010.png)

## 4、安装来源

安装来源选择选择“安装程序光盘映象文件”，点击“浏览”找到CentOS7的安装文件。

![](/images/OS_Linux/VMware_中安装CentOS7_009.png)

## 5、指定CentOS7的安装位置

虚拟机名称由您自定义。

安装位置保证有足够的磁盘空间。

![](/images/OS_Linux/VMware_中安装CentOS7_005.png)

## 6、指定磁盘容量和存放方式

大最磁盘大小20G基本上够用了，将虚拟磁盘存储为单个文件方便管理。

![](/images/OS_Linux/VMware_中安装CentOS7_006.png)

## 7、开始创建虚拟机

其它的参数用默认的，因为虚拟机完装完成后随时可以调整。

![](/images/OS_Linux/VMware_中安装CentOS7_015.png)

## 8、虚拟机打不开，提示“此主机不支持虚拟化实际模式”的解决方法

在VMWare软件中，安装/启动虚拟机时，如果出以类似以下的错误提示：

![](/images/OS_Linux/VMware_中安装CentOS7_016.png)

 出现该提示是由于电脑不支持虚拟化技术或是相关功能没有开启导致的。

解决方法有两种：

1）重启电脑，进入BIOS，开启VT虚拟化相关选项，不同品牌的电脑BIOS参数不同，认真看，多尝试。类似如下：

![](/images/OS_Linux/VMware_中安装CentOS7_003.png)

 2）修改C:\ProgramData\VMware\VMware Workstation\config.ini文件，注意，您的VMWare安装目录不一定是C:\ProgramData\VMware\VMware Workstation，根据您的实际情况来。

在config.ini中添加以下内容：

```
monitor.allowLegacyCPU = "true"
```

![](/images/OS_Linux/VMware_中安装CentOS7_001.png)

 通过法1或者法2后重新打开VMWare软件，问题解决。

## 9、选择安装过程中的语言

注意，这里所选的语言是指安装界面显示的语言，并不是CentOS7操作系统的语言。

![](/images/OS_Linux/VMware_中安装CentOS7_007.png)

## 10、安装信息摘要

![](/images/OS_Linux/VMware_中安装CentOS7_012.png)

 软件选择这里默认是最小安装，我们将其改为GNOME桌面

![](/images/OS_Linux/VMware_中安装CentOS7_011.png)

 安装位置点进去选择默认的“自动配置分区”即可

![](/images/OS_Linux/VMware_中安装CentOS7_008.png)

 配置完后的安装信息摘要如下图：

![](/images/OS_Linux/VMware_中安装CentOS7_002.png)

## 11、设置root用户（系统管理员）的密码

在Linux系统中，系统管理员用户名是root，也称为根用户。

![](/images/OS_Linux/VMware_中安装CentOS7_014.png)

## 12、系统安装中

等待安装完成，重启。

![](/images/OS_Linux/VMware_中安装CentOS7_013.png)

 参考博文：[CentOS7的安装和配置](https://www.cnblogs.com/wucongzhou/p/13173343.html)      [虚拟机VMware Workstation安装CentOS7](https://www.cnblogs.com/sunfairy/p/10869769.html)
