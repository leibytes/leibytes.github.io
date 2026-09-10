---
title: '【OS_Windows】Windows10系统可以禁止的服务（按名称排序）'
date: 2019-04-13 15:39:00
categories:
  - OS_Windows
------

1、Application LayerGateway Service（Windows必须禁止的10项服务）

2、Bluetooth Handsfree Service（没有蓝牙的用户可以关闭）

3、Bluetooth Support Service（没有蓝牙的用户可以关闭）

4、Computer Browser（维护网络上计算机的更新列表，不需要更新的话可以改为手动）

5、Diagnostic Policy Service（诊断策略服务）

6、Distributed Link Tracking Client（维护计算机内的NTFS文件之间的链接）

7、Fax（利用计算机或网络上的可用传真资源发送和接收传真）

8、IP Helper（使用IPv6转换技术）

9、Offline Files（脱机文件服务）

10、Performance Logs & Alerts（性能日志和警报）

11、Portable Device Enumerator Service（强制可移动大容量存储设备的组策略）

12、Program Compatibility Assistant Service（为应用程序兼容性助手提供支持）

13、Print Spooler（没有打印机的话可以关闭它）

14、Remote Registry（远程注册表设置，这项服务应该保持禁用状态）

15、Secondary Logon（在不同凭据下启用启动过程，改为手动即可，禁用会导致魔兽世界等游戏无法登录）

16、Security Center（监视并报告计算机上的安全健康设置）

17、Server （支持计算机通过网络进行共享，若处于单机状态的话，可以改为手动）

18、TCP/IP NetBIOS Helper （提供NetBIOS名称解析支持，若你的计算机没有连接到工作组网络的话，可以改为手动）

19、Windows Error Reporting Service （报告错误并提供现有解决方案）

20、Windows Image Acquisition （WIA） （为扫描仪和照相机提供图像采集服务）

21、Windows Search （提供内容索引，如果你不怎么使用Windows自带搜索的话，可以改手动）

22、Windows Time （维护时间和日期同步）  
以上就是Windows10系统可以禁止或设置为手动的服务。如果关闭这些服务后电脑出现问题，可以将其开启。

【注】有些服务是绝对不能禁止的，你像Network List Service（网络列表服务）被禁止后，win10自带的搜索框就会出现闪退用不了。
