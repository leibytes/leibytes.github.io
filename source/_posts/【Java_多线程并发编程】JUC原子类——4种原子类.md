---
title: '【Java_多线程并发编程】JUC原子类——4种原子类'
date: 2019-04-02 09:37:00
categories:
  - Java_多线程并发编程
------

根据修改的数据类型，可以将JUC包中的原子操作类可以分为4种，分别是：

1. **基本类型**: AtomicInteger, AtomicLong, AtomicBoolean ;  
2. **数组类型**: AtomicIntegerArray, AtomicLongArray, AtomicReferenceArray ;  
3. **引用类型**: AtomicReference, AtomicStampedRerence, AtomicMarkableReference ;  
4. **对象的属性修改类型**: AtomicIntegerFieldUpdater, AtomicLongFieldUpdater, AtomicReferenceFieldUpdater 。

有关原子类的操作可参考博文：[JUC原子类](https://www.cnblogs.com/skywang12345/p/java_threads_category.html)
