---
title: '【Java基础】java中的反射机制与动态代理'
date: 2019-03-06 11:50:00
categories:
  - Java_基础
------

# 一、java中的反射机制

java反射的官方定义：在运行状态下，可以获取任意一个类的所有属性和方法，并且可通过某类任意一对象实例调用该类的所有方法。这种动态获取类的信息及动态调用类中方法的功能称为java的反射机制。

获取一个类的Class对象是应用反射机制的前提，获取Class对象的方式有如下三种：

1. instance.getClass()，这个是Object类里面的方法
2. Type.Class属性，任何的数据类型，基本数据类型或者抽象数据类型，都可以通过这种方式获取类
3. Class.forName("类的全名")，Class类提供了这样一个方法，让我们通过类名来获取到对象类

1. 通过反射机制操作某个类的属性

```
package net.xsoftlab.baike;
import java.lang.reflect.Field;
public class TestReflect {
    private String proprety = null;
    public static void main(String[] args) throws Exception {
        Class<?> clazz = Class.forName("net.xsoftlab.baike.TestReflect");
        Object obj = clazz.newInstance();
        // 可以直接对 private 的属性赋值
        Field field = clazz.getDeclaredField("proprety");
        field.setAccessible(true);
        field.set(obj, "Java反射机制");
        System.out.println(field.get(obj));
    }
}
```

2. 通过反射机制调用某个类的方法

```
package net.xsoftlab.baike;
import java.lang.reflect.Method;
public class TestReflect {
    public static void main(String[] args) throws Exception {
        Class<?> clazz = Class.forName("net.xsoftlab.baike.TestReflect");
        // 调用TestReflect类中的reflect1方法
        Method method = clazz.getMethod("reflect1");
        method.invoke(clazz.newInstance());
        // Java 反射机制 - 调用某个类的方法1.
        // 调用TestReflect的reflect2方法
        method = clazz.getMethod("reflect2", int.class, String.class);
        method.invoke(clazz.newInstance(), 20, "张三");
        // Java 反射机制 - 调用某个类的方法2.
        // age -> 20. name -> 张三
    }
    public void reflect1() {
        System.out.println("Java 反射机制 - 调用某个类的方法1.");
    }
    public void reflect2(int age, String name) {
        System.out.println("Java 反射机制 - 调用某个类的方法2.");
        System.out.println("age -> " + age + ". name -> " + name);
    }
}
```

# 二、动态代理

## 2.1 动态代理AOP

java代理模式的定义：用一个对象来控制对另一个对象的访问，称控制对象为代理对象，被控制对象为被代理对象或真实对象。通过调用代理对象的方法来间接访问被代理对象中的同名方法，要保证二者具有同名方法通常将这些同名方法抽象成接口，由二者共同实现。

动态代理和静态代理的区别：静态代理必须要为每个真实类定义一个代理类，而动态代理中无需定义代理类，由java反射包中的Proxy类动态的创建代理对象或代理类。

在java的动态代理机制中，有一个类和接口至关重要，Proxy类用于动态创建代理类或代理对象，调度处理器接口InvocationHandler。每个动态代理对象必须与实现了InvocationHandler接口的实例关联，代理对象每个方法的内部都调用了此关联实例的invoke 方法，而invoke 方法的内部又利用反射机制调用真是对象的方法，从而实现了由代理对象访问真实对象的目的。

下面是实现动态代理的经典案例：

![](/images/Java_基础/java中的反射机制与动态代理_001.gif)![](/images/Java_基础/java中的反射机制与动态代理_004.gif)

```
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

//定义真实类和代理类共同的接口
interface Subject {
    public void rent();

    public void hello(String str);
}

//定义真实类（被代理类
class RealSubject implements Subject {
    @Override
    public void rent() {
        System.out.println("I want to rent my house");
    }

    @Override
    public void hello(String str) {
        System.out.println("hello: " + str);
    }
}

//定义继承了InvocationHandler接口的调度处理器
public class MyInvocationHandler implements InvocationHandler {
    //要代理的真实对象
    private Object realObj;

    // 构造方法给要代理的真实对象赋值
    public MyInvocationHandler(Object realObj) {
        this.realObj = realObj;
    }

    /**
     * 执行动态代理对象的所有方法都会被替换为执行如下的invoke方法
     * 其中：
     * autoProxy:动态代理对象
     * method:代表正在执行的方法
     * args:由动态代理对象调用方法时传入的实参
     */
    @Override
    public Object invoke(Object autoProxy, Method method, Object[] args) throws Throwable {
        //在代理真实对象前我们可以添加一些自己的操作
        System.out.println("before rent invoke");
        //当代理对象调用真实对象的方法时，其会自动的跳转到代理对象关联的handler对象的invoke方法来进行调用
        System.out.println("Method:" + method);
        method.invoke(realObj, args);
        //在代理真实对象后我们也可以添加一些自己的操作
        System.out.println("after rent invoke");
        return null;
    }

}

class Client {
    public static void main(String[] args) {
        //要代理的真实对象
        Subject realObj = new RealSubject();
        //要代理哪个真实对象，就将该对象传进去，最后是通过该真实对象来调用其方法的
        InvocationHandler MyHandler = new MyInvocationHandler(realObj);
        /*通过Proxy类的newProxyInstance方法动态创建代理对象
         *public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,  InvocationHandler h)  throws IllegalArgumentException
         * 它的三个参数
         * 第一个参数 指定动态代理类的类加载器
         * 第二个参数 指定动态代理类需实现的接口集合
         * 第三个参数 指定与该动态代理对象所关联的调度处理器实例
         */
        Subject autoProxy = (Subject) Proxy.newProxyInstance(MyHandler.getClass().getClassLoader(), realObj.getClass().getInterfaces(), MyHandler);
        System.out.println(autoProxy.getClass().getName());
        autoProxy.rent();
        autoProxy.hello("world");
    }
}
```

