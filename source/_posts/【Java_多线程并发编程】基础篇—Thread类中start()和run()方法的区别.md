---
title: '【Java_多线程并发编程】基础篇—Thread类中start()和run()方法的区别'
date: 2019-03-31 13:51:00
categories:
  - Java_多线程并发编程
------

## **1. start() 和 run()的区别说明**

start()方法： 它会启动一个新线程，并将其添加到线程池中，待其获得CPU资源时会执行run()方法，start()不能被重复调用。

run()方法：它和普通的方法调用一样，不会启动新线程。只有等到该方法执行完毕，其它线程才能获得CPU资源。

### **start() 和 run()的区别示例**

```
// Demo.java 的源码
class MyThread extends Thread{  
    public MyThread(String name) {
        super(name);
    }

    public void run(){
        System.out.println(Thread.currentThread().getName()+" is running");
    } 
}; 

public class Demo {  
    public static void main(String[] args) {  
        Thread mythread=new MyThread("mythread");
        System.out.println(Thread.currentThread().getName()+" is running");
        mythread.run();
        mythread.start();
    }  
}
```

**运行结果**：

```
main is running
main is running
mythread is running
```

**结果说明：**  
(01) Thread.currentThread().getName()是用于获取“当前线程”的名字，当前线程是指正在cpu中执行的线程。  
(02) mythread.run()是在“主线程main”中调用的，该run()方法直接运行在“主线程main”上。  
(03) mythread.start()会启动“线程mythread”，“线程mythread”启动之后，会调用run()方法；此时的run()方法是运行在“线程mythread”上。

## **2. start() 和 run()相关源码(基于JDK1.7)**

Thread.java中start()方法的源码如下：

```
public synchronized void start() {
    // 如果线程不是"就绪状态"，则抛出异常！
    if (threadStatus != 0)
        throw new IllegalThreadStateException();

    // 将线程添加到ThreadGroup中
    group.add(this);

    boolean started = false;
    try {
        // 通过start0()启动线程
        start0();
        // 设置started标记
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
        }
    }
}
```

**说明**：start()实际上是通过本地方法start0()启动一个新线程，新线程会调用run()方法。

```
private native void start0();
```

Thread.java中run()的代码如下：

```
public void run() {
    if (target != null) {
        target.run();
    }
}
```

**说明**：target是一个Runnable对象。run()就是直接调用Thread线程的run()方法，并不会新建一个线程。
