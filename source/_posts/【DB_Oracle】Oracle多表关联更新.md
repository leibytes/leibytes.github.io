---
title: '【DB_Oracle】Oracle多表关联更新'
date: 2020-11-26 11:57:00
categories:
  - DB_Oracle
------

很多场景我们需要依据两个表的某个字段进行关联更新。

  select \* from table1  t1;

![](/images/DB_Oracle/Oracle多表关联更新_002.png)

  select \* from table2  t2;

![](/images/DB_Oracle/Oracle多表关联更新_001.png)

**现需求：**参照table2表修改table1表，修改条件为两表的fname列内容一致。

**常见陷阱：**update table1 t1 set t1.fmoney = (select  t2.fmoney from table2 t2 where  t2.fname = t1.fname)

执行后table1 结果如下：

![](/images/DB_Oracle/Oracle多表关联更新_003.png)

  有一行原有值，被更新成空值了。

**正确写法：**

update table1 t1 set t1.fmoney = (select t2.fmoney from table2 t2 where t2.fname = t1.fname) where exists(select 1 from table2 t2 where t2.fname = t1.fname);

![](/images/DB_Oracle/Oracle多表关联更新_004.png)

**SQL模板：**

```
update table1 t1 set t1.c= (select t2.c from table2 t2 where t1.a=t2.a) WHERE EXISTS(SELECT 1 FROM table2 t2 WHERE t2.a = t1.a);
```

当在t1.a=t2.a的条件下t2查询出多条记录时也会报错，此时可以考虑将t2.c唯一化。常用的如下两种方法

法一:取满足条件的t2.c的最值

update table1 t1 set t1.c = (select max(t2.c) from table2  t2 where t1.a=t2.a) where exists(select 1 from table2 t2 where t2.a = t1.a);

法二:取满足条件第一行的t2.c值

update table1 t1 set t1.c = (select t2.c  from table2  t2 where t1.a=t2.a  and rownum =1) where exists(select 1 from table2 t2 where t2.a = t1.a);

参考博文：[ORACLE 两表关联更新三种方式](https://www.cnblogs.com/xtjatswc/p/12036653.html)
