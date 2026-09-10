---
title: '【OS_Linux】Centos中使用yum安装软件'
date: 2019-04-23 08:56:00
categories:
  - OS_Linux
------

# 1.YUM的简介

Yum（全称为 Yellow dog Updater, Modified）是一个rpm包管理器。它能够从指定的服务器上自动下载RPM包并安装，可以自动处理包之间的依赖性关系，并且一次性安装所有依赖的软件包，无须繁琐地一次次下载、安装。

# 2.YUM的分类

**YUM服务端：**服务端存放着各种rpm软件包，也称之为yum仓库或yum源。yum仓库分为网络yum仓库和本地yum仓库。若为网络yum仓库则需连网才能使用yum命令安装软件，若为本地yum仓库无需联网便可安装软件。

**YUM客户端：**客户端只需完成简单的配置，便可使用yum命令安装软件。yum配置文件包括两种：全局配置文件和仓库配置文件。

# 3.YUM仓库（yum源）的配置

yum仓库配置文件在/etc/yum.repos.d目录下，配置文件中描述着各仓库的基本信息，同一配置文件中可配置多个仓库，但要保证仓库id `[repositoryid]唯一`

![](/images/OS_Linux/Centos中使用yum安装软件_003.png)

![](/images/OS_Linux/Centos中使用yum安装软件_001.png)

## 3.1.yum源的配置

/etc/yum.repos.d/目录下系统提供的仓库配置文件默认都指向官方yum仓库，但是从官方仓库下载软件包比较慢，因此我们需要新建指向非官方仓库的配置文件，并替换原来的CentOS-Base.repo文件。通常先将原来的官方仓库配置文件CentOS-Base.repo备份为CentOS-Base.repo.bak,再进行替换。

**法一：**手动添加仓库配置文件，例如：vim locate.repo

```
[repo id] 　　仓库id，一定要放在[]里（必须要，一般第一个我们默认写成base）
repo name 　　仓库名，可以不要
mirrorlist 　　 镜像站点，可以不要
baseurl 　　　　　yum仓库地址。例如：https://mirrors.aliyun.com/centos/6.9/os/x86_64/
enabled 　　　　如果enabled=1或者不写，此仓库都是生效的，enabled=0仓库无效（默认是1）
gpgcheck 　　　如果是1 则RPM数字证书生效，如果是0不生效
gpgkey 　　 数字证书的公钥文件保存位置，不用管
enablegroups={1|0}开启组
failovermethod={roundrobin|priority}
roundrobin： 意为随机挑选，默认值
priority:仓库的优先级，数值越小优先级越高，未设置priority时默认优先级为 99
cost= 默认为1000
```

**法二：**直接下载仓库配置文件

<http://mirrors.aliyun.com/repo/>

![](/images/OS_Linux/Centos中使用yum安装软件_002.png)

通过wget http://mirrors.aliyun.com/repo/Centos-6.repo命令下载与系统版本对应的仓库配置文件，并替换原来的CentOS-Base.repo文件。

```
wget http://mirrors.aliyun.com/repo/Centos-6.repo
mv CentOS-Base.repo CentOS-Base.repo.bak
mv Centos-6.repo CentOS-Base.repo
```

## **3.2.多个仓库的生效规则**

1、`/etc/yum.repos.d/` 下所有 `.repo` 文件均会被加载，无论文件名如何

2、每个仓库配置通过 `enabled=1`（启用）或 `enabled=0`（禁用）控制是否生效

3、多个仓库文件中定义了相同的​仓库 ID​（如 `[base]`），按照文件名的字母顺序后加载的配置会覆盖先加载的配置

4、当多个仓库提供同一软件包时，YUM 默认选择优先级最高的仓库，即仓库的`priority最小的`

# 4.常用的YUM命令

## 4.1.仓库相关命令

　　1.启用与禁用仓库

　　启用仓库:yum-config-manager --enable "repo id"  
　　禁用仓库:yum-config-manager --disable "repo id "

　　2.显示仓库列表

　　yum repolist

　　3.显示仓库中所有的软件列表

　　yum list

## 4.2.软件的安装卸载与更新

　　安装：yum -y install package1 加上参数y后，所有的依赖均自动安装  
　　重新安装：yum reinstall package  
　　卸载：yum remove package  
　　更新：yum update package  
　　查找是否已安装某个软件包：yum search package  
　　降级：yum downgrage package  
　　检查可用的更新：yum check-update  
　　查询软件包的依赖：yum deplist package1

## 4.3.缓存命令

　　清除缓存：yum clean all  
　　构建缓存：yum makecache

# 5.应用经验

1）rpm安装/升级软件包需要手工的解决包的依赖关系，这一点让人确实很烦，所以，软件包的安装/升级一般采用yum命令。

2）rpm的某些功能，例如查看软件包的详细信息、软件包的安装目录、软件包的配置文件等还是有实用价值的。

3）rpm 只能安装已经下载到本机的rpm 包， yum能在yum仓库中自动下载并安装rpm包

本文参考：[CentOS配置yum仓库的三种方法](https://blog.csdn.net/lpl_lpl_lpl/article/details/85621740)    [yum仓库管理](https://www.cnblogs.com/duzhaoqi/p/6926050.html)
