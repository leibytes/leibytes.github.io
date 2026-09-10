---
title: '【DB_MySQL】 limit——取查询结果的子集'
date: 2019-04-26 05:17:00
categories:
  - DB_MySQL
------

语法：select \* from student limit beginIndex,length;

这里结果集的下标同数组一样从0开始，beginIndex表示起始位置，length表示从beginIndex开始获取之后的length条记录。若无起始位置参数beginIndex，则默认从0位置开始获取length条记录。

实例：

```
//如下两条语句的结果是一样的
select * from student limit 0,5; 
select * from student limit 5;
//返回3到10行记录
select * from student limit 2,8; 
+----+------+------+

| id | name | age  |

+----+------+------+

|  3 | chou |   13 |

|  4 | he   |   14 |

|  5 | lin  |   15 |

|  6 | ll   |   16 |

|  7 | chen |   17 |

|  8 | yu   |   18 |

|  9 | wu   |   19 |

| 10 | xie  |   20 |
```

【注意】若是从首条记录开始取（即起始下标为0）则可以省略起始下标，否则都需要起始下标参数beginIndex
