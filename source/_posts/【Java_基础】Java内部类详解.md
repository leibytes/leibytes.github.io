---
title: '【Java_基础】Java内部类详解'
date: 2019-04-21 03:17:00
categories:
  - Java_基础
------

# 1.四种内部类

java中的四种内部类：成员内部类、静态内部类、局部内部类和匿名内部类。其中匿名内部类用到的最多。

## 1.1.成员内部类

若一个类定义在另一个类的内部作为实例成员，我们把这个作为实例成员的类称之为成员内部类，也可以叫做实例成员内部类。

在成员内部类的里面可以无条件的访问外部类的任何成员（包括private成员和静态成员）。

```
class Circle {

    private double radius = 0;

    public static int count =1;

    public Circle(double radius) {

        this.radius = radius;

    }     

    class Draw {     //内部类无条件的访问外部类成员

        public void drawSahpe() {

            System.out.println(radius);  //外部类的private成员

            System.out.println(count);   //外部类的静态成员

        }

    }

}
```

但是，在外部类中访问成员内部类的成员，必须要通过内部类的对象才能访问，而不能直接访问。

```
class Circle {

    private double radius = 0;

    public Circle(double radius) {

        this.radius = radius;

        getDrawInstance().drawSahpe();   //必须先创建成员内部类的对象，再进行访问

    }     

    private Draw getDrawInstance() {

        return new Draw();

    }     

    class Draw {     //内部类

        public void drawSahpe() {

            System.out.println(radius);  //外部类的private成员

        }

    }

}
```

## 1.2.静态内部类

若一个类定义在另一个类的内部且加了static关键字作为静态成员，我们把这个作为静态成员的类称之为静态内部类。

静态内部类是不需要依赖于外部类实例的，这点和类的其它静态成员一样，作为静态成员他只能访问外部类的静态成员 ，而不能访问外部类的实例成员。

```
public class OuterClass {

private String outerClass1="我是外部类的实例成员";

private static String outerClass2="我是外部类的静态成员";

private static class StaticInnerClass{

public void printOuterClassMumber(){

//静态内部类里面只能访问外部类的静态成员

//System.out.println(outerClass1);此句需被注释掉否则会报错

System.out.println(outerClass2);

}
}
}
```

同成员内部类一样，外部类中也不能直接访问静态内部类成员，必须要通过内部类的对象才能访问。但是和成员内部类不同的是，静态内部类中可以定义静态成员，若外部类访问的是静态内部类的静态成员，则可以通过类名来完成，就无需创建静态内部类对象。

```
public class OuterClass {
private static class StaticInnerClass{
private String staticInnerClass1="我是静态内部类的实例成员变量";
private static String staticInnerClass2="我是静态内部类的静态成员变量";
}
public void staticInnerClassTest(){
//必须要通过内部类对象才能访问实例变量
System.out.println(new StaticInnerClass().staticInnerClass1);
//可以通过类名来访问静态变量
System.out.println(StaticInnerClass.staticInnerClass2);
}
}
```

## 1.3.局部内部类

局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法或者该作用域内。局部内部类同局部变量一样，不能加如何访问修饰符，在其作用域外不能被访问。

```
class OuterClass {
public void localInnerClassTest(){
System.out.println("<<<<<<<<<<<<<<<<测试局部内部类>>>>>>>>>>>>>>>>>");
class LocalInnerClass{
public void pritTest(){
System.out.println("我是局部内部类");
}
}
new LocalInnerClass().pritTest();
}
}
```

## 1.4.匿名内部类

匿名内部类通过父类类名加上子类类体new出子类对象，减少了单独定义子类的环节。

```
public class OuterClass {
public void anonymousInnerClassTest(){
System.out.println("<<<<<<<<<<<<<<<<测试匿名内部类>>>>>>>>>>>>>>>>>");
new AnonymousClass(){
public void printAnonymousClass(){
System.out.println("我是生成的匿名内部类");
}
}.printAnonymousClass();
}

}
```

# 2.深入理解内部类

## 2.1.成员内部类为什么可以无条件的访问外部类的所有成员

我们通过查看编译后的文件可知，编译后内部类和外部类不再是嵌套关系，而是在同一个包下的两个类，并将内部类命名为"外部类类名$内部类类名.class"。

![](/images/Java_基础/Java内部类详解_001.png)

不仅如此，通过反编译成员内部类.class文件可知，编译器为内部类生成了一个指向外部类对象的引用this$0和一个以外部类对象引用作为形参的构造函数。当创建内部类对象时，通过构造函数给对象引用this$0赋值，更为重要的是针对外部类的私有成员还会创建访问他们的accesso方法。结合指向外部类的对象和访问私有成员的方法，才实现了内部类对外部类的无条件访问。这里也间接说明了成员内部类是依赖于外部类的，如果没有创建外部类的对象，则无法对Outter this&0进行赋值，也就无法创建成员内部类对象了。

