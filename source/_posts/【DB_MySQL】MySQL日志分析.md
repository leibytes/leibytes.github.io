---
title: '【DB_MySQL】MySQL日志分析'
date: 2019-08-20 04:03:00
categories:
  - DB_MySQL
------

MySQL数据库常见的日志有：错误日志（log\_error）、慢查询日志（slow\_query\_log）、二进制日志（bin\_log）、通用日志（general\_log）

## 开启慢查询日志并分析

开启慢查询日志：

set global slow\_query\_log=1; 在线开启。如果MySQL发生重启，就会失效，如果要永久生效，就必选修改配置文件。

slow-query-log-file：新版（5.6及以上）MySQL数据库慢查询日志存储路径。可以不设置该参数，系统则会默认给一个缺省的文件host\_name-slow.log

long\_query\_time ：慢查询时间阈值，当查询时间大于设定的阈值时，查询会被记录到慢查询日志。

set global  log\_queries\_not\_using\_indexes=1;设置未走索引的查询全部进入慢查询日志

log\_output：日志存储方式。log\_output='FILE'表示将日志存入文件，默认值是'FILE'。log\_output='TABLE'表示将日志存入数据库，这样日志信息就会被写入到mysql.slow\_log表中。MySQL数据库支持同时两种日志存储方式，配置的时候以逗号隔开即可，如：log\_output='FILE,TABLE'。日志记录到系统的专用日志表中，要比记录到文件耗费更多的系统资源，因此对于需要启用慢查询日志，又需要能够获得更高的系统性能，那么建议优先记录到文件。

**注意：**开启慢查询日志后向日志写数据是比较耗时，影响性能的。一般需要时才开启慢查询日志，用完即可关掉。

参考博文：

[MySQL慢查询&分析SQL执行效率浅谈](https://www.jianshu.com/p/43091bfa8aa7)

[MySQL慢查询日志分析](https://blog.csdn.net/tianwang1101/article/details/76242114)
