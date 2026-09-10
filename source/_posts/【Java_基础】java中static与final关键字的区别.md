---
title: '【Java_基础】java中static与final关键字的区别'
date: 2019-03-12 09:32:00
categories:
  - Java_基础
------

# 1.static关键字

经static关键字修饰的成员被该类的所有对象所共享，任意一对象对静态变量的修改其它对象都是可见的。通常通过类名来引用static成员。类加载的连接阶段将会为静态成员变量在jvm内存的方法区分配空间，并在类的初始化阶段赋初值。

一个类的static成员变量只有“一份”（存储在方法区），无论该类创建了多少对象。看如下的示例：

```
public class StaticDemo {
    public static void main(String[] args) {
        People p1 = new People(18);
        People p2 = new People(19);
        People p3 = new People(20);

    }

}

class People {
    private static int num;
    private int age;
    public People(int age) {
        this.age = age;
        System.out.println(++num);
    }
}
```

输出结果：

```
1
2
3
```

# 2.final关键字

1）final修饰变量：意为其值不可改（即为常量），在类的编译阶段成员常量就会被初始化，并保存在class文件的静态常量池中，待类加载的时候再将静态常量池中的常量读取到jvm内存方法区，保存在方法区的运行时常量池中。

2）final修饰方法：表示方法不能被重写。

3）final修饰类：表示该类为终极类，不能被继承。JDK中的一些基本类库被定义成final，例如String，Math，Integer等，这样可以防止对系统造成危害。

final和static关键字的区别：final可修饰局部变量，任何位置经final修饰的变量其值是不可改的；static不可修饰局部变量，经static修饰的变量其值是可以更改的。
