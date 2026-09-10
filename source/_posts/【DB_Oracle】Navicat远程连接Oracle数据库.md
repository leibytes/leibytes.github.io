---
title: '【DB_Oracle】Navicat远程连接Oracle数据库'
date: 2020-04-14 08:44:00
categories:
  - DB_Oracle
------

1.下载Navicat安装，32位机器安装32位，64位机器安装64位，下面给出一个32位的安装包及注册机。

Navicat 32位下载链接：<https://pan.baidu.com/s/13caq6EUtfZUjiUugPltScA>   
提取码：ggyp

2.在oracle官网下载instantclient-basic和instantclient-sqlplus这两个即使客户端压缩包

地址：<https://www.oracle.com/database/technologies/instant-client/downloads.html>

根据你安装的Navicat版本，安装了32位的Navicat选择32位的instantclient，64位的Navicat选择64位的instantclient，否则会出现can not load oci.dll 193错误。

![](/images/DB_Oracle/Navicat远程连接Oracle数据库_001.png)

![](/images/DB_Oracle/Navicat远程连接Oracle数据库_003.png)

3.分别解压两个包，Navicat中打开工具-选项-OCI，将OCI指定为instantclient-basic中对应的oci，将SQL\*Plus指定为instantclient-sqlplus中对应的sqlplus，重启Navicat。

![](/images/DB_Oracle/Navicat远程连接Oracle数据库_002.png)

  4.重启Navicat后新建Oracle连接

![](/images/DB_Oracle/Navicat远程连接Oracle数据库_004.png)

**注意：**PLSQL和Navicat都可以通过OCI驱动连接Oracle数据库，但PLSQL是通过读取配置在本地客户端的tns文件里的数据库别名来连接的，Navicat无需在本地客户端配置tns文件，可以输入ip和服务名来连接。

 参考博文：[使用Navicat远程连接oracle数据库](https://blog.csdn.net/yy417168602/article/details/70215936)
