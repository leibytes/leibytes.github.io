---
title: '【DB_MySQL】MySQL中事物的详解'
date: 2018-07-12 02:34:00
categories:
  - DB_MySQL
------

# 1. 事物的定义及特性

事务是一组操作数据库的SQL语句组成的工作单元，该工作单元中所有操作要么同时成功，要么同时失败。事物有如下四个特性，ACID简称“酸性”。

1）原子性：工作单元中所有的操作要么都成功，要么都不成功，不会出现部分成功的情况。

2）一致性：工作完成其结果应与预期一致，比如由A账户向B账户转账的事物，若该事物执行成功则必须保证A账户转出多少钱,B账户相应转入多少钱；若该事物失败，则此次转账即失败。事物的其它三个性质都是为了保证该一致性的。

3）隔离性：隔离性还可以称为并发控制、可串行化、锁等。事物中所操作的数据要隔离起来，以防止其他用户访问这些数据而带来的不一致情况。

4）持久性：事务一旦提交，其所做的修改就会永久保存到数据库中，即使数据库发生故障也不应该对其有任何影响。

事务的持久性不能做到100%的持久，只能从事务本身的角度来保证永久性，而一些外部原因导致数据库发生故障，如硬盘损坏，那么所有提交的数据可能都会丢失。

# 2. MySQL中开启事务的方法

![](/images/DB_MySQL/MySQL中事物的详解_001.png)

## 2.1.方法一

用BEGIN或START TRANSACTION来开启一个事物，COMMIT或ROLLBACK来结束该事物。

![](/images/DB_MySQL/MySQL中事物的详解_002.gif)![](/images/DB_MySQL/MySQL中事物的详解_003.gif)

```
-- 保存点 savepoint  
begin  
  declare is_error int default false;#是否出错的标志  
  declare continue handler for sqlexception  
  set is_error=true;#声明异常处理程序，如果sql异常，则把标志为设置为true  
  start TRANSACTION;#开启事务，则会同时失败，同时成功  
  savepoint s1;#创建保存点  
  insert into employee(id,name,salary) values(146,'cq',9000);  
  savepoint s2;  
  insert into employee(id,name,salary) values(101,'cq',9000);  
  insert into employee(id,name,salary) values(102,'cq',9000);  
  if is_error THEN  
    rollback to savepoint s1;-- 还原到s1  
    insert into employee(id,name,salary) values(151,'cq',9000);  
    insert into employee(id,name,salary) values(152,'cq',9000);  
    commit;  
  end if;  
end;
```

View Code

## 2.2.方法二

关闭自动提交，设置**SET** AUTOCOMMIT = 0，该语句后的所有操作都将变成事物操作，而且关闭自动提交的情况下，每个事物结束其后续操作都将开启新的事物。

```
set autocommit=0;#关闭自动提交  
#因为关闭了自动提交事务，则添加数据不会保存到数据库中  
insert into employee(id,name,salary) values(143,'cq',9000);  
commit;#手动提交所有未执行的数据 
#由于是关闭自动提交的方式开启的事务，所以每个事物结束其后的操作自动开启新的事物
insert into employee(id,name,salary) values(258,'wd',8000); #该操作属于新启的事物
```

# 3. 事物的隐式提交

由于事物不能被嵌套，所以当新事物开启时其前的旧事物会被隐式提交。如下情况会导致事物被隐式提交：

1)新事物的开启会导致旧事物的隐式提交

```
START TRANSACTION;
INSERT INTO `dm_性别`(性别名称) VALUES('不限');#该操作会被隐式提交
START TRANSACTION;
INSERT INTO `dm_性别`(性别名称) VALUES('男女');
ROLLBACK;
```

2)InnoDB中所有的DDL或DCL操作都会开启一个新的事物，所以DDL或DCL语句会导致旧事物的隐式提交

```
SET AUTOCOMMIT = 0;#利用法二关闭自动提交来开启事务 
BEGIN;  
INSERT INTO t1 VALUES (1); 
#该DDL语句会导致其前面的插入操作隐式提交 ，并开启一单一的事物
CREATE TABLE t2 (pk int primary key);
INSERT INTO t2 VALUES (2); #自动开启新的事物 
ROLLBACK; #插入表t1的数据已提交，仅能回滚插入表t2的操作
```

3)过程的执行区结束End之前会有一次隐式提交

```
BEGIN
START TRANSACTION;
INSERT INTO `dm_性别`(性别名称) VALUES('不限');
INSERT INTO `dm_性别`(性别名称) VALUES('男女');
END #在此之前会导致事物的隐式提交
```

# 4. 有关事物操作的注意事项

①  存储过程的执行区Begin会开启一个事物，执行区结束End会隐式提交一次

```
BEGIN
INSERT INTO `dm_性别`(性别名称) VALUES('不限');
INSERT INTO `dm_性别`(性别名称) VALUES('男女');
## COMMIT隐式提交该执行区域的操作
END
```

②  不要在事物的中途进行提交操作，一方面会破坏事物的原子性 ，另一方面该事物会到此结束

```
create table testproc(id int(4) primary key, name varchar(100));
#测试过程
CREATE PROCEDURE test_proc_ins(  
IN i_id INT,  
IN i_name VARCHAR(100)  
)  
BEGIN  
start transaction; #本意是将两次插入操作捆绑成一个事物  
     INSERT INTO testproc VALUES (i_id, i_name);  
COMMIT;#由于中途提交导致该事物提交前结束，其后的操作不再是事物操作
      INSERT INTO testproc VALUES (i_id, i_name); #这里故意违反主键约束 
ROLLBACK;#由于第一条插入数据的操作已提交，故这里的ROLLBACK无效
END;
```

③  由于DDL或DCL操作会创建新的事物，这导致其前的操作会隐式提交，从而破坏事物的原子性，所以尽量不要在过程中使用DDL或DCL语句。而且在过程中使用DDL或DCL语句的语法是比较复杂的，所以不建议在过程中使用DDL或DCL语句。

```
SET AUTOCOMMIT = 0;  
BEGIN;  
INSERT INTO t1 VALUES (1);  
#该DDL语句开启新事物会隐式提交其前的事物 
CREATE TABLE t2 (pk int primary key);  
INSERT INTO t2 VALUES (2);  
ROLLBACK;  
SHOW TABLES
```

④  Start Transaction(Begin)与闭自动提交开启事务的区别：

Start Transaction只开启了一个当前事物，该事物结束其后的操作将不再是事物操作；但关闭自动提交的方式开启事务，每个事物结束其后的操作自动默认为新的事物操作。

⑤  MySQL的事务支持不是绑定在MySQL服务器本身，而是与存储引擎相关：

1.MyISAM：不支持事务，用于只读程序提高性能

2.InnoDB：支持ACID事务、行级锁、并发

3.Berkeley DB：支持事务
