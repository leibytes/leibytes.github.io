---
title: '【Java_基础】java类加载过程与双亲委派机制'
date: 2019-02-27 13:43:00
categories:
  - Java_基础
------

# 1.类的加载、连接和初始化

当程序使用某个类时，如果该类还未被加载到内存中，则系统会通过加载、连接、初始化三个步骤来对类进行初始化。如果没有意外，jvm将会连续完成这三个步骤，有时也把这三个步骤统称为类的加载和初始化

![](/images/Java_基础/java类加载过程与双亲委派机制_001.png)

## 1.1 类的加载

类加载器根据类的全名以二进制流的形式读取class文件到JVM内存的方法区中，然后转换为一个与目标类对应的java.lang.Class对象实例。

## 1.2 类的连接

链接阶段要做的是将加载到JVM中的二进制字节流的类数据信息合并到JVM的运行时状态中，经由验证、准备和解析三个阶段。

1）、验证

验证类数据信息是否符合JVM规范，是否是一个有效的字节码文件，验证内容涵盖了类数据信息的格式验证、语义分析、操作验证等。

格式验证：验证是否符合class文件规范

语义验证：检查一个被标记为final的类型是否包含子类；检查一个类中的final方法视频被子类进行重写；确保父类和子类之间没有不兼容的一些方法声明（比如方法签名相同，但方法的返回值不同）

操作验证：在操作数栈中的数据必须进行正确的操作，对常量池中的各种符号引用执行验证（通常在解析阶段执行，检查是否通过富豪引用中描述的全限定名定位到指定类型上，以及类成员信息的访问修饰符是否允许访问等）

2）、准备  为静态变量(即static修饰的变量)分配内存到方法区中，并赋为默认值(例如static int i=5;准备阶段会将i的值设为0而不是5，赋值为5的操作是在初始化阶段完成)。注意，这里不包括对常量的操作，常量在编译阶段就已经初始化了；同理也不包括对实例变量的操作，实例变量将会在对象实例化时随着对象一起分配在Java堆中。

3）、解析  主要将常量池中的符号引用替换为直接引用的过程。符号引用就是一组符号来描述目标，可以是任何字面量，而直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄。有类或接口的解析，字段解析，类方法解析，接口方法解析(这里涉及到字节码变量的引用，如需更详细了解，可参考《深入Java虚拟机》)。

## 1.3 类的初始化

类的初始化是类加载过程的最后一步，在整个类加载过程，除了在加载阶段可由自定义类加载器参与外，其余动作完全由虚拟机主导和控制。到了初始化阶段，才真正开始执行类中定义的Java代码。

初始化阶段是执行类构造器<clinit>()方法的过程。<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块(static{}块)中的语句合并产生的。

```
class SingleTon {
    private static SingleTon singleTon = new SingleTon();
    public static int count1;
    public static int count2 = 0;
 
    private SingleTon() {
        count1++;
        count2++;
    }
 
    public static SingleTon getInstance() {
        return singleTon;
    }
}
 
public class Test {
    public static void main(String[] args) {
        SingleTon singleTon = SingleTon.getInstance();
        System.out.println("count1=" + singleTon.count1);
        System.out.println("count2=" + singleTon.count2);
    }
}
```

分析:

1、SingleTon singleTon = SingleTon.getInstance();调用了类的SingleTon调用了类的静态方法，触发类的初始化  
2、类加载的时候在准备过程中为类的静态变量分配内存并初始化默认值 singleton=null count1=0,count2=0  
3、类初始化化，为类的静态变量赋值和执行静态代码快。singleton赋值为new SingleTon()调用类的构造方法  
4、调用类的构造方法后count=1;count2=1  
5、继续为count1与count2赋值,此时count1没有赋值操作,所有count1为1,但是count2执行赋值操作就变为0

### 1.3.1 类加载过程中成员变量的初始化顺序

1.父类静态成员和静态初始化快，按在代码中出现的顺序依次执行。

2.子类静态成员和静态初始化块，按在代码中出现的顺序依次执行。

3. 父类的实例成员和实例初始化块，按在代码中出现的顺序依次执行。

4.执行父类的构造方法。

5.子类实例成员和实例初始化块，按在代码中出现的顺序依次执行。

