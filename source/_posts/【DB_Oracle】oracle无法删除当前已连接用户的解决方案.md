---
title: '【DB_Oracle】oracle无法删除当前已连接用户的解决方案'
date: 2021-01-04 06:29:00
categories:
  - DB_Oracle
------

在还原Oracle某个用户的数据之前应把该用户及其已有的数据删掉，然后新建同名的用户并指定对应的表空间、临时空间等，并授权。但在删除该用户时提示“无法删除当前已连接的用户”，该问题的解决方案如下：

例如要将名为xdbh的用户及其下的数据删除

1、先将待删除用户锁定

```
alter user XDBH account lock;
```

2、查询该用户已连接的回话

```
select username,sid,serial# from v$session where username='XDBH';    
查询结果：  
USERNAME SID SERIAL#  
XDBH	927	25285  
XDBH	1974	31863  
XDBH	2405	18931
```

3、然后基于语句alter system kill session 'xx,xxx'来关闭对应的回话，xx,xxx分别代表查询出来的sid和serial#

```
alter system kill session '927,25285'
```

4、有可能与该用户建立的回话比较多，每个回话均要写一个关闭语句太麻烦，可以通过SQL拼接的方式得到所有回话的关闭语句，最后将查询结果复制出来批量执行即可。

```
select 'alter system kill session '||chr(39)||sid||','||serial#||chr(39)||';' from v$session where username='XDBH';  
查询结果：
```

alter system kill session '121,63557';  
alter system kill session '443,37363';  
alter system kill session '917,1621';  
alter system kill session '1167,13205';  
alter system kill session '1259,18173';  
alter system kill session '1675,21987';  
alter system kill session '1926,43463';  
alter system kill session '2030,15817';  
alter system kill session '2036,6069';  
alter system kill session '2427,3455';  
alter system kill session '2748,51889';  
alter system kill session '2819,37317';  
alter system kill session '2842,43539';
