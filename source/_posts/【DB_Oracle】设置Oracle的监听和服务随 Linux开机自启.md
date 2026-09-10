---
title: '【DB_Oracle】设置Oracle的监听和服务随 Linux开机自启'
date: 2020-08-31 05:11:00
categories:
  - DB_Oracle
------

第一步:修改/etc/oratab文件

```
[oracle@localhost ~]$ vim /etc/oratab    //如果没有这个文件，则是因为oracle安装完成的时候忘记执行root.sh文件，找到root.sh执行后就会出现oratab这个文件
```

第二步:把lsnrctl start和dbstart添加到rc.local文件中:

 root权限执行

```
[root@localhost oracle]# vim /etc/rc.d/rc.local
```

添加：

```
su - oracle -lc "/data/oracle/product/11.2.0/db_1/bin/lsnrctl start"
su - oracle -lc "/data/oracle/product/11.2.0/db_1/bin/dbstart"
```

 说明：第一行为开机启动数据库监听，第二行为开机启动数据库服务。(路径跟安装路径相关)。

注意：CentOs7中/etc/rc.d/rc.local不会开机执行，因此需添加执行权限。

查看/etc/rc.d/rc.local的权限

```
[root@localhost oracle]# ll /etc/rc.d/rc.local
```

 添加执行权限

```
[root@localhost oracle]# chmod +x /etc/rc.d/rc.local
```

 此致Oracle监听和服务开机自启动设置完成。
