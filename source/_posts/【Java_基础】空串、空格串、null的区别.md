---
title: '【Java_基础】空串、空格串、null的区别'
date: 2019-04-25 09:45:00
categories:
  - Java_基础
------

## 1、表示的区别

string str1 = ""; //空串 str1.length() 等于 0  
 string str2 = " "; //空格串 str2.length() 等于 1  
 string str3 = null; //NULL

## 2、内存区别

"" ：分配了一个内存空间  
 " " ：分配了内存,其ASCII码值为32  
 null ：未分配内存空间

## 3、使用的区别

null是一个空对象，在内存中是不存在的，null调用字符串方法会抛出异常。  
 ""是一个字符串(String).它在内存中是存在的.它可以使用Object对象中的方法(如"".toString();"".equals()) 。

## 4、判断字符串是否为空的方法

1.直观的：  
if(s == null ||"".equals(s));　　　　//先判断是否对象，再判断是否是空字符串  
2.比较字符串长度, 效率高, 比较绕:  
if(s == null || s.length() <= 0);  
3.效率和方法三几乎相等, 但出于兼容性考虑,不建议此方法.  
if(s == null || s.isEmpty());  
4.看起来高大上的：  
if(StringUtils.isNotBlank(str))　　　　//判断字符串不为空  
if(StringUtils.isBlank(str))　　　　//判断字符串为空
