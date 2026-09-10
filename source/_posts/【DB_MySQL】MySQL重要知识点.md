---
title: '【DB_MySQL】MySQL重要知识点'
date: 2019-04-09 13:35:00
categories:
  - DB_MySQL
------

1. # MySQL中的select语句

   1. ## 各子句的执行顺序

   SELECT语句的处理过程

   1. FROM 组装数据来源

   2. WHERE筛选元组

   3. GROUP BY 将满足条件的元组进行分组

   4. HAVING 筛选分组

   5. SELECT 投影出指定的结果列

   6. ORDER BY 对结果排序

   7. LIMIT取查询结果的子集

   【总结】每个子句执行后都会产生一个中间结果，供接下来的子句使用，如果不存在某个子句，就跳过。一旦进行了分组，在select子句投影过程中均会将每组合并成一条记录，因此select子句投影的字段要么在聚集函数中，要么为GROUP BY后面的分组依据。

   1. ## limit关键字的使用

   语法：select \* from student limit beginIndex,length;

   这里结果集的下标同数组一样从0开始，beginIndex表示起始位置，length表示从beginIndex开始获取之后的length条记录。若无起始位置参数beginIndex，则默认从0位置开始获取length条记录。

   实例：

   //如下两条语句的结果是一样的

   select \* from student limit 0,5;

   select \* from student limit 5;

   //返回3到10行记录

   select \* from student limit 2,8;     

   +----+------+------+

   | id | name | age |

   +----+------+------+

   | 3 | chou | 13 |

   | 4 | he | 14 |

   | 5 | lin | 15 |

   | 6 | ll | 16 |

   | 7 | chen | 17 |

   | 8 | yu | 18 |

   | 9 | wu | 19 |

   | 10 | xie | 20 |

   【总结】若是从首条记录开始返回（即起始下标为0）则可以省略起始下标，否则都需要起始下标参数beginIndex
