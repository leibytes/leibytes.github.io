---
title: '【Java_基础】java中的多态性'
date: 2019-05-11 06:32:00
categories:
  - Java_基础
------

方法的重载、重写和动态链接构成了java的多态性。

# 1.方法的重载

同一个类中多个同名但形参有所差异的方法，在调用时会根据参数的不同做出选择。

# 2.方法的重写

子类中重新定义了父类的方法，有关方法重写的规则请参考文章：[Java中方法重写的注意事项](https://www.jianshu.com/p/63e42d76332e)。

# 3.动态链接

动态链接出现在父类引用指向子类对象的场景。

因为子类中有一个隐藏的引用super指向父类实例，当出现父类引用指向子类对象时，子类对象就会将其隐藏的父类实例返回给该父类引用。因此，父类引用还是指向了父类实例，而不是像表面看到的那样指向了子类对象。

下面通过代码来看父类、子类的内存分配情况:

假设现在有一个父类Father，它里面的变量需要占用1M内存。有一个它的子类Son，它里面的变量需占用0.5M内存。当创建子类对象时会率先创建父类对象，并将其隐藏的引用super指向父类对象。

Father f = new Father();//系统将分配1M内存。

Son s = new Son();//系统将分配1.5M内存。

Father f1 =s;//父类引用指向子类对象，这时f1会指向子类对象中的1M内存

![](/images/Java_基础/java中的多态性_003.png)

由于f1仅指向了子类对象中的父类实例，所以f1只能调用父类的方法，而不能调用子类的方法。但是，若子类重写了父类的方法，那么该父类引用将会调用子类的方法，而不在是父类的方法，这就是java中的动态链接。

## 3.1.动态链接带来的价值

JAVA里没有多继承，一个类只能有一个父类。而继承的表现就是多态，一个父类可以有多个子类，而在子类里可以重写父类的方法，每个子类里重写的代码不一样，自然表现形式也就不一样。当同一父类引用指向不同的子类实例，调用同一方法却具有不同的表现形式，这就是多态。相同的消息（也就是调用相同的方法）会有不同的结果。

举例说明：

![](/images/Java_基础/java中的多态性_002.gif)![](/images/Java_基础/java中的多态性_001.gif)

```
//父类

public class Father{

//父类有一个打孩子方法

public void hitChild(){

}

}

//子类1

public class Son1 extends Father{

//重写父类打孩子方法

public void hitChild(){

System.out.println("为什么打我？我做错什么了！");

}

}

//子类2

public class Son2 extends Father{

//重写父类打孩子方法

public void hitChild(){

System.out.println("我知道错了，别打了！");

}

}

//子类3

public class Son3 extends Father{

//重写父类打孩子方法

public void hitChild(){

System.out.println("我跑，你打不着！");

}

}

//测试类

public class Test{

public static void main(String args[]){

Father father;

father = new Son1();

father.hitChild();

father = new Son2();

father.hitChild();

father = new Son3();

father.hitChild();

}

}
```

View Code

## 3.2.动态链接的总结

1、使用父类引用指向子类对象是动态链接的应用场景

2、该引用只能调用父类中定义的方法和变量 ，如果子类中重写了父类中的方法，那么在调用这个方法的时候，将会调用子类中的这个方法；（动态连接、动态调用）。

3、变量不能被重写（覆盖），"重写"的概念只针对方法，如果在子类中重写父类中的变量，那么在编译时会报错。

4、父类引用指向子类实例时，方法调用采用“编译看左边，运行看右边”规则来判断。

本博文参考了：[java多态性，父类引用指向子类对象](https://www.cnblogs.com/shilin/p/4784862.html)
