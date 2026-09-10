---
title: '【Java框架】控制反转IOC(Inversion of Control)'
date: 2019-04-04 04:19:00
categories:
  - Java框架
------

## 1. IOC的概念

控制反转IoC(Inversion of Control)是一种设计思想，而DI(依赖注入)是实现IoC的一种方法。在没有使用IOC的程序中，对象间的依赖关系是靠硬编码的方式实现的。引入IOC后对象的创建由程序自己控制的，控制反转即将对象的创建交给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![](</images/Java框架/控制反转IOC(Inversion_of_Control)_001.png>)

IoC是Spring框架的核心内容，在IOC容器中一切对象皆为Bean组件。IOC容器通过读取XML配置文件中的Bean信息，产生每个Bean实例。也可以使用注解，新版本的Spring也可以零配置实现IoC。

## 2.  使用**XML配置文件实现依赖注入**

```
/**人*/
public abstract class Person {
    public String name;
}
```

```
/**学生*/
public class Student extends Person {
    /**身高*/
    public int height;
    /**有参构造方法*/
    public Student(String name,int height){
        this.name=name;
        this.height=height;
    }
    @Override
    public String toString() {
        return "Student{" + "height=" + height+",name="+name +'}';
    }
}
```

```
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class School {
    public static void main(String[] args) {
        //IoC容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("beans02.xml");
        //从容器中获取对象
        Person tom=ctx.getBean("tom",Person.class);
        System.out.println(tom);
    }
}
```

### 2.1 通过参数名来注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="tom" class="spring02.Student">
        <constructor-arg name="name" value="张柏川"></constructor-arg>
        <constructor-arg name="height" value="195"></constructor-arg>
    </bean>
</beans>
```

### 2.2 通过参数下标来注入

```
<bean id="rose" class="spring02.Student">
        <constructor-arg index="0" value="张柏芝"></constructor-arg>
        <constructor-arg index="1" value="196"></constructor-arg>
</bean>
```

### 2.3 通过属性赋值

```
<bean id="jack" class="spring02.Student">
        <property name="name" value="张三"></property >
        <property  name="height" value="195"></property >
 </bean>
```

**注意：**这种通过property 注入的方式，只能对有构造器（即有set和get方法）的属性注入，对与没有构造器的属性不能用这种方式注入

**特殊情况：若注入的属性为引用类型，则通过ref指定属性值**

Student.java

```
/**学生*/
public class Student {
     /**姓名*/
    String name;
    /**身高*/
    public int height;
     /**地址*/
    public Address address;
    /**有参构造方法*/
    public Student(String name,int height){
        this.name=name;
        this.height=height;
    }
    /**有参构造方法*/
    public Student(String name,int height,Address address){
        this.name=name;
        this.height=height;
        this.address=address;
    }
    @Override
    public String toString() {
        return "Student{" + "height=" + height+",name="+name +'}'+address;
    }
}
```

Address.java

```
/**地址*/
public class Address {
    /**国家*/
    private String country;
    /**城市*/
    private String city;
    public Address() {
    }
    public Address(String country, String city) {
        this.country = country;
        this.city = city;
    }
    @Override
    public String toString() {
        return "Address{" +
                "country='" + country + '\'' +
                ", city='" + city + '\'' +
                '}';
    }

    public String getCountry() {
        return country;
    }

    public void setCountry(String country) {
        this.country = country;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }
}
```

通过配置文件注入引用类型属性

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean name="zhuhai" class="spring02.Address">
        <property name="country" value="中国"></property>
        <property name="city" value="珠海"></property>
    </bean>
    <bean id="tom" class="spring02.Student">
        <constructor-arg name="name" value="张柏川"></constructor-arg>
        <constructor-arg name="height" value="195"></constructor-arg>
        <constructor-arg name="address" ref="zhuhai"></constructor-arg>
    </bean>
    <bean id="rose" class="spring02.Student">
        <constructor-arg index="0" value="张柏芝"></constructor-arg>
        <constructor-arg index="1" value="196"></constructor-arg>
        <constructor-arg index="2" ref="zhuhai"></constructor-arg>
    </bean>
</beans>
```

 测试代码：

```
package spring02;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class School {
    public static void main(String[] args) {
        //IoC容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("bookbean01.xml","beans02.xml");
        //从容器中获取对象
        Person tom=ctx.getBean("tom",Person.class);
        Person rose=ctx.getBean("rose",Person.class);
        //Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(tom);
        System.out.println(rose);
    }
}
```

### 2.4 scope属性控制从容器中取回对象的作用域

从容器中取回的对象默认是单例的

```
Person roseA=ctx.getBean("rose",Person.class);
        Person roseB=ctx.getBean("rose",Person.class);
        //Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(tom);
        System.out.println(roseA==roseB);
```

运行结果：

![](</images/Java框架/控制反转IOC(Inversion_of_Control)_002.png>)

scope属性可取值如下：

![](</images/Java框架/控制反转IOC(Inversion_of_Control)_003.png>)

scope属性取值prototype的实例：

```
<bean id="rose" class="spring02.Student" scope="prototype">
        <constructor-arg index="0" value="张柏芝"></constructor-arg>
        <constructor-arg index="1" value="196"></constructor-arg>
        <constructor-arg index="2" ref="zhuhai"></constructor-arg>
    </bean>
```

```
//从容器中获取对象
        Person tom=ctx.getBean("tom",Person.class);
        Person roseA=ctx.getBean("rose",Person.class);
        Person roseB=ctx.getBean("rose",Person.class);
        //Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(tom);
        System.out.println(roseA==roseB);
```

运行结果：

![](</images/Java框架/控制反转IOC(Inversion_of_Control)_004.png>)

### 2.5 `lazy-init`属性控制Bean的延迟实例化

`ApplicationContext`默认是在启动时将所有 singleton bean提前实例化。 通常这样的提前实例化方式是好事，因为配置中或者运行环境的错误就会被立刻发现，否则可能要花几个小时甚至几天。如果你不想这样，你可以将单例bean定义为延迟加载防止它提前实例化。延迟初始化bean会告诉Ioc容器在第一次需要的时候才实例化而不是在容器启动时就实例化。

实例：

```
<bean id="lazytom" class="spring_IoC.dao.Student" lazy-init="true" scope="prototype">  
    <constructor-arg name="name" value="张柏川"></constructor-arg>  
    <constructor-arg name="height" value="195"></constructor-arg>  
</bean>
```

```
    public static void main(String[] args) {
        //IoC容器
        ApplicationContext ctx =new ClassPathXmlApplicationContext("applicationContext.xml");     
        try {
            Thread.sleep(2000);
             //从容器中获取对象
            Person tom1=ctx.getBean("lazytom",Person.class);
            System.out.println("tom1初始化了");
            Thread.sleep(2000);
            Person tom2=ctx.getBean("lazytom",Person.class);
            System.out.println("tom2初始化了");
            System.out.println(tom1==tom2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
```

结果：

```
tom1初始化了
tom2初始化了
false
```
