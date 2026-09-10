---
title: '【DB_Oracle】sqlplus登录Oracle数据库'
date: 2021-06-08 08:00:00
categories:
  - DB_Oracle
------

使用sqlplus登录Oracle指定的实例

方式一：

sqlplus 用户名/密码@实例名

实例：

```
[oracle@db ~]$ sqlplus dfds_test/dfds_test_2021@orac

SQL*Plus: Release 11.2.0.4.0 Production on Tue Jun 8 15:59:01 2021

Copyright (c) 1982, 2013, Oracle.  All rights reserved.

Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.4.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options

SQL>
```

**注意**：若为管理员用户登录，则需要在实例后面加上as sysdba，如：sqlplus sys/orcl@orcl as sysdba

方式二：

```
[oracle@db ~]$ sqlplus /nolog

SQL*Plus: Release 11.2.0.4.0 Production on Tue Jun 8 15:55:41 2021

Copyright (c) 1982, 2013, Oracle.  All rights reserved.

SQL> connect dfds_test/dfds_test_2021@orac;
Connected.
```

同样，若为管理员用户登录需要在实例后面加上as sysdba

```
SQL> connect sys/dba@orac as sysdba
Connected.
```
