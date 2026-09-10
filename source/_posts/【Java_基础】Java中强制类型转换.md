---
title: '【Java_基础】Java中强制类型转换'
date: 2019-08-08 14:02:00
categories:
  - Java_基础
------

首先，狭义上的强制类型转换指的是引用类型，且是父类向子类转换，这种转换只牵扯到引用名义类型的转换，具体的对象内存没有发生一点变化。

而基本类型的转换与此不同，其数据确实发生了变化。如果是基本类型和其包装类的相互转化，实际上调用是Integer.parseInt()和Integer.valueOf()来完成。

父类向子类进行强制转换，如果成功不会有任何数据丢失，如果不是其子类，那么会报错。基本类型之间的转换，如果是大范围的向小范围的转换，确实**有可能**会发生数据丢失或精度丢失。

实例：

```
public class Main {
    public static void main(String[] args) {
        Base base = new Child();
        base.test();
        System.out.println(base.getClass().getName());
        base = (Base) base;
        base.test();
        System.out.println(base.getClass().getName());
    }
}

class Base {
    public void test() {
        System.out.println("我是父类方法");
    }
}

class Child extends Base {
    @Override
    public void test() {
        System.out.println("我是子类方法");
    }
    public void childSelf(){
        System.out.println("我不是重写了父类的方法");
    }
}
```

运行结果：

```
我是子类方法
com.hubu.zhoulei.algorithm.Child
我是子类方法
com.hubu.zhoulei.algorithm.Child
```
