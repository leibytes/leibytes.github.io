---
title: '【DB_MySQL】select语句的执行顺序'
date: 2018-11-27 09:36:00
categories:
  - DB_MySQL
------

1. FROM 指明查询来源

2. WHERE筛选元组

3. GROUP BY进行分组

4. HAVING 筛选分组

5. SELECT 投影出指定的字段列

6. ORDER BY 对结果集排序

7. LIMIT 取结果的子集

【总结】

⑴若一查询语句包含上述七个子句，那么它会按照上述1~7步依次执行，每步都会产生一个中间结果。

⑵一旦进行了GROUP BY分组，select子句会将每组合并成一条记录，因此select子句后的投影字段要么在聚集函数中，要么作为GROUP BY后的分组字段。