6.执行子类的构造方法。

最后，生成对象由main线程调用

初始化顺序可简记为：父静态->子静态->父实例变量->父构造–>子实例变量->子构造

# 2.类加载器及双亲委派机制

## 2.1 类加载器

java程序中的 .java文件编译完会生成 .class文件，而 .class文件就是通过类加载器ClassLoader加载的，而ClassLoder在加载过程中会使用“双亲委派机制”来加载 .class文件，先上图：

![](/images/Java_基础/java类加载过程与双亲委派机制_003.png)

1. BootStrapClassLoader：启动类加载器，该ClassLoader由jvm在启动时创建，负责加载 $JAVA\_HOME/jre/lib/rt.jar包下的核心类。由于启动类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的对象实例，所以不能直接使用BootStrapClassLoader类。
2. ExtClassLoader：扩展类加载器，该ClassLoader是在sun.misc.Launcher里作为一个内部类ExtClassLoader定义的（即 sun.misc.Launcher$ExtClassLoader），负责加载 $JAVA\_HOME/jre/lib/ext/\*.jar包下的扩展类。
3. AppClassLoader：应用类加载器，该ClassLoader同样是在sun.misc.Launcher里作为一个内部类AppClassLoader定义的，AppClassLoader会加载java环境变量CLASSPATH所指定路径下的类，而CLASSPATH所指定的路径可以通过System.getProperty("java.class.path")获取。
4. CustomClassLoader：自定义类加载器，该ClassLoader是指我们自定义的ClassLoader，比如tomcat的StandardClassLoader属于这一类；当然，大部分情况下使用AppClassLoader就足够了。

**注意：**除了启动类加载器外，所有的类加载器都是ClassLoader的子类，原因在于启动类加载器是由C语言而不是Java实现的。ClassLoader中有两个重要的方法： loadClass(String name,boolean resolve)和findClass(String name)， loadClass方法实现双亲委派机制子一般不进行重写，各子类加载器通过重写findClass方法实现自己的类加载业务。

## 2.2 双亲委派机制

双亲委派的原理：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委托给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中。  
只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需要加载的类）时，子加载器才会尝试自己去加载。这种从下往上委托，再从上向下加载的过程叫作双亲委派机制。

双亲委派的意义：Java类随着类加载器具备了一种带有优先级的层次关系。例如类java.lang.Object，它存放在rt.jar之中，无论哪一个类加载器要加载这个类，最终都是委托给处于模型最顶端的启动类加载器进行加载，因此Object类在程序的各种加载器环境中都是同一个类。

相反，如果没有使用双亲委托模型，直接由接到加载请求的类加载器去加载，如果用户自己编写了一个称为java.lang.Object的类，并放在程序的ClassPath中，那系统中将会出现多个不同的Object类，Java类型体系中最基础的行为也就无法保证，应用程序也将会变得一片混乱。有了双亲委派机制，如果自己去编写一个rt.jar类库中同名的类，它将只能被编译，永远无法被加载。

### 2.2.1  双亲委派机制的实现

双亲委派由顶层父类加载器ClassLoader的loadClass(String name,boolean resolve)方法实现，其内部包含如下两个操作：

a.调用findLoadedClass(String name)检查当前类是否已被加载，若已被加载则直接返回该类的class对象；

b.调用当前类加载器的父类加载器loadClass方法，若父类加载器为空则调用启动类加载器进行加载，当启动类加载器反馈自己无法完成这个加载请求时，在由上至下调用各级类加载器的findClass(String name)方法进行类加载。

```
protected synchronized Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
    // First, check if the class has already been loaded
    Class c = findLoadedClass(name);
    if (c == null) {
        try {
            if (parent != null) {
                c = parent.loadClass(name, false);
            } else {
                c = findBootstrapClassOrNull(name);
            }
        } catch (ClassNotFoundException e) {
            // ClassNotFoundException thrown if class not found
            // from the non-null parent class loader
        }
        if (c == null) {
            // If still not found, then invoke findClass in order
            // to find the class.
            c = findClass(name);
        }
    }
    if (resolve) {
        resolveClass(c);
    }
    return c;
}
```

注意：布尔型参数resolve表示是否在加载过程中便解析该类。

## 2.3 自定义类加载器