View Code

  输出如下:

```
net.xsoftlab.baike.$Proxy0
before rent invoke
Method:public abstract void net.xsoftlab.baike.Subject.rent()
I want to rent my house
after rent invoke
before rent invoke
Method:public abstract void net.xsoftlab.baike.Subject.hello(java.lang.String)
hello: world
after rent invoke
```

**总结：1.动态代理中无需定义代理类，代理对象是由Proxy类动态产生的。**

**2.代理对象对方法的访问都将转换成调度处理器对其invoke 方法的访问。**

**3.调度处理器的invoke 方法内部利用反射机制去调用真是对象的同名方法。**

**4.像上面这种动态代理在Spring中被称为AOP（Aspect Orient Programming,面向切面编程），AOP代理可替代目标对象，AOP代理包含了目标对象的全部方法。**

## 2.2 AOP的应用

上面介绍了动态代理，接下来说下它的应用。在开发中如果要复用同一功能模块，可以通过复制该功能模块的代码来实现，但在开发中一般不会这样做，通常将该功能模块封装成一个方法，在需要的地方直接调用就可以了。

![](/images/Java_基础/java中的反射机制与动态代理_003.png)

通过将公共模块封装成方法来调用虽然提高了复用减少了代码的冗余，但却提高了这三个代码段与方法的耦合。最理想的效果是：代码块一，代码块二，代码块三既可以执行蓝色区域的代码部分，又无需以硬编码的方式来直接调用蓝色区域的方法。这时就可以通过动态代理来实现这个功能。

接下来使用Proxy和InvocationHandler来实现当程序调用info()或run()方法时，系统可以“自动”将method1()和method2()两个方法插入info()和run()方法中执行。  
首先提供一个Dog接口，在该接口中提供两个方法：

![](/images/Java_基础/java中的反射机制与动态代理_001.gif)![](/images/Java_基础/java中的反射机制与动态代理_004.gif)

```
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

interface Dog {
    //info()方法声明
    void info();

    //run()方法
    void run();
}

class GunDog implements Dog {
    //实现info方法，仅仅打印一个字符串
    @Override
    public void info() {
        System.out.println("我是一只猎狗");
    }

    //实现run()方法，仅仅打印一个字符串
    @Override
    public void run() {
        System.out.println("迅速奔跑");
    }
}

class DogUtil {
    public static void method1() {
        System.out.println("=====模拟第一个通用方法=====");
    }

    public static void method2() {
        System.out.println("=====模拟第二个通用方法=====");
    }
}

class MyInvocationHandler implements InvocationHandler {
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        //执行DogUtil对象中method1方法
        DogUtil.method1();
        //以target作为主调执行method方法
        Object result = method.invoke(target, args);
        //执行DogUtil对象中的method2方法
        DogUtil.method2();
        return result;
    }
}

class MyProxyFactory {
    public static Object getProxy(Object target) {
        MyInvocationHandler myInvocationHandler = new MyInvocationHandler();
        //设置需要代理的对象
        myInvocationHandler.setTarget(target);
        //创建并返回动态代理
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), myInvocationHandler);//使用指定的类加载器，需要实现的接口，自定义Handler
    }
}

public class TestAOP {
    public static void main(String[] args) {
        Dog dog = (Dog) MyProxyFactory.getProxy(new GunDog());
        dog.info();
        dog.run();
    }
}
```

View Code

 输出如下：

```
=====模拟第一个通用方法=====
我是一只猎狗
=====模拟第二个通用方法=====
=====模拟第一个通用方法=====
迅速奔跑
=====模拟第二个通用方法=====
```

下面这张图片对上面的程序流程进行了简单解释：

![](/images/Java_基础/java中的反射机制与动态代理_002.png)

本篇博文参考了：[【java】解析java类加载与反射机制](https://www.cnblogs.com/HDK2016/p/6863173.html)    [java的动态代理机制详解](https://www.cnblogs.com/xiaoluo501395377/p/3383130.html)