2. # MySQL中以字符串的形式存储数组

   1. ## 求数组中元素的个数

   方法：按指定符号分割字符串，返回分割后的元素个数，方法很简单，就是看字符串中存在多少个分隔符号，然后再加一，就是要求的结果。

   CREATE function Get\_StrArrayLength

   (

   @str varchar(1024), --要分割的字符串

   @split varchar(10) --分隔符号

   )

   returns int

   as

   begin

   declare @location int

   declare @start int

   declare @length int

   set @str=ltrim(rtrim(@str))

   set @location=charindex(@split,@str)

   set @length=1

   while @location<>0

   begin

   set @start=@location+1

   set @location=charindex(@split,@str,@start)

   set @length=@length+1

   end

   return @length

   end

   调用示例：select dbo.Get\_StrArrayLength('78,1,2,3',',')

   返回值：4

   1. ## 得到数组中指定位置的元素

   方法：按指定符号分割字符串，返回分割后指定索引的第几个元素，象数组一样方便

   CREATE function Get\_StrArrayStrOfIndex

   (

   @str varchar(1024), --要分割的字符串

   @split varchar(10), --分隔符号

   @index int --取第几个元素

   )

   returns varchar(1024)

   as

   begin

   declare @location int

   declare @start int

   declare @next int

   declare @seed int

   set @str=ltrim(rtrim(@str))

   set @start=1

   set @next=1

   set @seed=len(@split)

   set @location=charindex(@split,@str)

   while @location<>0 and @index>@next

   begin

   set @start=@location+@seed

   set @location=charindex(@split,@str,@start)

   set @next=@next+1

   end

   if @location =0 select @location =len(@str)+1

   --这儿存在两种情况：1、字符串不存在分隔符号 2、字符串中存在分隔符号，跳出while循环后，@location为0，那默认为字符串后边有一个分隔符号。

   return substring(@str,@start,@location-@start)

   end

   调用示例：select dbo.Get\_StrArrayStrOfIndex('8,9,4',',',2)

   返回值：9

   1. ## 结合以上两个函数，遍历数组中的元素

   方法：结合上边两个函数，象数组一样遍历字符串中的元素

   declare @str varchar(50)

   set @str='1,2,3,4,5'

   declare @next int

   set @next=1

   while @next<=dbo.Get\_StrArrayLength(@str,',')

   begin

   print dbo.Get\_StrArrayStrOfIndex(@str,',',@next)

   set @next=@next+1

   end

   调用结果：

   1

   2

   3

   4

   5

   1. # MySQL中的日期函数
   1. ## 转DATETIME类型为Date类型

   将add\_time=2013-01-12 23:23:56转化为date类型    

   select \* from product where Date(add\_time) = '2013-01-12'

   1. ## 获取时间中的年、月、日

   select \* from product where Year(add\_time) = 2013 and Month(add\_time) = 1

   1. ## 获取日期是本年的第几天, 值在1到366之间

   mysql> select DAYOFYEAR('1998-02-03');   
   -> 34

   1. ## 获取日期是本月的第几天, 值在1到31之间

   mysql> select DAYOFMONTH('1998-02-03');   
   -> 3

   1. ## 获取日期是本周的第几天, 值在1到7之间

   返回日期date的星期索引(1=星期天，2=星期一, ……7=星期六)。这些索引值对应于ODBC标准。

   mysql> select DAYOFWEEK('1998-02-03');   
   -> 3

   1. ## 获取日期的星期索引

   返回date的星期索引(0=星期一，1=星期二, ……6= 星期天)

   mysql> select WEEKDAY('1997-10-04 22:23:00');   
   -> 5   
   mysql> select WEEKDAY('1997-11-05');   
   -> 2

   1. ## 获取日期的月份英文名

   mysql> select MONTHNAME("1998-02-05");   
   -> 'February'

   1. ## 获取日期是星期几

   mysql> select DAYNAME("1998-02-05");   
   -> 'Thursday'

   1. ## 获取日期是本年的哪一季度，取值1到4

   mysql> select QUARTER('98-04-01');   
   -> 2

   1. ## 求两日期间的间隔天数

   mysql> SELECT something FROM table   
   WHERE TO\_DAYS(endTime) - TO\_DAYS(beginTime) <= 30;

   1. ## 日期加减函数

   date\_add('2012-05-25', interval 1 day) 表示 2012-05-26

   date\_add('2012-05-25', interval -1 day) 表示 2012-05-24

   date\_sub('2012-05-25',interval 1 day) 表示 2012-05-24  
   date\_sub('2012-05-25',interval -1 day) 表示 2012-05-26  
   //括号里的参数day可以换成month及year

   1. ## 获取当前的日期、时间以及日期时间

   获得当前日期（date）函数：curdate()

   获得当前时间（time）函数：curtime()

   获得当前日期+时间（date + time）函数：sysdate()

    sysdate() 日期时间函数跟 now() 类似，不同之处在于：now() 在执行开始时值就得到了， sysdate() 在函数执行时动态得到值。

   1. ## 将日期时间转换成对应的字符串

   SELECT DATE\_FORMAT(NOW(),'%Y%m%d') 日期字符串,DATE\_FORMAT(NOW(), '%Y%m%d%H%i%s') 日期时间字符串