JVM中，除了启动类加载器之外所有的类加载器都是ClassLoader的子类，开发者可以通过扩展ClassLoader，并重写protected Class<?> findClass(String name)方法实现自定义类加载器。

下面的自定义类加载器包含对目标类的编译操作：

![](/images/Java_基础/java类加载过程与双亲委派机制_002.gif)![](/images/Java_基础/java类加载过程与双亲委派机制_004.gif)

```
public class CompileAndClassLoader extends ClassLoader {
    //step1.编译java源代码
    private boolean compile(String javaFile) throws IOException{
        System.out.println("CompileAndClassLoader：正在编译"+javaFile+"...");
        //调用系统的javac命令
        Process p=Runtime.getRuntime().exec("javac "+javaFile);
        
        try {
            p.waitFor();//让其他线程等待这个执行完毕
        } catch (InterruptedException e) {
            System.out.println(e);
        }
        
        //获取线程的退出值
        int res=p.exitValue();
        //判断编译是否成功
        return res==0;
    }  
  //step2.以二进制流的形式读取字节码文件到JVM内存方法区  
  private byte[] getBytes(String fileName) throws IOException{
        File file=new File(fileName);
        long length=file.length();
        byte[] raw=new byte[(int)length];
        FileInputStream fin=null;
        fin=new FileInputStream(file);
        int r=fin.read(raw);
        if(r!=length){
            throw new IOException("无法读取全部文件");
        }
        fin.close();
        return raw;
        
    }
    
    //step3.重写findClass方法进行类加载
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        Class clazz=null;
        //将包路径中的点(.)替换为斜线(/)
        String fileStub=name.replace(".", "/");
        String javaFileName=fileStub+".java";
        String classFileName=fileStub+".class";
        File javaFile=new File(javaFileName);
        File classFile=new File(classFileName);
        //当java源文件存在，且class文件不存在，或者Java源文件的修改时间比Class文件的修改时间晚，则重新编译
        if(javaFile.exists() && (!classFile.exists() || javaFile.lastModified()>classFile.lastModified())){
            try{
                if(!compile(javaFileName) || !classFile.exists()){
                    throw new ClassNotFoundException("ClassNotFoundException:"+javaFileName);
                }
            }catch(Exception e){
                e.printStackTrace();
            }
        }
        //如果class文件存在，系统负责将该文件转化为class文件
        if(classFile.exists()){
            try{
                //将class文件的二进制数据读入数组
                byte[] raw=getBytes(classFileName);
                //调用ClassLoader的defineClass方法将二进制数据转化为Class对象
                clazz=defineClass(name, raw, 0,raw.length);
            }catch(Exception e){
                e.printStackTrace();
            }
        }
        if(clazz==null){
            throw new ClassNotFoundException(name);
        }
        return clazz;
    }

    public static void main(String[] args)  throws Exception{
        if(args.length<1){
            System.out.println("缺少目标类，请按如下格式运行java源文件");
            System.out.println("java CompileAndClassLoader ClassName");
            return;
        }
        //第一个参数是需要待加载的类全名
        String progClass=args[0];
        //剩下的参数作为运行目标类的参数
        String[] progArgs=new String[args.length-1];
        System.arraycopy(args, 1, progArgs, 0, args.length-1);
        
        CompileAndClassLoader ccl=new CompileAndClassLoader();
        //遵循双亲委派机制先调用loadClass方法进行委派
        Class<?> clazz=ccl.loadClass(progClass);
        Method main=clazz.getMethod("main", (new String[0]).getClass());
        Object progArr=progArgs;
        main.invoke(main, progArr);
    }
}
```

View Code

接下来提供一个简单的主类：

```
public class Hello
{
    public static void main(String[] args){
        System.out.println("hello");
    }
}
```

可以无需编译Hello.java，直接使用如下命令运行：

```
java CompileAndClassLoader 包名.Hello
```

本文参考了如下几篇博文：[【java】解析java类加载与反射机制](https://www.cnblogs.com/HDK2016/p/6863173.html)    [Java类加载过程](https://www.cnblogs.com/xiaoxian1369/p/5498817.html)     [java类加载-ClassLoader双亲委派机制](https://www.cnblogs.com/benwu/articles/7269392.html)
