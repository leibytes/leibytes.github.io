---
title: '【Java_多线程并发编程】基础篇——synchronized关键字'
date: 2019-03-31 14:36:00
categories:
  - Java_多线程并发编程
------

## **1. synchronized同步锁的****原理**

当我们调用某对象的synchronized方法或代码块时，就获取了该对象的同步锁。例如，synchronized(obj)就获取了“obj这个对象”的同步锁。  
不同线程对同步锁的访问是互斥的。也就是说某一时刻，对象的同步锁只能被一个线程获取到。通过同步锁，我们就能在多线程中，实现对“对象/方法”的互斥访问。 例如，现在有两个线程A和线程B，它们都会访问“对象obj的同步锁”。假设，在某一时刻，线程A获取到“obj的同步锁”并在执行一些操作；而此时，线程B也企图获取“obj的同步锁” —— 线程B会获取失败，它必须等待，直到线程A释放了“该对象的同步锁”之后线程B才能获取到“obj的同步锁”从而才可以运行。

## **2. 使用****同步锁三****规则**

第一条: 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程对“该对象”的该“synchronized方法”或者“synchronized代码块”的访问将被阻塞。  
第二条: 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程仍然可以访问“该对象”的非同步代码块。  
第三条: 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程对“该对象”的其他的“synchronized方法”或者“synchronized代码块”的访问将被阻塞。

综上所述：synchronized(obj)是对实例obj上的所有synchronized资源上了锁，而对实例的非synchronized资源未上锁。

## 3. **实例锁和全局锁**

实例锁 -- 同一实例上的同步锁，在某一时刻只能由一个线程能访问，其它线程要访问该锁只能同步等待，直到当前线程使用完该锁并释放锁资源。如果该类是单例类，那么该锁也具有全局锁的概念。实例锁对应的就是synchronized关键字。     

全局锁 -- 线程对该类的所有对象上锁资源访问均是互斥的，无论实例多少个对象，那么线程都共享该锁。全局锁对应的就是static synchronized（或者是锁在该类的class或者classloader对象上）。

有关同步锁的详解参考博文：[Java多线程系列--“基础篇”04之 synchronized关键字](https://www.cnblogs.com/skywang12345/p/3479202.html)
