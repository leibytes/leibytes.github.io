---
title: '【OS_Linux】Centos中目录及文件管理'
date: 2019-04-11 03:06:00
categories:
  - OS_Linux
------

## 1.Linux系统中目录的树状结构

![](/images/OS_Linux/Centos中目录及文件管理_002.jpg)

|  |  |
| --- | --- |
| 目录 |  |
| /bin | 存放二进制可执行文件(ls,cat,mkdir等)，常用命令一般都在这里。 |
| /etc | 存放系统管理和配置文件 |
| /home | 存放所有用户文件的根目录，是用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示 |
| /usr | 用于存放系统应用程序，比较重要的目录/usr/local 本地系统管理员软件安装目录（安装系统级的应用）。这是最庞大的目录，要用到的应用程序和文件几乎都在这个目录。  /usr/x11r6 存放x window的目录  /usr/bin 众多的应用程序    /usr/sbin 超级用户的一些管理程序    /usr/doc [**Linux**](http://lib.csdn.net/base/linux "Linux知识库")文档    /usr/include linux下开发和编译应用程序所需要的头文件    /usr/lib 常用的动态链接库和软件包的配置文件    /usr/man 帮助文档    /usr/src 源代码，linux内核的源代码就放在/usr/src/linux里    /usr/local/bin 本地增加的命令    /usr/local/lib 本地增加的库 |
| /opt | 额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把tomcat等都安装到这里。 |
| /proc | 虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息。 |
| /root | 超级用户（系统管理员）的主目录（特权阶级^o^） |
| /sbin | 存放二进制可执行文件，只有root才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如ifconfig等。 |
| /dev | 用于存放设备文件。 |
| /mnt | 系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统。 |
| /boot | 存放用于系统引导时使用的各种文件 |
| /lib | 存放跟文件系统中的程序运行所需要的共享库及内核模块。共享库又叫动态链接共享库，作用类似windows里的.dll文件，存放了根文件系统程序运行所需的共享文件。 |
| /tmp | 用于存放各种临时文件，是公用的临时文件存储点。 |
| /var | 用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等。）等。 |
| /lost+found | 这个目录平时是空的，系统非正常关机而留下"无家可归"的文件（windows下叫什么.chk）就在这里 |

 重要目录的解释：

**/bin**：bin是Binary的缩写, 这个目录存放着最常使用的命令，诸如mkdir、cat命令。

**/etc：etc**这个目录用来存放所有系统管理的配置目录和配置文件。

**/home**：用于存放用户主目录的，在Linux中，每个用户都有一个以自身账号命名的目录，我们称其为用户主目录，用户主目录一般存放在**/home**下。

【注】

⑴windows与linux文件路径的区别在于目录的分隔符，Windows目前用正反斜杠（/和\）均可以，Linux只能用正斜杠（/）。

⑵因为web程序开始于Unix系统，所以URL也用 / 做路径分割符，后沿袭做运算符/除法

## 2.目录的增、删、改、查操作

### mkdir (创建新目录)

```
语法：mkdir [-mp] 目录名称
选项与参数：
-m ：mode创建目录的同时指明目录的权限
-p ：录递归的创建上层目
实例：
[root@www ~]# cd /tmp
[root@www tmp]# mkdir –m 711 test1/test2/test3/test4
mkdir: cannot create directory `test1/test2/test3/test4':
No such file or directory <== 因找不到上层目录而创建失败
[root@www tmp]# mkdir –m 711 -p test1/test2/test3/test4
加了参数-p后就可以递归的创建上层目录
```

### rmdir (删除空的目录)

```
语法：rmdir [-p] 目录名称
选项与参数：
-p ：连同上一级『空的』目录也一起删除，仅当前目录删除后其父目录为空时父目录才会被删除
实例：
[root@www tmp]# rmdir test <==可直接删除，没问题
[root@www tmp]# rmdir test1 <==因为尚有内容，所以无法删除！
【注意】  
⑴加了参数 -p，仅当前目录删除后其父目录为空时父目录才会被删除。
⑵rmdir 仅能删除空目录，用 rm –rf 目录 来删除非空目录。
```

### cp (复制目录或文件)

```
语法:
cp [-adfilprsu] 来源档(source) 目标档(destination)
cp [options] source1 source2 source3 .... directory
选项与参数：
-i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
-p：连同文件的属性一起复制过去，而非使用默认属性(备份时常用)；
-r：递归的复制，用于目录的复制行为(常用)
实例：
[root@www ~]# cp ~/.bashrc /tmp/bashrc
[root@www ~]# cp -i ~/.bashrc /tmp/bashrc
cp: overwrite `/tmp/bashrc'? n <==n不覆盖，y为覆盖
```

### mv (移动文件与目录，或修改名称)

```
语法：
mv [-fiu] source destination
mv [options] source1 source2 source3 .... directory
选项与参数：
-f ：force 强制移动的意思，如果目标文件已经存在，不会询问而直接覆盖；
-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
实例：
[root@www tmp]# cp ~/.bashrc bashrc
[root@www tmp]# mkdir mvtest
[root@www tmp]# mv bashrc mvtest <==将目录bashrc移动到目录 mvtest
[root@www tmp]# mv mvtest mvtest2 <==将目录mvtest重命名为mvtest2
【注】mv命令中若目标已存在则表示移动，若目标不存在则表示重命名
```

### ls(列出某目录中的内容)

```
语法：
ls [options] 目录名称
选项与参数：
-a ：列出包括隐藏文件在内的全部文件，
-l ：列出文件的属性与权限等数据(常用)
实例：
[root@localhost ~]# ls -l ~ #查看root主目录中各文件或目录的属性
```

![](/images/OS_Linux/Centos中目录及文件管理_005.png)

【注意】⑴目录的增删操作中参数-p表示向上递归，而在拷贝和移动中参数-r表示向下递归，参数-f和-i分别表示强制和交互式操作。

## 3. 文件的增、删、改、查操作

### touch(新增文件)

```
语法： touch 文件名
功能描述：当参数为已存在的文件，则表示更新该文件的创建时间为当前时间。
若参数为不存在的文件则表示新建空的文件。
实例：
①更新文件的创建时间：
[root@mail test]# ll
总用量 4
-rw-r--r-- 1 root root 12 3月 12 13:43 1.txt
[root@mail test]# touch 1.txt
[root@mail test]# ll
总用量 4
-rw-r--r-- 1 root root 12 3月 12 13:58 1.txt #更新到系统当前时间
②创建空的文件：
[root@mail test]# ll
-rw-r--r-- 1 root root 12 3月 12 13:58 1.txt
[root@mail test]# touch 2.txt
[root@mail test]# ll
-rw-r--r-- 1 root root 12 3月 12 13:58 1.txt
-rw-r--r-- 1 root root 0 3月 12 14:06 2.txt #文件大小为0所以是个空文件
```

### rm (删除文件或目录)

```
语法：rm [options] 文件或目录
选项与参数：
-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
-i ：交互式删除，在删除前会询问使用者是否动作
-r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！
实例：
[root@www tmp]# rm -i bashrc
rm: remove regular file `bashrc'? y
```

【注】rm 命令不仅可删除文件，还可以用来删除非空目录

### cat（查看文件内容）

cat命令可用于查看文件内容、创建新的文件、将多个文件合并成一个文件，它常与重定向符号(">")配合使用，具体语法如下：

* 查看文件内容:cat filename
* 新建一个名为filename的文件:cat >filename 只能创建新文件,不能编辑已有文件，这是与vi命令的区别
* 将多个文件合并为一个文件:cat file1 file2 > file

【注】tac与cat命令刚好相反，文件内容从最后一行开始显示，可以看出 tac 是 cat 的倒着写。

## 4.文件或目录的属性管理

可以使用ll或者ls –l命令来显示一个目录中所有的文件或目录的属性。

示例：[root@localhost ~]# ls -l ~     查看root主目录中各文件或目录的属性

![](/images/OS_Linux/Centos中目录及文件管理_006.png)

⑴开始的total表示该目录的大小（以KB为单位）

⑵各文件或目录七个属性段的含义：

    第1段表示文件类型及读、写、执行权限

    第2段表示文件硬链接数（同快捷方式）或子目录数（类型为目录的前提下）

    第3段表示该文件的属主

    第4段表示该文件的属组

    第5段表示该文件的大小（以byte字节为单位）

    第6段表示该文件的创建时间（以月-日-年的格式表示）

    第7段表示该文件的文件名

### 4.1 chown（更改文件属主）

```
语法：
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
【注】chown可同时更改属主和属组，参数R表示递归更改
```

### 4.2 chgrp（更改文件属组）

```
语法：chgrp [-R] 属组名 文件名
```

### 4.3 chmod（更改文件的读、写以及执行权限）

Linux文件的权限有两种更改方法：一种是通过数字，一种是通过符号。文件的基本权限有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限，各权限的权值对照表如下：

r:4

w:2

x:1

这相当于将rwx视作3位二进制数，有相应权限该位为1，无权限该位为0。

法一、通过数字更改权限：

```
[root@www ~]# ls -al .bashrc
-rw-r--r-- 1 root root 395 Jul 4 11:45 .bashrc
[root@www ~]# chmod 777 .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx 1 root root 395 Jul 4 11:45 .bashrc
```

法二、通过符号更改权限

改变文件权限就是改变(1)user (2)group (3)others这三个身份上的读、写及执行权限，我们可以由u, g, o来分别代表这三种身份，此外可用a (all)代表全部身份。那么改变权限的另一种方式如下：

![](/images/OS_Linux/Centos中目录及文件管理_001.png)

```
创建文件并将文件权限设置为 -rwxr-xr-- ，可以使用 chmod u=rwx,g=rx,o=r 文件名 来设定:
# touch test1 // 创建 test1 文件
# ls -al test1 // 查看 test1 默认权限
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
# chmod u=rwx,g=rx,o=r test1 // 修改 test1 权限
# ls -al test1
-rwxr-xr-- 1 root root 0 Nov 15 10:32 test1
拿掉所有人的可执行权限，其它权限不变
# chmod a-x test1
# ls -al test1
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
```

【注】⑴第一种数字权限表示法无需指明身份，第二种需指明身份的简写。

有关文件和目录的属性设置可参考博文：[Linux 文件属性](https://www.cnblogs.com/kaituorensheng/archive/2013/03/26/2983573.html)

## 5.文件的特殊操作

### **5.1 find查找命令**

Linux下find命令提供相当多的查找条件，因此功能比较强大，可以在众多文件或目录下查找你想要的任何文件或目录。

**find命令语法格式：**find  [查找路径]  [查找条件]  [针对查找到的结果逐个处理]

**查找路径**：指在那个目录下查找，默认为在当前目录；

**查找条件**：即为搜索条件，可取值如下：

按文件类型（-type），文件名（-name），文件属主（-user），文件所属组）（-group），文件权限（-perm），文件长度（-size），文件修改时间（-mtime）等查找。

文件类型包括：d  目录文件。

l  符号链接(指向另一个文件,类似于瘟下的快捷方式) 。   
                    　 s  套接字文件。  
                    　 b  块设备文件,二进制文件。  
                   　  c  字符设备文件。  
                         p  命名管道文件。  
                         -  普通文件，或更准确地说，不属于以上几种类型的文件。

文件权限包括：r    读文件

w   写文件

x    执行文件操作

示例：

（1）按文件类型查找：find test1 -type d

![](/images/OS_Linux/Centos中目录及文件管理_003.png)

(2)按文件名查找：find -name test.sh

![](/images/OS_Linux/Centos中目录及文件管理_004.png)

**针对查找到的结果逐个处理：**

-print： 打印查找到的文件名，此为默认的处理  
-exec： 对查找的结果逐个执行某种shell命令。命令的形式为command { }  \;，其中“{ }  \;”是固定写法，变化的仅仅是command，且需注意他们之间的空格。

示例：

```
#在当前目录下查找名为test.sh的文件，首先打印出文件名，再输出文件的内容
[root@localhost bash]# find -name test.sh -print -exec cat {} \;
./test.sh
#!/bin/bash
echo "Hello World !"
#只输出文件的内容，不打印文件名
[root@localhost bash]# find -name test.sh -exec cat {} \;
#!/bin/bash
echo "Hello World !"
```

-ok：和-exec的作用相同，只不过是以一种更为安全的模式来对查询结果执行某种shell命令。在执行每一个命令之前，都会给出提示，让用户来确定是否执行。

```
[root@localhost bash]# find -name test.sh -ok cat {} \;
< cat ... ./test.sh > ? y
#!/bin/bash
echo "Hello World !"
[root@localhost bash]# find -name test.sh -ok cat {} \;
< cat ... ./test.sh > ? n
[root@localhost bash]#
```