下面是经编译后的成员内部类字节码文件：

```
class OuterClass$MumberInnerClass {
private String mumberInnerClass1;
private OuterClass$MumberInnerClass(OuterClass var1) {
this.this$0 = var1;
this.mumberInnerClass1 = "我是成员内部类的实例变量";
}
public void printOuterClassMumber() {
System.out.println(OuterClass.access$000(this.this$0));
System.out.println(OuterClass.access$100());
}
}
```

## 2.2.外部类中为什么要创建内部类的对象才能访问内部类成员

成员内部类中对外部类的访问是无条件的，但是外部类中访问内部类成员需要通过内部类对象才能完成。原因在于编译器并未在外部类中自动创建内部类对象，所以需要自己去创建。

下面是编译后的外部类字节码文件，可以看出外部类经编译后没有自动创建内部类对象。

![](/images/Java_基础/Java内部类详解_002.gif)![](/images/Java_基础/Java内部类详解_003.gif)

```
package innerClassTest;

public class OuterClass {
private String outerClass1 = "我是外部类的实例成员";
private static String outerClass2 = "我是外部类的静态成员";

public OuterClass() {
}

public void mumberInnerClassTest() {
System.out.println("<<<<<<<<<<<<<<<<1.测试成员内部类>>>>>>>>>>>>>>>>>");
System.out.println((new OuterClass.MumberInnerClass(null)).mumberInnerClass1);
(new OuterClass.MumberInnerClass(null)).printOuterClassMumber();
}

public void staticInnerClassTest() {
System.out.println("<<<<<<<<<<<<<<<<2.测试静态内部类>>>>>>>>>>>>>>>>>");
System.out.println((new OuterClass.StaticInnerClass(null)).staticInnerClass1);
System.out.println(OuterClass.StaticInnerClass.staticInnerClass2);
(new OuterClass.StaticInnerClass(null)).printOuterClassMumber();
}

public void localInnerClassTest() {
System.out.println("<<<<<<<<<<<<<<<<3.测试局部内部类>>>>>>>>>>>>>>>>>");
class LocalInnerClass {
LocalInnerClass() {
}

public void pritTest() {
System.out.println("我是局部内部类");
}
}

(new LocalInnerClass()).pritTest();
}

public void anonymousInnerClassTest() {
System.out.println("<<<<<<<<<<<<<<<<测试匿名内部类>>>>>>>>>>>>>>>>>");
(new AnonymousClass() {
public void printAnonymousClass() {
System.out.println("我是生成的匿名内部类");
}
}).printAnonymousClass();
}

private static class StaticInnerClass {
private String staticInnerClass1;
private static String staticInnerClass2 = "我是静态内部类的静态成员变量";

private StaticInnerClass() {
this.staticInnerClass1 = "我是静态内部类的实例成员变量";
}

public void printOuterClassMumber() {
System.out.println(OuterClass.outerClass2);
}
}

private class MumberInnerClass {
private String mumberInnerClass1;

private MumberInnerClass() {
this.mumberInnerClass1 = "我是成员内部类的实例变量";
}

public void printOuterClassMumber() {
System.out.println(OuterClass.this.outerClass1);
System.out.println(OuterClass.outerClass2);
}
}
}
```

View Code

## 2.3.四种内部类的区别

1.四种内部类经编译后均会产生独立的class文件，除了静态内部类以外编译器均会为内部类添加一个指向外部类的引用，使得内部类能无条件访问外部类成员。

2.虽然内部类经编译后会产生独立的字节码文件，但是仍然要满足作为外部类成员的要求，即成员内部类中不能定义静态成员、静态内部类中可以定义静态成员、局部内部类同局部变量一样不能被其作用域以外的地方访问。

3.在外部类的外面创建内部类对象

a.由于成员内部类是依赖于外部类的，所以创建成员内部类对象必须先创建外部类对象。

b.静态内部类是不依赖外部类的，因此可以无需创建外部类对象而直接创建静态内部类对象。

下面是外部类之外创建内部类对象的案例：

```
public class Test {
public static void main(String[] args) {
OuterClass outerClass=new OuterClass();
OuterClass.MumberInnerClass mumberInnerClassInstance=outerClass.new MumberInnerClass();
mumberInnerClassInstance.printOuterClassMumber();
OuterClass.StaticInnerClass staticInnerClassInstance=new OuterClass.StaticInnerClass();
staticInnerClassInstance.printOuterClassMumber();
}
}
```

本文参考与：[Java内部类详解](https://www.cnblogs.com/dolphin0520/p/3811445.html) [Java外部类可以访问内部类private变量](https://www.cnblogs.com/cielosun/p/6591444.html)
