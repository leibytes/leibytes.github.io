---
title: '【Java_多线程并发编程】基础篇——线程状态扭转函数'
date: 2019-08-29 02:01:00
categories:
  - Java_多线程并发编程
------

## 1. wait() sleep() yield() join()用法与区别

本文提到的当前线程是指：当前时刻，获得CPU资源正在执行的线程。

### 1.1 wait()方法

wait()方法定义在Object类中，它的作用是让当前线程由“[运行状态](http://www.cnblogs.com/skywang12345/p/3479024.html)”进入到“[等待(阻塞)状态](http://www.cnblogs.com/skywang12345/p/3479024.html)”，同时释放它所持有的锁。被wait()阻塞的线程可通过notify() 方法或 notifyAll() 方法唤醒，达到就绪态。

Object类中关于等待/唤醒的API详细信息如下：

**wait()** -- 让当前线程处于“等待(阻塞)状态”，“直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法”，当前线程被唤醒(进入“就绪状态”)。  
**wait(long timeout)** -- 让当前线程处于“等待(阻塞)状态”，“直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量”，当前线程被唤醒(进入“就绪状态”)。  
**wait(long timeout, int nanos)**-- 让当前线程处于“等待(阻塞)状态”，“直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量”，当前线程被唤醒(进入“就绪状态”)。  
**notify()**-- 唤醒在此对象监视器上等待的单个线程。  
**notifyAll()**-- 唤醒在此对象监视器上等待的所有线程。

wait()和notify()用法示例:

```
// WaitTest.java的源码  
class ThreadA extends Thread {  
    public ThreadA(String name) {  
        super(name);  
    }  
  
    public void run() {  
        synchronized (this) {  
            System.out.println(Thread.currentThread().getName() + " call notify()");  
            // 唤醒当前等待的线程  
            notify();  
        }  
    }  
}  
  
public class WaitTest {  
    public static void main(String[] args) {  
        ThreadA t1 = new ThreadA("t1");  
        synchronized (t1) {  
            try {  
                System.out.println(Thread.currentThread().getName() + " start t1");  
                // 启动“线程t1”，使其进入就绪状态  
                t1.start();  
                //阻塞当前正在执行的线程，并释放其上的同步锁资源  
                t1.wait();  
                System.out.println(Thread.currentThread().getName() + " continue");  
            } catch (InterruptedException e) {  
                e.printStackTrace();  
            }  
        }  
    }  
}
```

 运行结果：

```
main start t1
t1 call notify()
main continue
```

**注意：(1)notify(), wait()依赖于“同步锁”，而“同步锁”是对象锁持有，并且每个对象有且仅有一个。这就是为什么notify(), wait()等函数定义在Object类，而不是Thread类中的原因。**  
**(2 )这两个方法只能在synchronized同步代码块中调用。**

### **1.2 sleep()方法**

sleep() 是定义在Thread类中的静态方法，它的作用是让当前线程会由“[运行状态](http://www.cnblogs.com/skywang12345/p/3479024.html)”进入到“[休眠(阻塞)状态](http://www.cnblogs.com/skywang12345/p/3479024.html)”。它和wait()方法的区别：

* 这两个方法来自不同的类分别是Thread和Object
* 最主要是sleep方法没有释放当前线程持有的锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法(锁代码块和方法锁)。
* wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用(使用范围)
* sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常

sleep()的用法实例：

```
// SleepTest.java的源码
class ThreadA extends Thread{
    public ThreadA(String name){ 
        super(name); 
    } 
    public synchronized void run() { 
        try {
            for(int i=0; i <10; i++){ 
                System.out.printf("%s: %d\n", this.getName(), i); 
                // i能被4整除时，休眠100毫秒
                if (i%4 == 0)
                    Thread.sleep(100);
            } 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    } 
} 

public class SleepTest{ 
    public static void main(String[] args){ 
        ThreadA t1 = new ThreadA("t1"); 
        t1.start(); 
    } 
}
```

**运行结果**：

```
t1: 0
t1: 1
t1: 2
t1: 3
t1: 4
t1: 5
t1: 6
t1: 7
t1: 8
t1: 9
```

### 1.3 **yield()方法**

yield()是定义在Thread类中的静态方法，作用是让当前线程让步。即让当前线程由“[运行状态](http://www.cnblogs.com/skywang12345/p/3479024.html)”进入到“[就绪状态](http://www.cnblogs.com/skywang12345/p/3479024.html)”，从而让同优先级或更高优先级的线程有执行机会。但是，并不能保证在当前线程调用yield()方法之后，具有相同优先级的其它线程就一定能获得执行机会，也有可能是当前线程又进入到“运行状态”继续运行。

yield()用法实例：

```
// YieldTest.java的源码
class ThreadA extends Thread{
    public ThreadA(String name){ 
        super(name); 
    } 
    public synchronized void run(){ 
        for(int i=0; i <10; i++){ 
            System.out.printf("%s [%d]:%d\n", this.getName(), this.getPriority(), i); 
            // i整除4时，调用yield
            if (i%4 == 0)
                Thread.yield();
        } 
    } 
} 

public class YieldTest{ 
    public static void main(String[] args){ 
        ThreadA t1 = new ThreadA("t1"); 
        ThreadA t2 = new ThreadA("t2"); 
        t1.start(); 
        t2.start();
    } 
}
```

运行结果：

```
t1 [5]:0
t2 [5]:0
t1 [5]:1
t1 [5]:2
t1 [5]:3
t1 [5]:4
t1 [5]:5
t1 [5]:6
t1 [5]:7
t1 [5]:8
t1 [5]:9
t2 [5]:1
t2 [5]:2
t2 [5]:3
t2 [5]:4
t2 [5]:5
t2 [5]:6
t2 [5]:7
t2 [5]:8
t2 [5]:9
```

**结果说明**：  
“线程t1”在能被4整数的时候，并没有切换到“线程t2”。这表明，yield()虽然可以让线程由“运行状态”进入到“就绪状态”；但是，它不一定会让其它线程获取CPU执行权(即，其它线程进入到“运行状态”)，即使这个“其它线程”与当前调用yield()的线程具有相同的优先级。

### 1.4 join() 方法

join()是定义在Thread类中的实例方法，它的作用是等待调用该方法的线程执行完毕，其它线程才能获得执行机会。

join()的用法实例：

```
// JoinTest.java的源码
public class JoinTest{ 

    public static void main(String[] args){ 
        try {
            ThreadA t1 = new ThreadA("t1"); // 新建“线程t1”

            t1.start();                     // 启动“线程t1”
            t1.join();                        // 将“线程t1”加入到“主线程main”中，并且“主线程main()会等待它的完成”
            System.out.printf("%s finish\n", Thread.currentThread().getName()); 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    } 

    static class ThreadA extends Thread{

        public ThreadA(String name){ 
            super(name); 
        } 
        public void run(){ 
            System.out.printf("%s start\n", this.getName()); 

            // 延时操作
            for(int i=0; i <1000000; i++)
               ;

            System.out.printf("%s finish\n", this.getName()); 
        } 
    } 
}
```

**运行结果**：

```
t1 start
t1 finish
main finish
```

**总结：这四个方法中前三个都是针对当前线程的操作，与调用它的线程无关，只有最后一个方法是让调用该方法的线程执行完毕。**
