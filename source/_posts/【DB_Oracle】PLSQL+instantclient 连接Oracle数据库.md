---
title: '【DB_Oracle】PLSQL+instantclient 连接Oracle数据库'
date: 2020-04-14 05:55:00
categories:
  - DB_Oracle
------

instantclient是一个轻量级的连接Oracle数据库的中间件，解压即可用。

**安装过程如下：**

1.下载instantclient并解压到指定的目录（注：32位的PLSQL就下载32位的客户端，64位的PLSQL就下载64位的客户端，如果不对应将连接不上）

下载地址：<http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html>

**注意:**Oracle客户端向下兼容，故下载最新版本的instantclient就好

![](/images/DB_Oracle/PLSQL+instantclient_连接Oracle数据_005.png)

 2、PLSQL借助instantclient 连接Oracle数据库的两种方法

**法一：**通过配置tnsnames.ora文件，并将tnsnames.ora文件所在的目录加入到环境变量。

具体的操作为：在解压的instantclient目录下新建配置文件tnsnames.ora并写入待连接数据库的配置信息，tnsnames.ora文件可直接从服务器端复制后再做修改。

tnsnames.ora文件的配置参考：[DB\_Oracle】Oracle中tnsnames.ora文件的作用](https://www.cnblogs.com/leiblog/p/14429584.html)

配置环境变量TNS\_ADMIN（值为tnsnames.ora文件所在的目录）

![](/images/DB_Oracle/PLSQL+instantclient_连接Oracle数据_001.png)

 然后在PLSQL的登录界面下拉选择数据库

![](/images/DB_Oracle/PLSQL+instantclient_连接Oracle数据_002.png)

**法二：**不配置tnsnames.ora文件，为PLSQL指定OCI路径，然后通过ip:port/实例进行连接

![](/images/DB_Oracle/PLSQL+instantclient_连接Oracle数据_004.png)

![](/images/DB_Oracle/PLSQL+instantclient_连接Oracle数据_003.png)

   参考博文：[oracle instantclient + plsql 远程连接数据库](https://www.cnblogs.com/lelehellow/p/6801800.html)
