---
title: '【DB_Oracle】impdp/expdp导入导出dmp文件'
date: 2020-08-31 07:41:00
categories:
  - DB_Oracle
------

dmp文件是Oracle数据库的转储文件，用于在不同主机之间进行数据迁移。通常使用expdp和impdp命令进行dmp文件的导出导入，expdp和impdp是服务端工具，只能在Oracle服务端使用。

## **一、从数据库导出dmp文件**

切换到Oracle安装目录的宿主用户，然后通过expdp进行导出。

导出命令的模板：

```
expdp exp_user/exp_user_password@ip:1521/sid directory= DATA_PUMP_DIR dumpfile=file_name.dmp logfile=exp_logfile.log content=all SCHEMAS=exp_user
```

1、exp\_user/exp\_user\_password表示导出用户的账号/密码

2、ip和端口号只有从远程导出时才需要，若本地导出只需指定实例名sid就好

3、directory指定导出的dmp文件存放的目录，可以自己新建导出目录，但一般使用默认的就好。

通过语句select \* from dba\_directories查询dmp文件存储的目录

![](/images/DB_Oracle/impdp_expdp导入导出dmp文件_001.png)

 4、dumpfile指定导出的dmp文件的文件名

5、logfile导出时的日志文件，可选

6、SCHEMAS导出用户的账号

导出实例：

```
expdp jslt/passwd@orcl directory= DATA_PUMP_DIR dumpfile=jslt_export_20200831.dmp logfile=jslt_export_20200831.log content=all SCHEMAS=jslt
```

导出完成后dmp文件就在 directory指定的目录下

```
[oracle@centos7 dpdump]$ cd /data/oracle/admin/orcl/dpdump/
[oracle@centos7 dpdump]$ ls
jslt_export_20200831.dmp  jslt_export_20200831.log
```

## 二、将dmp文件导入到Oracle数据库

登录待导入dmp文件的服务器后切换到Oracle安装目录的宿主用户：su -  oracle（宿主由根据安装Oracle时指定，我这里时oracle）

1、进入sqlplus命令行

```
[oracle@centos7 ~]$ sqlplus /nolog
```

2、以拥有dba身份的用户登录数据库

```
SQL> conn jslt/passwd@orcl
Connected.
```

3、创建用户、表空间、并给用户赋予权限

```
create tablespace XDBH_DATA  
logging
```

```
#表空间所在的目录可通过语句select file_name,tablespace_name from dba_data_files查询得到
```

```
datafile '/data/oracle/oradata/orcl/XDBH_DATA.DBF'   
size 512m  
autoextend on  
next 50m  
extent management local;  

create temporary tablespace XDBH_TEMP  
tempfile '/data/oracle/oradata/orcl/XDBH_TEMP.DBF'   
size 112m   
autoextend on next 100m   
extent management local;   
  
create user xdbh identified by xdbh   
default tablespace XDBH_DATA   
temporary tablespace XDBH_TEMP;   
  
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;   
  
grant connect,resource,dba to xdbh ;
```

4、通过语句select \* from dba\_directories查询dmp文件存储的目录，将已导出的dmp文件上传到该目录下供导入使用

![](/images/DB_Oracle/impdp_expdp导入导出dmp文件_002.png)

 5、通过impdp命令进行导入

导入模板

```
impdp imp_user/imp_user_password@ip:1521/sid directory=DUMP_DIR dumpfile=export.dmp REMAP_SCHEMA=exp_user:imp_user REMAP_TABLESPACE=exp_tablespacename:imp_tablespacename   logfile=implog.log table_exists_action=replace content=all transform=OID:N
```

 导入实例：

```
impdp jslt/passwd@orcl directory=DATA_PUMP_DIR dumpfile=jslt_export_20200831.dmp logfile=jslt_import_20200831.log REMAP_SCHEMA=jslt:jslt REMAP_TABLESPACE=LTBH_DATA:JSLT_DATA table_exists_action=replace content=all transform=OID:N
```

 参考博客：

1、[关于Linux下使用expdp和impdp命令对Oracle数据库进行导入和导出操作](https://www.cnblogs.com/xialiaoliao0911/p/7523921.html)

2、[Oracle 导出导入数据](https://www.cnblogs.com/activiti/p/6709686.html)

3、[oracle表空间的创建及dmp 文件的导入](https://www.cnblogs.com/activiti/p/7575109.html)
