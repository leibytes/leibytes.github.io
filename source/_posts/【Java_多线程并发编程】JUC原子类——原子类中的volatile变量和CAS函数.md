---
title: '【Java_多线程并发编程】JUC原子类——原子类中的volatile变量和CAS函数'
date: 2019-08-29 01:54:00
categories:
  - Java_多线程并发编程
------

JUC中的原子类是依靠volatile变量和`Unsafe类中的`CAS函数实现的。

## 1. volatile变量的特性

1. 内存可见性（当一个线程修改volatile变量的值后，另一个线程就可以实时看到此变量的更新值，也可以理解为敏感性）
2. 禁止指令重排（位于volatile变量之前的变量执行先于volatile变量执行，volatile之后的变量执行在volatile变量之后）

## 2. CAS函数保证数据更新的原子性

CAS是Unsafe 类中定义的函数，它只有如下三种形式：

```
public final native boolean compareAndSwapInt(Object paramObject, long paramLong, int paramInt1, int paramInt2);

public final native boolean compareAndSwapLong(Object paramObject, long paramLong1, long paramLong2, long paramLong3);
```

```
public final native boolean compareAndSwapObject(Object paramObject1, long paramLong, Object paramObject2, Object paramObject3);
```

我们发现Unsafe类只提供了3种CAS方法：compareAndSwapInt、compareAndSwapLong和compareAndSwapObject，且都是native方法

**AtomicBoolean 源码解析**

```
public class AtomicBoolean implements java.io.Serializable {
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;

    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicBoolean.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }

    private volatile int value;

    public AtomicBoolean(boolean initialValue) {
        value = initialValue ? 1 : 0;
    }
    public final boolean compareAndSet(boolean expect, boolean update) {
        int e = expect ? 1 : 0;
        int u = update ? 1 : 0;
        return unsafe.compareAndSwapInt(this, valueOffset, e, u);
    }
    ...
}
```

从AtomicBoolean源码，发现它底层也是使用volatile类型的int 变量，跟AtomicInteger 实现方式一样，只不过是把Boolean转换成 0和1进行操作。  
所以原子更新char、float和double变量也可以转换成int 或long来实现CAS的操作。

### 2.1 CAS的优点

1. CAS即为Compare-And-Swap，是一条CPU的原子指令。CAS操作需要输入两个数值，一个是读取到变量的旧值和一个新值，在操作期间首先比较变量的旧值与当前值是否一致，若一致则更新成新值，若不一致则重复上述操作直到成功为止。  
  
2. CAS操作是原子性的，所以多线程并发中使用CAS更新数据，就可以不使用独占锁。独占锁是一种悲观锁，synchronized就是一种独占锁，会导致其它所有需要锁的线程挂起，等待持有锁的线程释放锁。而另一个更加有效的锁就是乐观锁。所谓乐观锁就是，每次不加锁而是假设没有冲突而去完成某项操作，如果因为冲突失败就重试，直到成功为止,乐观锁用到的机制就是CAS,如果不使用CAS，在高并发下，多线程同时修改一个变量的值我们需要synchronized加锁（可能有人说可以用Lock加锁，Lock底层的AQS也是基于CAS进行获取锁的）。  
  
3.相信sql大家都熟悉，类似sql中的条件更新一样：update set id=3 from table where id=2。因为单条sql执行具有原子性，如果有多个线程同时执行此sql语句，只有一条能更新成功。

### 2.2 CAS的缺点

1.ABA问题。因为CAS需要在操作值的时候检查下值有没有发生变化，如果没有发生变化则更新，但是如果一个值原来是A，变成了B，又变成了A，那么使用CAS进行检查时会发现它的值没有发生变化，但是实际上却变化了。ABA问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加一，那么A-B-A 就会变成1A-2B-3A。  
从Java1.5开始JDK的atomic包里提供了一个类AtomicStampedReference来解决ABA问题。这个类的compareAndSet方法作用是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

2. 循环时间长开销大。自旋CAS如果长时间不成功，会给CPU带来非常大的执行开销。
