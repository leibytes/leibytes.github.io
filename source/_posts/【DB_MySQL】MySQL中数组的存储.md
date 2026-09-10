---
title: '【DB_MySQL】MySQL中数组的存储'
date: 2018-11-27 13:05:00
categories:
  - DB_MySQL
------

# 1. MySQL中以字符串的形式存储数组

MySQL中无数组类型，通常将数组元素按某个字符分割以字符串形式存储

## 1.1. 求数组中元素的个数

方法：按指定符号分割字符串，返回分割后的元素个数。方法很简单，就是看字符串中存在多少个分隔符号，然后再加一，就是要求的结果。

```
CREATE function Get_StrArrayLength 
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
```

调用示例：select  Get\_StrArrayLength('78,1,2,3',',')

返回值：4

## 1.2. 得到数组中指定位置的元素

方法：按指定符号分割字符串，返回分割后指定索引的第几个元素（注意索引从1开始），象数组一样方便

```
CREATE function Get_StrArrayStrOfIndex 
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
```

调用示例：select  Get\_StrArrayStrOfIndex('8,9,4',',',2)

返回值：9

## 2.3. 结合以上两个函数，遍历数组中的元素

方法：结合上边两个函数，象数组一样遍历字符串中的元素

```
declare @str varchar(50) 
set @str='1,2,3,4,5' 
declare @next int 
set @next=1 
while @next<=Get_StrArrayLength(@str,',') 
begin 
print Get_StrArrayStrOfIndex(@str,',',@next) 
set @next=@next+1 
end
```

调用结果：

1

2

3

4

5