3. # MySQL中游标的使用

   1. ## 定义游标

   --创建接收游标数据的变量  

       declare c int;  

       declare n varchar(20);  

   --创建游标

   declare cur cursor for select name,count from store where name = 'iphone';

   --指定游标结束循环时的标志位

   declare continue HANDLER for not found set done = true;

   在MySql中，游标溢出时会引发mysql预定义的NOT FOUND错误，当游标已经指向最后一行时继续fetch会造成游标溢出，近而引发not found错误事件，指定这个事件发生时修改done变量的值为true，从而结束循环。

   1. ## 打开游标

   OPEN cur

   1. ## 循环遍历游标

   fetch cur into n,c; #先走一步 

       while(not done) do  

           set total = total + c;  

           fetch cur into n,c;  #继续向下走

   end while; #当遍历到的结果为null时，遍历完成结束循环

   【注①用当型循环（while）遍历游标时，要先将游标向下走一步。

   ②fetch是获取游标当前指向的数据行，并将指针指向下一行，当游标已经指向最后一行时继续fetch会造成游标溢出。

   1. ## 关闭游标

   close cur;  

   while循环遍历游标的完整代码

   1. **drop** **procedure** if exists StatisticStore1;
   2. **CREATE** **PROCEDURE** StatisticStore1()
   3. **BEGIN**
   4. **declare** c **int**;
   5. **declare** n **varchar**(20);
   6. **declare** total **int** **default** 0;
   7. **declare** done **int** **default** **false**;
   8. **declare** cur **cursor** **for** **select** **name**,count **from** store **where** **name** = 'iphone';
   9. **declare** **continue** HANDLER **for** not found **set** done = **true**;
   10. **set** total = 0;
   11. **open** cur;
   12. **fetch** cur **into** n,c;
   13. while(not done) do
   14. **set** total = total + c;
   15. **fetch** cur **into** n,c;
   16. **end** while;
   17. **close** cur;
   18. **select** total;
   19. **END**;
   20. call StatisticStore1();

   注意：①所有变量的定义必须要放在游标和句柄定义之前，否则将会报如下错误：

   ②所有的游标必须要在句柄之前定义
4. # MySQL中常用的系统函数

   1. ## 数据类型转化函数

   CAST(xxx AS 类型) , CONVERT(xxx,类型)

   可用的类型

   二进制,同带binary前缀的效果 : BINARY

   字符型,可带参数 : CHAR()

   日期 : DATE

   时间: TIME

   日期时间型 : DATETIME

   浮点数 : DECIMAL

   整数 : SIGNED

   无符号整数 : UNSIGNED

   注：varchar类型的变量可以自动转换为数字型，转换过程中把首字符为'0'的字符串或其它非数字字符均转换为0

   1. ## 获取新增记录自增长ID的方法

   mysql和oracle插入的时候有一个很大的区别，oracle支持序列做id，mysql通常将自增长的整型列作为id，mysql在插入数据时一般无需插入id字段，那么插入数据后如何获得这个自增的id呢？

   方法一：使用LAST\_INSERT\_ID()函数

   LAST\_INSERT\_ID()函数用于返回当前Connection所更新记录的自增长字段值，这里的更新包括Insert和Update。因为LAST\_INSERT\_ID是基于Connection的，保证了能够找回各客户端自己的ID而不用担心其它客户端的活动，而且不需要加锁。

   SELECT LAST\_INSERT\_ID();

   【注意】1.若同一条insert语句中插入多行记录（如insert into tbl\_name (col\_a, col\_b) values ('aa', 'bb'), ('aaa', 'bbb')，那么该函数返回的是第一条被插入记录的id值，不是最后一条插入记录的id值。因此该方法最适合一条insert语句只插入一条数据的情况。例如：

   INSERT INTO t VALUES  (NULL, 'Mary'), (NULL, 'Jane'), (NULL, 'Lisa');

   mysql> SELECT LAST\_INSERT\_ID();  
       ->2**;//返回的是所插入多条记录中的最后一条记录的自增长的字段值**

   2.
   LAST\_INSERT\_ID 与table无关的，如果向表a插入数据后，再向表b插入数据，LAST\_INSERT\_ID得到的是插入到表b的id值。

   方法二：使用max(id)

   使用LAST\_INSERT\_ID是基于连接的，如果换一个窗口的时候调用则会一直返回10，如果不是频繁的插入我们也可以使用这种方法来获取返回的id值。

   select max(id) from user;

   【注】该法不是基于连接的，故不适合高并发时获取插入记录的id值。如果同时插入的时候返回的值可能不准确。

   方法三:使用系统全局变量@@identity

   @@identity表示最近一次向具有identity属性(即自增列)的表中插入数据时对应的自增列的值，是系统定义的全局变量。如有表A，它的自增列是id，当向A表插入一条数据时自增列id的值为101，则通过select @@identity得到的值就是101。

   select @@IDENTITY

   【注】1. Insert语句后使用全局变量@@identity才有效，并且执行select @@identity的时候连接没有关闭，否则得到的将是NULL值。

   1. ## 多个字符串的拼接

   语法：CONCAT(str1,str2,…)                         
   返回结果为连接参数产生的字符串。如有任何一个参数为NULL，则返回值为 NULL。可以有一个或多个参数。

   SELECT CONCAT('My', NULL, 'SQL');

   由于参与拼接的参数含null，所以返回结果为null

   1. ## 补齐函数

      1. ### 从左边补齐LPAD(str,length, padchar)

   若字符串str的长度不够length，则将其左边用字符padchar补齐至长为length；若str的长度本身就大于length，则将其从右边起截取至length长度。

   select LPAD('123', 8, '0');

   select LPAD('123', 2, '0');//超过固定的长度后从右截取

   1. ### 从右边补齐RPAD (str,length, padchar)

   若字符串str的长度不够length，则将其右边用字符padchar补齐至长为length；若str的长度本身就大于length，则将其从右边起截取至length长度。

   select RPAD('123', 8, '0');

   select RPAD('123', 2, '0');//超过固定的长度后从右截取

   【注】当超过固定的长度后左右补齐都是将右边截取
