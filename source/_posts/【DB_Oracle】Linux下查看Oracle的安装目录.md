---
title: '【DB_Oracle】Linux下查看Oracle的安装目录'
date: 2021-02-22 04:25:00
categories:
  - DB_Oracle
------

有时候我们需要知道Oracle的安装目录，可以通过以下命令查得：

```
env |grep ORACLE
```

例如：

```
[root@instance-kgwl9gll oracle]# env |grep ORACLE
ORACLE_OWNER=oracle
ORACLE_SID=ORCL
ORACLE_HOSTNAME=oracle
ORACLE_BASE=/db/app/oracle
ORACLE_HOME=/db/app/oracle/product/11.2.0/db_1
```
