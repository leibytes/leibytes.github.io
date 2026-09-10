---
title: '【OS_Linux】Centos7 设置定时任务'
date: 2020-12-04 09:26:00
categories:
  - OS_Linux
------

在centos上最常用的定时任务应该就是crontab了，在经过各种度娘之后有三种方式实现定时任务的方法，但是所用到的场景有所不同

```
at 　　　适合仅执行一次就结束的调度命令，需要启动一个后端的atd服务。
crontab　需要启动一个服务crond才行，crond服务通过crontab命令实现
anacron　无法周期性执行，只能以天为周期，但有个特点，在关机状态下未>执行的任务，下次开机时可以补上执行
```

这里也直讲最常用的crontab

## 关于crontab和crond

用到crontab那么就一定要知道crond，crond和crontab是不可分割的，crond我的理解是一个守护进程，或者说这才是我们定时任务的核心，crontab是一个编辑或者说是管理定时任务的工具  
crontab我的理解就是crond服务处于启动状态，这个服务会默认每分钟去检测是否有需要执行的定时任务，所以crond不能守护秒级定时任务。而这个任务的具体内容是通过crontab来管理的，至于具体管理方式后面继续讲

## 安装

一般情况下crond、crontab是安装好的,这里主要提一下万一没有crontab你别想着直接用yum去安装crond或者crontab因为你会发现找不到包，其实定时任务的包是cronie。(这里因为我遇到了所以提一下)

```
yum install -y cronie
```

## 关于crond和crondtab常用命令

### crond常用命令

```
systemctl start   crond         启动服务
systemctl stop    crond         停止服务
systemctl restart crond         重启服务
systemctl reload  crond         重载配置文件
systemctl status  crond         查看状态
```

### crontab常用命令

```
crontab -u         设定某个用户的cron服务
crontab -l        显示crontab文件(显示已设置的定时任务)
crontab -e        编辑crontab文件(编辑定时任务)
crontab -r        删除crontab文件(删除定时任务)
crontab -i        删除crontab文件提醒用户(删除定时任务)
```

## 三种编辑定时任务的方式

#### 定时任务格式

```
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

用户的定时任务分6段,分别是：分，时，日，月，周，命令
第1列表示分钟1～59 每分钟用*或者 */1表示
第2列表示小时1～23（0表示0点）
第3列表示日期1～31
第4列表示月份1～12
第5列标识号星期0～6（0表示星期天）
第6列要运行的命令

*：表示任意时间都，实际上就是“每”的意思。可以代表00-23小时或者00-12每月或者00-59分
-：表示区间，是一个范围，00 17-19 * * * cmd，就是每天17,18,19点的整点执行命令
,：是分割时段，30 3,19,21 * * * cmd，就是每天凌晨3和晚上19,21点的半点时刻执行命令
/n：表示分割，可以看成除法，*/5 * * * * cmd，每隔五分钟执行一次
```

#### 1、直接编辑配置文件

配置文件路径：/etc/crontab  
在配置文件中,原本6个段的配置，我们需要在命令前面再加一个用户段  
即：分 时 日 月 周 用户 命令  
直接添加到最后一行即可

#### 2、使用crontab -e 命令编辑

使用命令编辑相当于就是编辑当前用户的任务，所以不需要加用户字段，这里就是6段

```
* * * * * /home/wsj/scheduledTest.sh
```

#### 3、直接编辑目标文件

进入/var/spool/cron/

如果你执行了以上两种方式，你再进来看一下这个目录下的文件你可能会发现一点问题  
这里的文件都是以用户名来命名的，你再打开你使用crontab -e 添加过任务的用户文件，原来crontab -e编辑的就是这里的文件，所以我们如果需要为哪个用户添加任务，只用编辑这个目录下对应用户的文件即可

#### 4、实例

```
* * * * *                 每1分钟执行一次
15,30,45 * * * *         每小时的第15、30、45分执行
15,30 10-11 * * *         在上午10点到11点的第15和第30分钟执行
* */2 * * *                 每两个小时执行一次
```

#### 5、相关文件

```
#该文件中所列用户不允许使用crontab命令
    /etc/cron.deny

# 该文件中所列用户允许使用crontab命令
    /etc/cron.allow

#所有用户crontab文件存放的目录,以用户名命名
    /var/spool/cron/

# crond日志文件
    /var/log/cron
```

## 个人测试中的问题

1、关于重新加载配置文件

根据网上说的修改配置文件之后需要重新加载配置文件或者重启crond才能生效，测试过程中发现只要修改了配置文件即可生效，无需使用systemctl reload crond加载

2、crond报错  
(CRON) DEATH (can’t lock /var/run/crond.pid, otherpid may be 8138): Resource temporarily unavailable

解决的各种姿势

```
1、杀掉所有crond进程，然后重启crond服务
2、检查你写的配置文件是否有错
```

参考博文：[Centos7 使用crontab 设置定时任务](https://blog.csdn.net/Sun_Army/article/details/106699670?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)