5. # MySQL中变量大体可以分为四种类型

   1. ## 局部变量

   局部变量一般用在sql语句块中，比如存储过程的begin/end。其作用域仅限于该语句块，在该语句块执行完毕后，局部变量就消失了。

   局部变量一般用declare来声明，可以使用default来说明默认值。

   例如在存储过程中定义局部变量：

   **drop procedure if exists add;**

   **create procedure add**

   **(**

   **in a int,**

   **in b int**

   **)**

   **begin**

   **declare c int default 0;**

   **set c = a + b;**

   **select c as c;**

   **end;**

   在上述存储过程中定义的变量c就是局部变量

   1. ## 用户变量

   用户变量的作用域要比局部变量要广。用户变量可以作用于当前整个连接，但是当当前连接断开后，其所定义的用户变量都会消失。

   用户变量使用如下(这里我们无须使用declare关键字进行定义，可以直接这样使用):

   select @变量名

   对用户变量赋值有两种方式，一种是直接用"="号，另一种是用":="号。其区别在于使用set命令对用户变量进行赋值时，两种方式都可以使用；当使用select语句对用户变量进行赋值时，只能使用":="方式，因为在select语句中，"="号被看作是比较操作符。

   示例程序如下：

   **drop procedure if exists math;**

   **create procedure math**

   **(**

   **in a int,**

   **in b int**

   **)**

   **begin**

   **set @var1 = 1;**

   **set @var2 = 2;**

   **select @sum:=(a + b) as sum, @dif:=(a - b) as dif;**

   **end;**

   **调用过程**

   **mysql> call math(3, 4);  
   +------+------+  
   | sum  | dif  |  
   +------+------+  
   |    7 |   -1 |   
   +------+------+  
   查看用户变量@var1  
   mysql> select @var1; //var1为用户变量  
   +-------+  
   | @var1 |  
   +-------+  
   | 1     |   
   +-------+**

   **查看用户变量@var1  
   mysql> select @var2; //var2为用户变量  
   +-------+  
   | @var2 |  
   +-------+  
   | 2     |   
   +-------+**

   1. ## 会话变量

   服务器为每个连接的客户端维护一系列会话变量。在客户端连接时，使用相应全局变量的当前值对客户端的会话变量进行初始化。设置会话变量不需要特殊权限，但客户端只能更改自己的会话变量，而不能更改其它客户端的会话变量。会话变量的作用域与用户变量一样，仅限于当前连接。当当前连接断开后，其设置的所有会话变量均失效。

   设置会话变量有如下三种方式：

   set session var\_name = value;

   set @@session.var\_name = value;

   set var\_name = value;

   查看一个会话变量也有如下三种方式：

   select @@var\_name;

   select @@session.var\_name;

   show session variables like "%var%";

   1. ## 全局变量

   全局变量影响服务器整体操作。当服务器启动时，它将所有全局变量初始化为默认值。这些默认值可以在选项文件中或在命令行中指定的选项进行更改。要想更改全局变量，必须具有SUPER权限。全局变量作用于server的整个生命周期，但是不能跨重启。即重启后所有设置的全局变量均失效。要想让全局变量重启后继续生效，需要更改相应的配置文件。

   要设置一个全局变量，有如下两种方式：

   set global var\_name = value; //注意：此处的global不能省略。根据手册，set命令设置变量时若不指定GLOBAL、SESSION或者LOCAL，默认使用SESSION

   set @@global.var\_name = value; //同上

   要想查看一个全局变量，有如下两种方式：

   select @@global.var\_name;

   show global variables like "%var%";

   【注】仅当对非局部变量赋值时才可以用":="，局部变量的赋值是不能用"：="
