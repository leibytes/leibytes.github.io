---
title: '【DB_MySQL】MySQL多表关联更新'
date: 2026-05-26 07:16:00
categories:
  - DB_MySQL
------

# MySQL 多表关联更新（UPDATE JOIN）最实用写法
## 一、标准语法（最常用）
```sql
UPDATE 表1
JOIN 表2 ON 表1.关联字段 = 表2.关联字段
SET 表1.要更新的字段 = 表2.来源字段
WHERE 过滤条件;
```
---
## 二、最常用示例（你直接套）
### 示例 1：根据用户 ID，把 user 表的昵称更新成 order 表的收货人
```sql
UPDATE user u
JOIN orders o ON u.id = o.user\_id
SET u.nickname = o.receiver\_name
WHERE o.order\_status = 1;
```
### 示例 2：多字段同时更新
```sql
UPDATE user u
JOIN orders o ON u.id = o.user\_id
SET u.phone = o.receiver\_phone,
u.address = o.receiver\_address
WHERE o.create\_time > '2025-01-01';
```
### 示例 3：左关联更新（没有匹配也更新为 NULL）
```sql
UPDATE user u
LEFT JOIN orders o ON u.id = o.user\_id
SET u.total\_amount = o.order\_amount;
```
---
## 三、带条件 \+ 只更新符合条件的数据（最安全）
```sql
UPDATE product p
JOIN category c ON p.cat\_id = c.id
SET p.status = 1
WHERE c.name = '电子产品'
AND p.price > 100;
```
---
## 四、关联更新时匹配到多条数据的处理方案
\*\*如果 1 条主表记录 匹配到多条从表记录\*\*
\*\*MySQL 只会随机取其中 1 条记录的值来更新，其他会被忽略！\*\*
\*\*只取第一条关联上的（不能确定具体是哪一条）\*\*
不会报错
不会合并数据
不会更新多次
---
## 五、3 个关键点（必看）
1. \*\*必须写 JOIN \+ ON 关联条件\*\*，否则会全表乱更新
2. \*\*SET 必须指定表名\*\*：`表1.字段 = 表2.字段`
3. \*\*一定要加 WHERE\*\*，否则更新全表
