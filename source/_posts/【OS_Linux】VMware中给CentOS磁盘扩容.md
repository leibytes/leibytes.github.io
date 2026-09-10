---
title: '【OS_Linux】VMware中给CentOS磁盘扩容'
date: 2020-08-25 07:42:00
categories:
  - OS_Linux
------

当VMware中的CentOS磁盘空间不足时就需要进行磁盘扩容，具体步骤如下：

## 步骤一：虚拟机扩展磁盘容量

1、首先，打开已经安装好的虚拟机，选择需要扩展磁盘的虚拟机，点击设备中的“硬盘”。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_003.png)

 2、接着，进入虚拟机设置界面，在界面的右侧选择“扩展”。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_028.png)

 3、接着，在弹出的扩展磁盘容量中，输入需要扩展磁盘的大小，完成后点击“确定”。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_002.png)

 4、接着，等待系统自动扩展完成，在弹出的对话框中选择“确定”，这样虚拟机的磁盘就扩展完成了。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_032.png)

 5、接着，回到虚拟机设置界面，点击下方的“确定”保存刚设置的参数。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_009.png)

 这样，虚拟机扩展磁盘容量就完成了，接下来进入centos扩展磁盘容量。

## 步骤二：在centos 7 系统中挂载磁盘

1、首先，开机启动虚拟机，进入centos 7系统中，打开终端，在终端界面中输入“# df -h”命令，并回车，在终端界面中可以看到原本30G的磁盘，并未扩容。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_018.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_015.png)

 2、接着，在终端界面中输入“# fdisk -l”命令，查看新磁盘的分区信息。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_016.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_029.png)

 3、接着，在终端界面中输入“# fdisk  /dev/sda”命令，对新加的磁盘进行分区操作（选择新增的磁盘），并回车。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_030.png)

 4、接着，在终端命令中，输入“p”命令，回车，查看已分区的数量。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_017.png)

 5、接着，在终端命令中，输入“n”命令，回车，新增一个分区。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_006.png)

 6、接着，在终端命令窗口中输入“p”命令,回车，在打开的分区号命中使用默认的分区号，并回车。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_027.png)

 7、接着，在显示的起始扇区直接敲回车键，在弹出的Last后面中，直接敲回车键即可进入下一步（在如图画框位置直接使用回车即可进入下一步）。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_024.png)

 8、接着，继续在终端命令窗口中输入“p”回车，查看当前分区的信息，在显示的信息中就可以看到已经多了一个新的分区了，大小就是刚设置的大小。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_021.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_033.png)

 9、接着，在终端命令中输入“w”命令，回车，写入磁盘信息并保存。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_013.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_012.png)

 10、接着，在终端命令窗口中，输入“# reboot”回车，重启虚拟机格式化新建分区。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_022.png)

 11、接着，等待系统自动重启完成后，打开终端命令，在命令窗口中输入“# vgdisplay”，查看磁盘卷组名。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_020.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_001.png)

 12、接着，在终端命令窗口中输入“# pvcreate  /dev/sda3”命令并回车，初始化刚建立的分区。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_014.png)

 13、接着，在终端命令窗口中输入“# vgextend centos /dev/sda3”命令并回车，把刚初始化的分区加入到虚拟卷组名中（命令：# vgextend 虚拟卷组名 新增的分区）。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_005.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_010.png)

 14、接着，继续在终端命令窗口中输入“# vgdisplay”命令，查看卷组的详细信息，在详细信息中可以看到刚增加的10G空间还是处于空闲的状态。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_019.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_004.png)

 15、接着，继续在终端命令窗口中输入“# df -h”命令并回车，查看并记下需要扩展的文件系统名（小编这边查看到的系统名是：/dev/mapper/centos-root，不同版本查看到的应该有所区别，大家应该注意区分）。如图：

**![](/images/OS_Linux/VMware中给CentOS磁盘扩容_025.png)**

 16、接着，继续在终端命令窗口中输入“# lvextend -L +9G /dev/mapper/centos-root”命令并回车，扩容已有的卷组容量（注意：如果您扩容的是10G，这里10G就不能全部扩展，只能扩展比10G小的容量，不然系统会报错导致扩容失败，小编这里只扩容9G。命令：# lvextend -L +需要扩展的容量 需要扩展的文件系统名，**需要注意命令中区分字母的大小写**）。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_031.png)

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_023.png)

 17、接着，继续在终端命令窗口中输入“# pvdisplay”命令并回车，查看当前的卷组，在显示的信息中卷组已经扩容成功了，接下来需要将文件系统也扩容。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_011.png)

18、接着，继续在终端命令窗口中输入“# cat /etc/fstab | grep centos-root”命令并回车，查看文件系统的格式。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_007.png)

 19、接着，在上一步已经看到了文件系统的格式了，就需要使用相应的命令来扩展磁盘空间，在终端命令中输入“# xfs\_growfs /dev/mapper/centos-root”命令并回车（命令：# xfs\_growfs 文件系统名，不同的文件系统要用不同的命令，不然会导致报错）。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_026.png)

 20、接着，等待系统自动扩容完成，完成后，在终端命令窗口中输入“# df -h”命令并回车，查看磁盘大小是否扩容成功，在显示的磁盘信息中，可以看到磁盘扩容成功了。如图：

![](/images/OS_Linux/VMware中给CentOS磁盘扩容_008.png)

 21、至此，VMware虚拟机如何设置CentOS 7 磁盘扩容教程分享结束。

参考博文：[VMware虚拟机如何设置CentOS 7 磁盘扩容？](https://jingyan.baidu.com/article/ca00d56cad34dda89febcf7b.html)
