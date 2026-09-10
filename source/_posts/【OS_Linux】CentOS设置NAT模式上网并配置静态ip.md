---
title: '【OS_Linux】CentOS设置NAT模式上网并配置静态ip'
date: 2019-04-09 07:33:00
categories:
  - OS_Linux
------

在VMWare中安装的本地虚拟机CentOS7操作系统，动态IP地址会经常变化，设置成静态IP地址后，本地局域网可以互相访问，按以下步骤解决这个问题。

## 1、以系统管理员打开VMWare

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_001.png)

## 2、选择虚拟网络编辑器菜单

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_007.png)

## 3、选择VMnet8这一行

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_004.png)

## 4、还原VMnet8的默认设置

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_012.png)

## 5、修改VMnet8的参数

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_009.png)

**注意：**虚拟机子网IP和掩码都可以自己修改，但一般用默认的就好

## 6、NAT设置

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_005.png)

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_006.png)

## 7、保存设置

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_002.png)

## 8、确认虚拟机为NAT上网模式

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_008.png)

## 9、启动虚拟机

## 10、设置CentOS7的静态IP地址

vi命令修改虚拟机网卡配置文件，如/etc/sysconfig/network-scripts/ifcfg-ens33，注意网卡配置文件不一定是ifcfg-ens33，具体通过ls  /etc/sysconfig/network-scripts命令来查看。

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_011.png)

1）修改BOOTPROTO参数，把地址协议改为静态IP方式。

```
BOOTPROTO=static  # dhcp-动态分配，static-静态分配（重要）。
```

2）修改ONBOOT参数，把开机启动选项ONBOOT设置为yes。

```
ONBOOT=yes  # 是否开机引导。
```

3）设置DSN服务器的IP，添加以下内容。

```
DNS1=114.114.114.114  # 第1个DSN服务器的IP地址。
DNS2=192.168.226.2  # 第2个DSN服务器通常配置为网关。
```

4）设置CentOS7的IP地址、子网掩码和网关参数，添加以下内容。

```
IPADDR=192.168.226.128  # IP地址（重要）。
NETMARSK=255.255.255.0  # 子网掩码（重要）。
GATEWAY=192.168.226.2   # 网关（重要）。
```

## 11、重启CentOS7的网络服务

```
systemctl restart network
```

## 12、测试效果

ping一下百度。

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_003.png)

## 13、解决虚拟机能ping通宿主主机，但宿主主机不能ping通虚拟机的问题

宿主主机是通过VMware Network Adapter VMnet8这块网卡来访问虚拟机的，因此要想使主机能ping通虚拟机，必须保证VMware Network Adapter VMnet8与虚拟机在同一网段，即配置VMware Network Adapter VMnet8的网关和掩码与虚拟机相同，ip在同一个网段。

若虚拟机的网络信息如下：

```
IPADDR=192.168.226.128
NETMARSK=255.255.255.0
GATEWAY=192.168.226.2
```

则VMware Network Adapter VMnet8的网络可设置为：

![](/images/OS_Linux/CentOS设置NAT模式上网并配置静态ip_010.png)

参考博文：[Centos7设置静态IP后无法上网的解决方法](https://www.cnblogs.com/wucongzhou/p/12588705.html)
