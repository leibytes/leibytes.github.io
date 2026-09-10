---
title: '【Java_基础】java中的常量池'
date: 2019-03-13 14:03:00
categories:
  - Java_基础
------

## 1.java常量池的介绍

java中的常量池，通常指的是运行时常量池，它是方法区的一部分，一个jvm实例只有一个运行常量池，各线程间共享该运行常量池。

java常量池简介：java常量池中保存了一份在编译期间就已确定的数据。它里面包括final常量的值（包括成员常量、局部常量和引用常量）、以及对象字面量的值。

在编译期间，每当给常量赋值它就会去检测常量池中是否存在该值，若存在直接返回该值的地址给常量，若不存在则先在常量池中创建该值，再返回该值的地址给常量。因此常量池中不可能出现相等的数据。

## 2.final常量

一切经final关键字修饰的变量均为常量，final常量必须在定义时就赋初值，否则编译不通过。

## 3.对象字面量

对象字面量是指直接以一常量给对象赋值，而不是在堆空间new出一个对象实例。

常见的两种对象字面量：基本类型的包装类对象字面量、String对象字面量。

### 3.1基本类型的包装类对象字面量

java中基本类型的包装类大都实现了常量池技术，即Byte,Short,Integer,Long,Character,Boolean。这5种包装类默认创建了数值[-128，127]的相应类型的缓存数据，但是超出此范围仍然会去创建新的对象。 两种浮点数类型的包装类Float,Double并没有实现常量池技术。

包装类型Integer与常量池

```
Integer i1 = 40;
Integer i2 = 40;
Integer i3 = 0;
Integer i4 = new Integer(40);
Integer i5 = new Integer(40);
Integer i6 = new Integer(0);

System.out.println("i1=i2   " + (i1 == i2));
System.out.println("i1=i2+i3   " + (i1 == i2 + i3));
System.out.println("i1=i4   " + (i1 == i4));
System.out.println("i4=i5   " + (i4 == i5));
System.out.println("i4=i5+i6   " + (i4 == i5 + i6));  
System.out.println("40=i5+i6   " + (40 == i5 + i6));

i1=i2   true
i1=i2+i3   true
i1=i4   false
i4=i5   false
i4=i5+i6   true
40=i5+i6   true
```

解释：

* Integer i1=40；直接以字面量给对象赋值，它会先去检查常量池中是否存在该值，若存在直接返回该值的地址，若不存在则现在常量池中创建该值，再返回该值的地址。
* Integer i1 = new Integer(40);这种情况会在堆空间创建新的对象。
* 语句i4 == i5 + i6，因为+这个操作符不适用于Integer对象，首先i5和i6进行自动拆箱操作，进行数值相加，即i4 == 40。然后Integer对象无法与数值进行直接比较，所以i4自动拆箱转为int值40，最终这条语句转为40 == 40进行数值比较。

### 3.2String对象字面量

```
String str1 = "abcd";
String str2 = new String("abcd");
System.out.println(str1==str2);//false

String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";
String str4 = str1 + str2;
System.out.println("string" == "str" + "ing");// true
System.out.println(str3 == str4);//false

String str5 = "string";
System.out.println(str3 == str5);//true
```

解释：

* 引用str1指向常量池中字符串"abcd"的地址，是在常量池中拿对象，new String("abcd")是直接在堆内存空间创建一个新的对象。只要使用new方法，便需要创建新的对象。
* 连接表达式 +，只有使用引号包含文本的方式创建的String对象之间使用“+”连接产生的新对象才会被加入常量池中。
* 对于字符串变量的“+”连接表达式，它所产生的新对象都不会被加入字符串池中，其属于在运行时创建的字符串，具有独立的内存地址,所以不引用自同一String对象。

（3）String.intern()方法强制将字符串放入常量池中

```
public static void main(String[] args) {
　　String s1 = new String("计算机");
　　String s2 = s1.intern();
　　String s3 = "计算机";
　　System.out.println("s1 == s2? " + (s1 == s2));
　　System.out.println("s3 == s2? " + (s3 == s2));
}

s1 == s2? false
s3 == s2? true
```

**解释：**String的intern()方法会查找在常量池中是否存在一份equal相等的字符串,如果有则返回该字符串的引用,如果没有则添加自己的字符串进入常量池。

**注意：**final常量必须在定义时就赋初值，但对象字面量可以先定义后赋值。

# 4. 常量池的好处

常量池是为了避免频繁的创建和销毁对象而影响系统性能，实现了常量池中的内容由对象共享。例如字符串常量池，在编译阶段就把所有的字符串文字放到一个常量池中。

* 节省内存空间：常量池中所有相同的字符串常量被合并，只占用一个空间。
* 节省运行时间：比较字符串时，==比equals()快。对于两个引用变量，只用==判断引用是否相等，也就可以判断实际值是否相等。

**注：**==号比较基本数据类型是值比较，但比较引用类型则是引用所指向的地址比较。

参考的博文：[java常量池技术](https://yq.aliyun.com/articles/49481)
