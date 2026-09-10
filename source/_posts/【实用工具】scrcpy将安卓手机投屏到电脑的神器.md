---
title: '【实用工具】scrcpy将安卓手机投屏到电脑的神器'
date: 2021-03-31 14:03:00
categories:
  - 实用工具
------

scrcpy 是一款免费开源的投屏软件，支持将安卓设备投屏在 Windows、macOS、GNU/Linux 上，同时支持有线和无线投屏，其优点如下：

1、终端无需安装任何软件，只需在电脑上下载软件包解压后按照操作配置即可实现投屏  
2、支持无线投屏并且能通过电脑来操作终端上的APP

项目地址：[https://github.com/Genymobile/scrcpy](https://link.zhihu.com/?target=https%3A//github.com/Genymobile/scrcpy)

Windows 下载：[scrcpy-win64-v1.17.zip](https://link.zhihu.com/?target=https%3A//github.com/Genymobile/scrcpy/releases/download/v1.17/scrcpy-win64-v1.17.zip)

国内搬运链接: [https://wwx.lanzoux.com/iatIekv](https://link.zhihu.com/?target=https%3A//wwx.lanzoux.com/iatIekv736h)

## 1、有线投屏

需要在手机端开启 `开发者选项` 及 `USB 调试`，然后使用数据线将手机和电脑连接并允许 USB 调试，解压安装包后运行scrcpy.exe即可实现对手机的投屏。

![](/images/实用工具/scrcpy将安卓手机投屏到电脑的神器_001.png)

## 2、无线投屏

1. 确保 PC 和手机处于同一局域网中
2. 在 scrcpy.exe 文件所在目录打开命令窗口

```
# a.切换到scrcpy.exe 文件所在的目录
cd D:\Downloads\scrcpy-win64-v1.17
​
# b.在手机端开启「开发者选项」及「USB 调试」，然后使用数据线将手机和电脑连接并允许 USB 调试，开启手机端口
# 如果本行显示 no device 或未启动 adb，需检查「USB 调试」是否开启。
# 此外，一些手机需选择「文件传输」模式方能使用 adb。
 adb tcpip 5555
​
# c.拔出手机数据线，开始无线投屏。(192.168.2.234 为手机端 ip，需更改)
 adb connect 192.168.2.234:5555
​
# d.启动 scrcpy.exe
 scrcpy

# 如有报错，可启动低分辨率投屏
 scrcpy -m 1920
```

## 3、屏幕录制

同样在 scrcpy.exe 文件所在目录打开命令窗口

```
# 将代码目录定位到 scrcpy 文件夹
cd D:\Libraries\Desktop\scrcpy-win64-v1.17
​
# 开始录制，录屏文件会以命令指定的文件名自动保存在当前文件夹内。
scrcpy -r filename.mp4
# 关闭投屏窗口后，自动停止录屏并将视频保存在相应目录
```

## 4、Scrcpy的快捷键

进入到投屏界面后可通过快捷键进行操作

![](/images/实用工具/scrcpy将安卓手机投屏到电脑的神器_003.png)

 投屏后鼠标与手机键位的对应关系

![](/images/实用工具/scrcpy将安卓手机投屏到电脑的神器_002.png)

鼠标左键就是手机的确认键

鼠标右键就是安卓系统的返回键

鼠标中键就是手机的主页按钮

## 5、常见问题

1. 无线连接报错时，首先确认有线连接步骤是否正确，`开启 USB 调试`－`连接手机与电脑`－`启动 scrcpy`，然后检查手机的本机 IP 是否正确。90% 的错误都出在这两步。

2. `ERROR: Exception on thread Thread[main,5,main]` 此项错误多为手机不兼容 scrcpy 默认分辨率设置。解决方位为，按下方修改 scrcpy 启动代码，使用较低的分辨率。

```
# 三项设置，任选其一
.\scrcpy -m 1920
.\scrcpy -m 1024
.\scrcpy -m 800c
```

参考博文：

[scrcpy - 手机无线投屏到电脑](https://zhuanlan.zhihu.com/p/80264357?utm_source=qq)

[Scrcpy-在电脑无缝操作手机 (投屏/录屏/免Root)](https://zhuanlan.zhihu.com/p/82427039)

[如何在电脑上控制手机上所有的app软件操作？](https://www.zhihu.com/question/46795475?sort=created)
