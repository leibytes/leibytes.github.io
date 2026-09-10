---
title: '【DB_Oracle】Linux下启动Oracle服务和监听程序'
date: 2020-08-25 08:40:00
categories:
  - DB_Oracle
------

**linux中启动oracle主要分两步：**一个是启动服务、**一个是启动监听器。**  
**方法/步骤**  
一、终端连接到数据库所在的linux机器，切换到oracle用户模式下  
[root@nstlbeta ~]# su - oracle

![](/images/DB_Oracle/Linux下启动Oracle服务和监听程序_001.jpg)

二、sqlplus下登录到数据库启动服务  
[oracle@nstlbeta bin]$ sqlplus /nolog  //登录sqlplus  
SQL> connect /as sysdba  //连接oracle  
SQL> startup //起动数据库服务  
SQL> exit  //退出sqlplus

![](/images/DB_Oracle/Linux下启动Oracle服务和监听程序_002.jpg)  
三、启动监听  
[oracle@nstlbeta bin]$ lsnrctl start  //起动监听

![](/images/DB_Oracle/Linux下启动Oracle服务和监听程序_003.jpg)
