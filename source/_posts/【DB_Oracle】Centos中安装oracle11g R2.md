---
title: '【DB_Oracle】Centos中安装oracle11g R2'
date: 2020-08-28 10:17:00
categories:
  - DB_Oracle
------

### **安装包:**

* CentOS-7-x86\_64-DVD
* linux.x64\_11gR2\_database\_1of2.zip
* linux.x64\_11gR2\_database\_2of2.zip

### 安装Oracle前准备

1、在桌面单击右键，选择“在终端中打开”，进入终端

输入命令：su

输入ROOT密码：

创建用户组oinstall：groupadd oinstall

创建用户组dba：groupadd dba

创建oracle用户，并加入到oinstall和dba用户组：useradd -g oinstall -g dba -m oracle

设置用户oracle登录密码，需要确认一次，注意两次密码要一样(注意：此处的密码是oracle账户登录密码)：passwd oracle

查看新建的oracle用户：id oracle

2、创建oracle数据库安装目录

oracle数据库安装目录：mkdir -p /data/oracle

oracle数据库配置文件目录：mkdir -p /data/oraInventory

oracle数据库软件包解压目录：mkdir -p /data/database

再输入：cd /data

创建完毕检查一下：ls

设置目录所有者为oinstall用户组的oracle用户：chown -R oracle:oinstall /data/oracle

再输入：chown -R oracle:oinstall /data/oraInventory

再输入：chown -R oracle:oinstall /data/database

![\](https://www.2cto.com/uploadfile/Collfiles/20160729/2016072909205292.png)

3、修改os系统标识

因为oracle默认不支持Centos系统安装，需要修改为对应的RedHat版本。

查看当前系统版本：cat /proc/version

输入：vi /etc/redhat-release

将版本改为：redhat-7

4、关闭防火墙

关闭防火墙：systemctl stop firewalld.service

禁止使用防火墙：systemctl disable firewalld.service

![\](https://www.2cto.com/uploadfile/Collfiles/20160729/2016072909205293.png)

5、关闭selinux（需重启生效）

进入编辑状态：vi /etc/selinux/config

将SELINUX的值改为disabled，保存并退出后重启系统生效。

![\](https://www.2cto.com/uploadfile/Collfiles/20160729/2016072909205294.png)

6、修改内核参数

进入编辑状态：vi /etc/sysctl.conf

在最后添加如下代码：

net.ipv4.icmp\_echo\_ignore\_broadcasts = 1

net.ipv4.conf.all.rp\_filter = 1

fs.file-max = 6815744

fs.aio-max-nr = 1048576

kernel.shmall = 2097152

kernel.shmmax = 2147483648

kernel.shmmni = 4096

kernel.sem = 250 32000 100 128

net.ipv4.ip\_local\_port\_range = 9000 65500

net.core.rmem\_default = 262144

net.core.rmem\_max= 4194304

net.core.wmem\_default= 262144

net.core.wmem\_max= 1048576

保存并退出。

使配置参数立即生效：sysctl -p

![\](https://www.2cto.com/uploadfile/Collfiles/20160729/2016072909205295.png)

7、对oracle用户设置限制，提高软件运行性能

进入编辑状态：vi /etc/security/limits.conf

找到最后一行# End of file，在它上面空行处添加如下代码：

oracle soft nproc 2047

oracle hard nproc 16384

oracle soft nofile 1024

oracle hard nofile 65536

保存并退出。

8、配置用户的环境变量

进入编辑状态：vi /home/oracle/.bash\_profile

在最后添加如下代码：

export ORACLE\_BASE=/data/oracle #oracle数据库安装目录

export ORACLE\_HOME=$ORACLE\_BASE/product/11.2.0/db\_1

export ORACLE\_SID=orcl #oracle启动数据库实例名

export ORACLE\_TERM=xterm

export PATH=$ORACLE\_HOME/bin:/usr/sbin:$PATH

export LD\_LIBRARY\_PATH=$ORACLE\_HOME/lib:/lib:/usr/lib

export LANG=C

export NLS\_LANG=AMERICAN\_AMERICA.ZHS16GBK

保存并退出。

使环境变量配置立即生效：source /home/oracle/.bash\_profile

9、用ftp 软件将Oracle的两个安装包linux.x64\_11gR2\_database\_1of2.zip和linux.x64\_11gR2\_database\_2of2.zip上传到指定的目录（如/usr/local/src），然后将压缩包解压到同一目录

```
[oracle@localhost /]$ cd /usr/local/src　　#进入到压缩包所在的目录
[oracle@localhost src]$ ls
linux.x64_11gR2_database_1of2.zip linux.x64_11gR2_database_2of2.zip
[oracle@localhost src]$ unzip linux.x64_11gR2_database_1of2.zip -d /data/database/　　#解压
(省略...)
[oracle@localhost src]$ unzip linux.x64_11gR2_database_2of2.zip -d /data/database/　　#解压
(省略...)
[oracle@localhost src]$ su root
Password:
[root@localhost src]# chown -R oracle:oinstall /data/database/database/　　
```

自此准备工作完成便可以进行Oracle的安装了

### oracle安装

切换至oracle用户：su - oracle

#### 启动oralce安装

到/data/database/database/目录下，执行./runInstaller

会启动图形界面进行安装

![](/images/DB_Oracle/Centos中安装oracle11g_R2_014.png)

  点击下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_002.png)

  点击下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_005.png)

 实际开发中服务器是无需图形界面的，我们选择服务类安装，下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_012.png)

 不需要集群，选择单实例安装，下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_001.png)

 下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_011.png)

 选择语言，下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_004.png)

