---
title: '【OS_Linux】查看Linux系统版本的命令'
date: 2020-08-03 04:16:00
categories:
  - OS_Linux
------

## 查看Linux系统版本的命令的方法

1、各版本通用的方法，查看 `/etc/os-release` 文件

```
cat /etc/os-release
```

![](/images/OS_Linux/查看Linux系统版本的命令_001.png)

2、使用lsb\_release -a命令【Linux Standard Base】（适用于RedHat、SUSE、Debian…等发行版，需安装 `lsb-release` 包）

如果运行命令提示不可用，需先安装对应包：

* Debian/Ubuntu: `sudo apt install lsb-release`
* CentOS/RHEL: `sudo yum install redhat-lsb-core`

```
[root@tv1ccmsbccolrjg ~]$lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.9.2009 (Core)
Release:        7.9.2009
Codename:       Core
```

参考博文：[查看Linux系统版本](https://www.cnblogs.com/ywl925/p/11083351.html)
