---
title: '【Oracle】修改oracle数据库的字符集'
date: 2020-12-17 13:52:00
categories:
  - DB_Oracle
------

由于安装Oracle数据库时未勾选正确的字符集，导致数据库的字符集与我们需要的不一样，我们可以通过如下步骤来修改。

### 1、查出oracle server 端当前的字符集

select userenv('language') from dual

查询结果中NLS\_CHARACTERSET为server端字符集，NLS\_LANGUAGE 为 server端字符显示形式。

### 2、修改 server端字符集

下面将Oracle数据库的字符集改为ZHS16GBK

```
将数据库启动到RESTRICTED模式下做字符集更改： 
SQL> conn /as sysdba 
Connected. 
SQL> shutdown immediate; 
Database closed. 
Database dismounted. 
ORACLE instance shut down. 
SQL> startup mount 
ORACLE instance started. 
Total System Global Area  236000356 bytes 
Fixed Size                   451684 bytes 
Variable Size             201326592 bytes 
Database Buffers           33554432 bytes 
Redo Buffers                 667648 bytes 
Database mounted. 
SQL> ALTER SYSTEM ENABLE RESTRICTED SESSION; 
System altered. 
SQL> ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0; 
System altered. 
SQL> ALTER SYSTEM SET AQ_TM_PROCESSES=0; 
System altered. 
SQL> alter database open; 
Database altered. 
SQL> ALTER DATABASE CHARACTER SET ZHS16GBK; 
ALTER DATABASE CHARACTER SET ZHS16GBK 
* 
ERROR at line 1: 
ORA-12712: new character set must be a superset of old character set 
提示我们的字符集：新字符集必须为旧字符集的超集，这时我们可以跳过超集的检查做更改： 
SQL> ALTER DATABASE character set INTERNAL_USE ZHS16GBK; 
Database altered. 
SQL> select * from v$nls_parameters; 
略 
19 rows selected. 
重启检查是否更改完成： 
SQL> shutdown immediate; 
Database closed. 
Database dismounted. 
ORACLE instance shut down. 
SQL> startup 
ORACLE instance started. 
Total System Global Area  236000356 bytes 
Fixed Size                   451684 bytes 
Variable Size             201326592 bytes 
Database Buffers           33554432 bytes 
Redo Buffers                 667648 bytes 
Database mounted. 
Database opened. 
SQL> select * from v$nls_parameters; 
略 
19 rows selected.
```

参考博文：[oracle数据库的字符集更改](https://blog.csdn.net/weixin_34238642/article/details/93320761)
