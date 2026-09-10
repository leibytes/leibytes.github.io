---
title: '【DB_MySQL】MySQL关联删除'
date: 2026-05-26 07:55:00
categories:
  - DB_MySQL
------

# MySQL关联删除（DELETE JOIN）表数据
## 一、最核心语法（记住这个就够）
### 1\. \*\*只删除子表数据\*\*（最常用、最安全）
```sql
DELETE 子表
FROM 子表
JOIN 主表 ON 子表.关联字段 = 主表.关联字段
WHERE 主表.条件;
```
### 2\. \*\*同时删除主表 \+ 子表数据\*\*
```sql
DELETE 主表, 子表
FROM 主表
JOIN 子表 ON 主表.id = 子表.主表ID
WHERE 条件;
```
---
## 二、最常用示例（直接套）
### 示例 1：删除某个用户的所有订单（只删订单）
```sql
DELETE orders
FROM orders
JOIN user ON orders.user\_id = user.id
WHERE user.id = 100;
```
✅ \*\*只删 orders 表\*\*
✅ \*\*user 表不动\*\*
---
### 示例 2：删除用户 \+ 同时删除他的所有订单
```sql
DELETE user, orders
FROM user
JOIN orders ON user.id = orders.user\_id
WHERE user.id = 100;
```
✅ \*\*主表子表一起删\*\*
✅ \*\*一步到位\*\*
---
### 示例 3：左关联删除（没有匹配也删）
```sql
DELETE user
FROM user
LEFT JOIN orders ON user.id = orders.user\_id
WHERE orders.user\_id IS NULL;
```
✅ 删除\*\*没有订单的用户\*\*
---
## 三、安全提醒（非常重要）
✅删除必须带where条件，\*\*如果不带where条件会将所有关联上的数据全部删除\*\*
✅DELETE LEFT JOIN WHERE NULL：专门用来删除没关联上的数据（不存在的数据）
