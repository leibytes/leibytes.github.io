---
title: '【DB_Oracle】Java连接Oracle数据库的两种常用方法'
date: 2020-04-14 14:16:00
categories:
  - DB_Oracle
------

jdbc连接上oracle有两种方法：

**1、****使用thin****连接**

由于thin驱动都是纯Java代码，并且使用TCP/IP技术通过java的Socket连接上Oracle数据库，所以thin驱动是与平台无关的，你无需安装Oracle客户端，只需要下载一个thin驱动的jar包，并且将环境变量中的CLASS\_PATH变量中加入thin驱动的路径就可以了。Thin驱动虽然与平台无关，也无需安装Oracle客户端，但是有一个致命的缺陷就是性能一般，达不到如OCI方式的企业级的要求。另外，如果一个oracle数据库对应一台主机，可以使用thin连接；如果一个oracle数据库对应四五台主机（集群服务器），使用thin时，需要把tnsnames.ora文件中的相关数据库的整个连接字符串都拷贝下来，如此才能连接上oracle集群数据库。而这种方法也是和数位同事讨论并Google了大量资料后才发现的。之前我一直以为thin不支持集群数据库的方式。

然后就是设置连接字符串了。这个是固定的写法，如下所示：

```
Class.forName("oracle.jdbc.driver.OracleDriver");
Connection conn=DriverManager.getConnection("jdbc:oracle:thin:@10.87.134.107:1521:ora9","sms","zzsms");
```

**注意：**

**（1）使用thin连接时，若不是集群数据库就在@后直接输入ip:数据库名（或者SERVICE\_NAME）**

**![](/images/DB_Oracle/Java连接Oracle数据库的两种常用方法_002.png)**

**（2）如果是集群数据库，则需要在@后输入tnsnames.ora文件中有关该数据库的所有描述内容（通常是=后边的所有的内容），如下图所示。**

![](/images/DB_Oracle/Java连接Oracle数据库的两种常用方法_001.png)

**2、****使用oci****连接(Oracle Call Interface)**

使用OCI连接数据库是企业级的做法，适应于单个数据库和集群数据库，性能优越，尤其是连接池功能大大提高了应用程序的性能和并发量。唯一的缺点是，若想使用OCI必须要安装Oracle客户端。

安装完Oracle客户端后，里面有个jdbc的文件夹，该文件夹下就包含了OCI驱动和THIN驱动。所以是不需要去网上下载的。这个驱动在jdbc/lib文件夹下，主要有classes12.jar、nls\_charset12.jar等等。其中以12结尾的驱动包适应于jdk1.1以上的版本。以11结尾的适应于jdk1.1以下的版本。文件名中含有classes的jar包就是驱动程序，文件名中含有nls的jar包是与国际化有关的类。

找到文件后，就需要把class和nls的jar包的绝对路径加入CLASS\_PATH环境变量，否则会报ClassNotFound的异常。

设置环境变量后，就可以直接使用OCI驱动了，不要被“驱动”两个字吓坏了，（驱动就是一个可执行文件和一个连接字符串）。很简单，无非是连接字符串的问题。标准的连接字符串如下所示：

```
Class.forName("oracle.jdbc.driver.OracleDriver");
Connection con = DriverManager.getConnection("jdbc:oracle:oci:@xxzc","duansiyuan","oracle_password");
```

只要这两行，就可以保证连接到数据库，而这个数据库不管是单个数据库还是集群数据库。其中xxzc就是数据库名，duansiyuan就是用户名，oralce\_password就是密码。这种方式和C#中的方式比较类似。

需要注意的问题：

1、oracle\_home/jdbc文件夹下有大量的样板代码和帮助文档，里面对如何安装和使用jdbc都有详细的说明，请引起高度重视并耐心阅读。里面有很多知识仅通过自己反复的实践是无法获取的。

2、如果想要高性能，请使用OCI连接，如果不想装Oracle客户端，请使用thin连接。

参考博文：[Java连接oracle数据库的两种常用方法](https://www.cnblogs.com/xiohao/p/3510980.html)