这里仅需要选择oracle安装的基目录为我们新建的/data/oracle即可，其它目录会跟着变动

![](/images/DB_Oracle/Centos中安装oracle11g_R2_018.png)

 下一步

![](/images/DB_Oracle/Centos中安装oracle11g_R2_009.png)

配置全局实例名

![](/images/DB_Oracle/Centos中安装oracle11g_R2_016.png)

 内存默认就好

![](/images/DB_Oracle/Centos中安装oracle11g_R2_003.png)

**设置Oracle服务端字符编码，此步至关重要（一般设置为：**AMERICAN\_AMERICA.ZHS16GBK**）**

![](/images/DB_Oracle/Centos中安装oracle11g_R2_019.png)

 这里选择不启用备份

![](/images/DB_Oracle/Centos中安装oracle11g_R2_008.png)

 设置特殊用户的密码

![](/images/DB_Oracle/Centos中安装oracle11g_R2_010.png)

  先决条件检查，缺少依赖包（直接忽略 进行安装）

![](/images/DB_Oracle/Centos中安装oracle11g_R2_013.png)

  待安装完成，会让你以root身份登录终端执行以下脚本，目的是配置监听和本地网络服务。

```
[root@centos7 oraInventory]# su root
[root@centos7 oraInventory]# cd /data/oraInventory                #oraInventory目录由自己的安装路径来决定
[root@centos7 oraInventory]# sh orainstRoot.sh
[root@centos7 oraInventory]# cd /data/oracle/product/11.2.0/db_1  #db_1目录由自己的安装路径来决定   
[root@centos7 db_1]# sh root.sh
```

#### 设置Oracle的监听和实例随 Linux开机自启

第一步:修改/etc/oratab文件

```
[oracle@localhost ~]$ vim /etc/oratab    //如果没有这个文件，则是因为oracle安装完成的时候忘记执行root.sh文件，找到root.sh执行后就会出现oratab这个文件
```

第二步:把lsnrctl start和dbstart添加到rc.local文件中: 

 root权限执行

```
[root@localhost oracle]# vim /etc/rc.d/rc.local
```

添加：

```
su - oracle -lc "/data/oracle/product/11.2.0/db_1/bin/lsnrctl start"
su - oracle -lc "/data/oracle/product/11.2.0/db_1/bin/dbstart"
```

 说明：第一行为开机启动数据库监听，第二行为开机启动数据库实例。(路径跟安装路径相关)。

注意：CentOs7中/etc/rc.d/rc.local不会开机执行，因此需添加执行权限。

查看/etc/rc.d/rc.local的权限

```
[root@localhost oracle]# ll /etc/rc.d/rc.local
```

 添加执行权限

```
[root@localhost oracle]# chmod +x /etc/rc.d/rc.local
```

 此致Oracle监听和服务开机自启动设置完成。

### 安装完成后可能出现的问题

**问题一：**如果使用Navicat Premium软件远程连接不了Centos的Oracle数据库出现：“ORA-12514 TNS 监听程序当前无法识别连接描述符中请求服务”。

解决方案如下

一、修改数据库服务器中listener.ora文件内容

[oracle@localhost admin]$ vi /data/oracle/product/11.2.0/db\_1/network/admin/listener.ora

修改为：

```
# listener.ora Network Configuration File: /data/oracle/product/11.2.0/db_1/network/admin/listener.ora
# Generated by Oracle configuration tools.
 
 
SID_LIST_LISTENER =
  (SID_LIST =
      (SID_DESC =
         (GLOBAL_DBNAME = orcl)
         (ORACLE_HOME = /data/oracle/product/11.2.0/db_1)
         (SID_NAME = orcl)
      )
   )
LISTENER =
    (DESCRIPTION =
         (ADDRESS =
             (PROTOCOL = TCP)
                (HOST = 192.168.0.102) #这里为centos的ip地址
                (PORT = 1521)
         )
     )
 
ADR_BASE_LISTENER = /data/oracle
```

二、修改数据库服务器中tnsnames.ora文件内容

命令:

[oracle@localhost admin]$ vi /data/oracle/product/11.2.0/db\_1/network/admin/tnsnames.ora

修改为：

```
# tnsnames.ora Network Configuration File: /data/oracle/product/11.2.0/db_1/network/admin/tnsnames.ora
# Generated by Oracle configuration tools.
orcl =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.0.102)(PORT = 1521))  #这里的host为centos的ip地址
    )
    (CONNECT_DATA =
      (SID = orcl)
    )
  )
```

修改完成，lsnrctl start命令重启监听。

使用Navicat Premium远程连接，测试连接是否成功

![](/images/DB_Oracle/Centos中安装oracle11g_R2_006.png)

**问题二：**远程连接oracle数据库出现

```
ERROR:
ORA-01034: ORACLE not available
ORA-27101: shared memory realm does not exist
Process ID: 0
Session ID: 0 Serial number: 0
```

解决方案

```
SQL> conn / as sysdba
Connected to an idle instance.
SQL> startup
ORACLE instance started.
```

 参考博文：

1、[Centos安装Oracle及问题处理](https://www.cnblogs.com/pxblog/p/10545073.html)

2、[centos7下oracle11g详细的安装与建表操作](https://www.cnblogs.com/guzhanyu/p/7943728.html)

3、[centos7下安装oracle11gR2的详细步骤](https://www.cnblogs.com/activiti/p/7575133.html)

###
