---
title: '【OS_Windows】用微pe制作启动盘安装操作系统'
date: 2021-01-31 12:14:00
categories:
  - OS_Windows
------

# 一、准备工作

u盘，电脑一台，win10原版镜像[（msdn官网）](https://msdn.itellyou.cn/)

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_012.png)

# 二、下载wepe工具箱

极力推荐微pe[（微pe官方下载）](http://www.wepe.com.cn/)

下载64位的win10 pe，使用工具箱制作U启动盘。

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_002.png)

 选择安装到U盘（默认设置无需更改）

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_007.png)

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_006.png)

# 三、重启进入pe系统

1、关机后，将U盘插入电脑

2、按下电源后，按住F12进入启动项选择（技嘉主板是F12）

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_001.png)

 3、进入PE系统

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_010.png)

# 四、进行硬盘分区

双击桌面的“分区工具DiskGeniUs”图标，打开工具，当硬盘为空闲状态时，点击“快速分区”按钮，进行硬盘分区操作，如图所示：

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_009.png)

**磁盘分区类型有两种：**

1、GPT 新型分区表类型，需对应UEFI启动，支持快速启动，支持2TB以上硬盘

2、MBR 传统分区表类型，需对应Legacy启动，旧机器上比较常见

**注意：磁盘分区类型决定了系统的启动类型，GPT分区只能采用UEFI启动模式，MBR 传统分区采用Legacy启动，如果分区类型与启动模式不匹配，开机会提示“No Bootable Device”，无法启动，此时需要进BIOS调整启动模式与分区相匹配。**

**![image](/images/OS_Windows/用微pe制作启动盘安装操作系统_003.png)**

 选择分区表类型为GUID，必须勾选创建ESP分区（系统启动是从ESP分区中查找并执行后缀为 .efi 的引导程序文件），MSR分区非必要，分区数目及分区大小根据自己的硬盘大小和个人习惯决定，建议系统分区大于50G，对齐分区是4K对齐，固态硬盘勾上，机械硬盘非必要。

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_005.png)

 选择好了之后点击确定等待分区完成，随后便可以进行系统安装。

# 五、安装系统

### #####

[原生安装方法](http://www.wepe.com.cn/ubook/installwin810.html)

### #####

（一）右键你要安装的镜像，选择装载

（二）利用桌面上的windows安装器进行初步安装

1.打开桌面上的windows安装器，界面如下：

      第一个框选择你装载那个镜像里面的install.wim，

      第二个框选择引导ESP分区所在的盘

      第三个框选择你想要安装系统的盘（一般是c盘）如图：

2.记得选择你的版本（图中选了专业版）

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_004.png)

3.三个标志都变绿说明设置成功，然后点击开始安装

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_008.png)

 按照图示选择，其他选项不用改，等待安装完成。

安装完成会重启（若没有重启则自己重启）， 进入真正的安装界面

（三）进入windows安装界面

然后等一会，去上个厕所的时间吧！

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_013.png)

 各种设置之后系统便完成了安装，进入桌面，win10一般不用装什么驱动。

![](/images/OS_Windows/用微pe制作启动盘安装操作系统_011.png)

 参考博文：[win10系统安装教程（U盘PE+UEFI安装）](https://blog.csdn.net/weixin_41964258/article/details/102322630?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

[微PE优盘使用说明书](http://www.wepe.com.cn/ubook/start.html)