6. # MySQL设计定时器

   1. ## 创建定时器

   创建evevt（定时器）要调用的存储过程test\_proce

   **drop procedure if exists test\_proce  
   create procedure test\_proce()  
   begin  
   insert into test(time) values(now());  
   end**

   创建事件test\_event（其作用：每隔一秒自动调用test\_proce()存储过程）

   **drop event if exists test\_event;  
   create event test\_event  
   on schedule every 1 second  
   on completion preserve disable  
   do call test\_proce();**

   1. ## 开启定时器

   MySQL中evevt功能默认是关闭的,可以使用下面的语句来看evevt的状态，如果是OFF或者0，表示是关闭的。

   show variables like 'event\_scheduler';

   开启evevt功能

   SET GLOBAL event\_scheduler = 1;

   1. ## 设置MySQL服务启动时所有定时器自启动

   在文件夹选项下的查看菜单勾选显示隐藏的文件，使系统盘下的ProgramData文件夹显示出来

   在ProgramData目录下找到MySQL Server的my.ini文件，将其打开后在 [mysqld]节点的如下位置添加：event\_scheduler=ON
7. # MySQL中varchar类型的长度含义

   首先我们知道varchar类型是长度可变，但有上限的字符串类型。例如varchar(n)则表示最大长度为n个字符的字符串，注意这里n表示最大的字符数而非字节数。

   实例论证：

   **CREATE TABLE `test\_varchar\_utf8` (**

   **`id` int(11) NOT NULL AUTO\_INCREMENT,**

   **`name` varchar(12) NOT NULL,**

   **PRIMARY KEY (`id`)**

   **) ENGINE=InnoDB AUTO\_INCREMENT=9 DEFAULT CHARSET=utf8**

   1. ## 插入非中文字符

   mysql> insert into test\_varchar\_utf8(name)values('123456789012');

   Query OK, 1 row affected (0.00 sec)

   mysql> insert into test\_varchar\_utf8(name)values('1234567890123');

   ERROR 1406 (22001): Data too long for column 'name' at row 1

   #发现，对于非中文字符串，可以插入包含12个字符以及小于12个字符的字符串，当大于12个字符时报错。

   #因为对于非中文的字符一个字符占用一个字节，所以，至此还不能说明varchar(n)中的n究竟代表字节数还是字符数

   1. ## 插入包含中文字符的字符串

   mysql> insert into test\_varchar\_utf8(name)values('你好么亲爱的你好么亲爱的');

   Query OK, 1 row affected (0.00 sec)

   mysql> insert into test\_varchar\_utf8(name)values('你好么亲爱的你好么亲爱的你');

   ERROR 1406 (22001): Data too long for column 'name' at row 1

   #发现，对于中文字符串，也可以插入包含12个字符以及小于12个字符的字符串，当大于12个字符时报错。

   #因为对于中文的字符一个字符占用三个字节，所以，说明varchar(n)中的n代表字符数而非字节数

   【总结】MySQL中varchar(n)类型的长度n表示最多可接受的字符长度，而不是字节数。非中文字符时每个字符占用一个字节，当为中文字符时每个字符占用3个字节。
