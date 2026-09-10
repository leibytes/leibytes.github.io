---
title: '【DB_Oracle】Oracle中tnsnames.ora文件的作用'
date: 2021-02-22 05:03:00
categories:
  - DB_Oracle
------

tnsnames.ora 文件用来定义一个或多个数据库实例的网络描述，客户端要想连接Oracle数据库实例必须要知道实例的网络信息，我们可以在tnsnames.ora 文件中指明实例的网络信息并给个别名，本地可通过该别名来连接，异地登录需再通过ip:port/服务名的方式来连接，例如：  

![image](/images/DB_Oracle/Oracle中tnsnames.ora文件的作用_001.png)

(1) 别名登录

别名仅用于在数据库本机登录，如果异地要用别名登录，就要装Oracle客户端（如解压即可用的instantclient），并在客户端的tnsnames.ora文件配置相同的别名才可用别名登录，登录时需注意区分大小写。

```
sqlplus 用户名/密码@ehyk_ser
```

(2) 服务名登录

本地和异地都可以用ip:端口/服务名的通用模式登录。

```
sqlplus 用户名/密码@172.18.0.252:1521/orcl
```

(3) 修改服务名

SERVICE\_NAME可以改，但并不是改下tnsnames.ora里的SERVICE\_NAME就可以了，仅修改tnsnames.ora文件登录会报 ORA-12514 连不上，需要修改Oracle参数来调整服务名。

1.登录数据库

```
sqlplus / as sysdba
```

2.查看当前服务名

```
show parameter service_names;
```

3.修改服务名

```
alter system set service_names = 新名称 scope=both;
```

4.重启监听（服务端）  
5.最后再去tnsnames.ora文件将SERVICE\_NAME同步改成新名称
