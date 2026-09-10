---
title: '【DB_MySQL】MySQL中日期函数的使用'
date: 2018-11-27 13:30:00
categories:
  - DB_MySQL
------

# 1. MySQL中日期函数的使用

## 1.1. 转DATETIME类型为Date类型

```
将add_time=2013-01-12 23:23:56转化为date类型    
select * from product where Date(add_time) = '2013-01-12'
```

## 1.2. 获取时间中的年、月、日

```
select * from product where Year(add_time) = 2013 and Month(add_time) = 1
```

## **1.3. 获取日期是本年的第几天, 值在1到366之间**

```
mysql> select DAYOFYEAR('1998-02-03'); 
-> 34
```

## **1.4. 获取日期是本月的第几天, 值在1到31之间**

```
mysql> select DAYOFMONTH('1998-02-03'); 
-> 3
```

## **1.5. 获取日期是本周的第几天, 值在1到7之间**

返回日期date的星期索引(1=星期天，2=星期一, ……7=星期六)，这些索引值对应于ODBC标准。

```
mysql> select DAYOFWEEK('1998-02-03'); 
-> 3
```

## 1.6. 获取日期的星期索引

返回日期的星期索引(0=星期一，1=星期二, ……6= 星期天)

```
mysql> select WEEKDAY('1997-10-04 22:23:00'); 
-> 5 
mysql> select WEEKDAY('1997-11-05'); 
-> 2
```

## **1.7. 获取日期的月份英文名**

```
mysql> select MONTHNAME("1998-02-05"); 
-> 'February'
```

## 1.8. 获取日期是星期几

```
mysql> select DAYNAME("1998-02-05"); 
-> 'Thursday'
```

## 1.9. 获取日期是本年的哪一季度，取值1到4

```
mysql> select QUARTER('98-04-01'); 
-> 2
```

## 1.10. 求两日期间的间隔天数

```
mysql> SELECT something FROM table WHERE TO_DAYS(endTime) - TO_DAYS(beginTime) <= 30;
```

## 1.11. 日期加减函数

```
date_add('2012-05-25', interval 1 day) 表示 2012-05-26
date_add('2012-05-25', interval -1 day) 表示 2012-05-24
date_sub('2012-05-25',interval 1 day) 表示 2012-05-24
date_sub('2012-05-25',interval -1 day) 表示 2012-05-26
//括号里的参数day可以换成month及year
```

## 1.12. 获取当前的日期、时间以及日期时间

```
获得当前日期（date）函数：curdate()
获得当前时间（time）函数：curtime()
获得当前日期+时间（date + time）函数：sysdate()
sysdate() 日期时间函数跟 now() 类似，不同之处在于：now() 在执行开始时值就得到了， sysdate() 在函数执行时动态得到值。
```

## 1.3. 将日期时间转换成对应的字符串

```
SELECT DATE_FORMAT(NOW(),'%Y%m%d') 日期字符串,DATE_FORMAT(NOW(), '%Y%m%d%H%i%s') 日期时间字符串
```

![](/images/DB_MySQL/MySQL中日期函数的使用_001.png)