8. # MySQL存储过程中执行动态SQL语句

   --存储过程名和参数，参数中in表示传入参数，out标示传出参数，inout表示传入传出参数

   create procedure p\_procedurecode(in sumdate varchar(10))

   begin

   declare v\_sql varchar(500); --需要执行的SQL语句

   declare sym varchar(6);

   declare var1 varchar(20);

   declare var2 varchar(70);

   declare var3 integer;

   --定义游标遍历时，作为判断是否遍历完全部记录的标记

   declare no\_more\_departments integer DEFAULT 0;

   --定义游标名字为C\_RESULT

   DECLARE C\_RESULT CURSOR FOR

   SELECT barcode,barname,barnum FROM tmp\_table;

   --声明当游标遍历完全部记录后将标志变量置成某个值

   DECLARE CONTINUE HANDLER FOR NOT FOUND

   SET no\_more\_departments=1;

   set sym=substring(sumdate,1,6); --截取字符串，并将其赋值给一个遍历

   --连接字符串构成完整SQL语句，动态SQL执行后的结果记录集，在MySQL中无法获取，因此需要转变思路将其放置到一个临时表中（注意代码中的写法）。一般写法如下：

   -- Create TEMPORARY Table 表名（Select的查询语句）;

   set v\_sql= concat('Create TEMPORARY Table tmp\_table(select aa as aacode,bb as aaname,count(cc) as ccnum from h',sym,' where substring(dd,1,8)=''',sumdate,''' group by aa,bb)');

   set @v\_sql=v\_sql; --注意很重要，将连成成的字符串赋值给一个变量（可以之前没有定义，但要以@开头）

   prepare stmt from @v\_sql; --预处理需要执行的动态SQL，其中stmt是一个变量

   EXECUTE stmt; --执行SQL语句

   deallocate prepare stmt; --释放掉预处理段

   OPEN C\_RESULT; --打开之前定义的游标

   REPEAT --直到型循环语句的关键词

   FETCH C\_RESULT INTO VAR1, VAR2, VAR3; --取出每条记录并赋值给相关变量，注意顺序

   --执行查询语句，并将获得的值付给一个变量 @oldaacode（注意如果以@开头的变量可以不用通过declare语句事先声明）

   select @oldaacode:=vcaaCode from T\_sum where vcaaCode=var1 and dtDate=sumdate;

   if @oldaacode=var1 then --判断

   update T\_sum set iNum=var3 where vcaaCode=var1 and dtDate=sumdate;

   else

   insert into T\_sum(vcaaCode,vcaaName,iNum,dtDate) values(var1,var2,var3,sumdate);

   end if;

   UNTIL no\_more\_departments END REPEAT; --循环语句结束

   CLOSE C\_RESULT; --关闭游标

   DROP TEMPORARY TABLE tmp\_table; --删除临时表

   end;
9. # MySQL中事物详解

   1. ## 事物的定义及特性

   事务是一组操作数据库的SQL语句组成的工作单元，该工作单元中所有操作要么同时成功，要么同时失败。事物有如下四个特性，ACID简称"酸性"。

   1）原子性：工作单元中所有的操作要么都成功，要么都不成功，不会出现部分成功的情况。

   2）一致性：工作完成其结果应与预期一致，比如由A账户向B账户转账的事物，若该事物执行成功则必须保证A账户转出多少钱,B账户相应转入多少钱；若该事物失败，则此次转账即失败。事物的其它三个性质都是为了保证该一致性的。

   3）隔离性：隔离性还可以称为并发控制、可串行化、锁等。事物中所操作的数据要隔离起来，以防止其他用户访问这些数据而带来的不一致情况。

   4）持久性：事务一旦提交，其所做的修改就会永久保存到数据库中，即使数据库发生故障也不应该对其有任何影响。

   事务的持久性不能做到100%的持久，只能从事务本身的角度来保证永久性，而一些外部原因导致数据库发生故障，如硬盘损坏，那么所有提交的数据可能都会丢失。

   1. ## MySQL中开启事务的方法
   1. ### 方法一

   用BEGIN或START TRANSACTION来开启一个事物，COMMIT或ROLLBACK来结束该事物。

   -- 保存点 savepoint  

   begin  

     declare is\_error int default false;#是否出错的标志  

     declare continue handler for sqlexception  

     set is\_error=true;#声明异常处理程序，如果sql异常，则把标志为设置为true  

     start TRANSACTION;#开启事务，则会同时失败，同时成功  

     savepoint s1;#创建保存点  

     insert into employee(id,name,salary) values(146,'cq',9000);  

     savepoint s2;  

     insert into employee(id,name,salary) values(101,'cq',9000);  

     insert into employee(id,name,salary) values(102,'cq',9000);  

     if is\_error THEN  

       rollback to savepoint s1;-- 还原到s1  

       insert into employee(id,name,salary) values(151,'cq',9000);  

       insert into employee(id,name,salary) values(152,'cq',9000);  

       commit;  

     end if;  

   end;  

   1. ### 方法二

   关闭自动提交，设置**SET** AUTOCOMMIT = 0，该语句后的所有操作都将变成事物操作，而且关闭自动提交的情况下，每个事物结束其后续操作都将开启新的事物。

   set autocommit=0;#关闭自动提交  

   #因为关闭了自动提交事务，则添加数据不会保存到数据库中  

   insert into employee(id,name,salary) values(143,'cq',9000);  

   commit;#手动提交所有未执行的数据 

   #由于是关闭自动提交的方式开启的事务，所以每个事物结束其后的操作自动开启新的事物

   insert into employee(id,name,salary) values(258,'wd',8000); #该操作属于新启的事物

   1. ## 事物的隐式提交

   由于事物不能被嵌套，所以当新事物开启时其前的旧事物会被隐式提交。如下情况会导致事物被隐式提交：

   1)新事物的开启会导致旧事物的隐式提交

   START TRANSACTION;

   INSERT INTO `dm\_性别`(性别名称) VALUES('不限');#该操作会被隐式提交

   START TRANSACTION;

   INSERT INTO `dm\_性别`(性别名称) VALUES('男女');

   ROLLBACK;

   2)InnoDB中所有的DDL或DCL操作都会开启一个新的事物，所以DDL或DCL语句会导致旧事物的隐式提交

   SET AUTOCOMMIT = 0;#利用法二关闭自动提交来开启事务 

   BEGIN;  

   INSERT INTO t1 VALUES (1); 

   #该DDL语句会导致其前面的插入操作隐式提交 ，并开启一单一的事物

   CREATE TABLE t2 (pk int primary key);

   INSERT INTO t2 VALUES (2); #自动开启新的事物 

   ROLLBACK; #插入表t1的数据已提交，仅能回滚插入表t2的操作

   3)过程的执行区结束End之前会有一次隐式提交

   BEGIN

   START TRANSACTION;

   INSERT INTO `dm\_性别`(性别名称) VALUES('不限');

   INSERT INTO `dm\_性别`(性别名称) VALUES('男女');

   END #在此之前会导致事物的隐式提交

   1. ## 有关事物操作的注意事项

      1. 存储过程的执行区Begin会开启一个事物，执行区结束End会隐式提交一次

   BEGIN

   INSERT INTO `dm\_性别`(性别名称) VALUES('不限');

   INSERT INTO `dm\_性别`(性别名称) VALUES('男女');

   ## COMMIT隐式提交该执行区域的操作

   END

   1. 不要在事物的中途进行提交操作，一方面会破坏事物的原子性
      ，另一方面该事物会到此结束

   create table testproc(id int(4) primary key, name varchar(100));

   #测试过程

   CREATE PROCEDURE test\_proc\_ins(

   IN i\_id INT,

   IN i\_name VARCHAR(100)

   )

   BEGIN

   start transaction; #本意是将两次插入操作捆绑成一个事物

   INSERT INTO testproc VALUES (i\_id, i\_name);

   COMMIT;#由于中途提交导致该事物提交前结束，其后的操作不再是事物操作

   INSERT INTO testproc VALUES (i\_id, i\_name); #这里故意违反主键约束

   ROLLBACK;#由于第一条插入数据的操作已提交，故这里的ROLLBACK无效

   END;

   1. 由于DDL或DCL操作会创建新的事物，这导致其前的操作会隐式提交，从而破坏事物的原子性，所以尽量不要在过程中使用DDL或DCL语句。而且在过程中使用DDL或DCL语句的语法是比较复杂的，所以不建议在过程中使用DDL或DCL语句。

   SET AUTOCOMMIT = 0;  

   BEGIN;  

   INSERT INTO t1 VALUES (1);  

   #该DDL语句开启新事物会隐式提交其前的事物 

   CREATE TABLE t2 (pk int primary key); INSERT INTO t2 VALUES (2);  

   ROLLBACK;  

   SHOW TABLES

   1. Start Transaction(Begin)与闭自动提交开启事务的区别：Start Transaction只开启了一个当前事物，该事物结束其后的操作将不再是事物操作；但关闭自动提交的方式开启事务，每个事物结束其后的操作自动默认为新的事物操作。
   2. MySQL的事务支持不是绑定在MySQL服务器本身，而是与存储引擎相关：

   1.MyISAM：不支持事务，用于只读程序提高性能

   2.InnoDB：支持ACID事务、行级锁、并发

   3.Berkeley DB：支持事务
10. # Navicat中常用快捷键

    **1.ctrl+q           打开查询窗口  
    2. ctrl+r           运行查询窗口的sql语句**

    **3.ctrl+shift+r   只运行选中的sql语句**

    **4.ctrl+/            注释sql语句  
    5.ctrl+shift +/  解除注释   
    6. ctrl+d     在查询表数据界面打开一个该表的设计视图  
    7. ctrl+l            删除一行  
    8. F6               打开一个mysql命令行窗口  
    9.ctrl+n           打开一个新的查询窗口  
    10.ctrl+w          关闭一个查询窗口**
11. # MySQL中的用户及权限管理

    1. ## 用户管理

    MySQL中同一个数据库服务器下的所有用户信息都存在mysql库中的user表中，可以通过查询该表来查看各用户的信息。其中超级管理员具有最高权限可以查看所有的数据库。MySQL中是以用户名：User及可访问的主机：Host来唯一确定一个用户的。

    --1.创建用户

    create user 'username'@'host' identified by 'password';

    --2.删除用户

    drop user 'username'@'localhost';

    --3.修改用户的登录密码

    set password for 'username'@'host'=PASSWORD('123456');

    --4.查看mysql.user表中的用户信息

    SELECT \* FROM mysql.user WHERE user='sys';

    1. ## 用户权限管理

    MySQL中权限分的比较细，大致可分为表权限、列权限、过程权限。其中过程的权限最需注意，定义存储过程需要CREATE ROUTINE权限、编辑或删除过程需要ALTER ROUTINE权限，调用存储过程需要EXECUTE权限。当过程的SQL SECURITY属性为DEFINER时，过程的执行均是由过程的定义者来调用的。

    --1.给用户赋予权限

    GRANT ALL PRIVILEGES ON hb\_subjectinfo.\* TO 'subject'@'%';

    --2.收回用户的权限

    REVOKE ALL PRIVILEGES ON hb\_subjectinfo.\* FROM 'subject'@'%';

    flush privileges;--权限变更后一般要执行权限刷新操作

    --3.查看用户权限

    SHOW GRANTS FOR 'subject'@'%'

    MySQL中当过程的调用者与定义者不一致时需注意，过程的执行者与SQL SECURITY属性有关，当过程的SQL SECURITY属性为DEFINER时，拥有EXECUTE权限的用户是通过定义者来执行存储过程的；当过程的SQL SECURITY属性为INVOKER时，拥有EXECUTE权限的用户是通过调用者自身来执行存储过程的，即过程的调用者即为执行者。

    综上，当过程的调用者与定义者不一致时可通过如下几种操作完成过程的调用

    1.修改存储过程的definer使其为调用者

    update mysql.proc set definer='调用者' where db='servant\_591up';

    UPDATE `mysql`.`proc` SET `definer`='root00@%' WHERE `db`='test' AND `name`='jjjj' AND `type`='PROCEDURE';

    UPDATE `mysql`.`proc` SET `definer`='wtc\_678869@%' WHERE `db`='servant\_591up' AND `type`='PROCEDURE';

    2.修改过程的sql security属性为INVOKER

    ALTER PROCEDURE www SQL SECURITY INVOKER ;
