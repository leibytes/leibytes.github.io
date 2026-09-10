---
title: '【OS_Windows】彻底关闭windows10自动更新'
date: 2021-09-13 03:37:00
categories:
  - OS_Windows
------

### 第一步，禁用windows update服务

Win+r键打开运行后输入“services.msc”唤起windows服务应用，在服务（本地）中找到windows update一项，双击打开，选择禁用。

![](/images/OS_Windows/彻底关闭windows10自动更新_007.png)

 再将“恢复”选项中，将重新启动服务改为“无操作”

![](/images/OS_Windows/彻底关闭windows10自动更新_002.png)

### 第二步，关闭自动更新计划程序

同样调出win+r的运行菜单，输入taskschd.msc,在任务计划程序库中找到Microsoft》windows》windowsupdate，将其下的计划任务项均设为禁用。

![](/images/OS_Windows/彻底关闭windows10自动更新_006.png)

![](/images/OS_Windows/彻底关闭windows10自动更新_004.png)

### 第三步，关闭组策略

Win+R调出运行，再输入gpedit.msc，在其目录下找到计算机配置》管理模板》windows组件》windows更新

![](/images/OS_Windows/彻底关闭windows10自动更新_005.png)

 并在windows更新中找到配置自动更新目录，双击选择已禁用。

![](/images/OS_Windows/彻底关闭windows10自动更新_001.png)

### 第四步，利用清理软件修改升级档缓存（可选步骤）

进行到以上三步后，其实新的更新已经不会被唤醒了，但仍然有可能系统会在已经下载好的安装包中进行升级。这时候我们可以利用电脑各种管家工具，清理工具中的软件进行垃圾清理，删除windows更新缓存，这是比较简单的办法，如果没有相关软件，可以右键打开c盘属性，点击磁盘清理，在选项中选中windows 更新清理即可。

![](/images/OS_Windows/彻底关闭windows10自动更新_003.png)

**总结：**虽然windows自动更新有很多烦人的地方，但还是建议大家隔一段时间手动更新一下，不然系统的重大漏洞可能会导致如上次“wannacry”比特币勒索病毒之类令人欲哭无泪的时间，另外，禁用系统更新还将导致微软商店无法使用哦~禁用之前也请大家三思

参考博文：[彻底关闭windows自动更新](https://baijiahao.baidu.com/s?id=1629413738184879892&wfr=spider&for=pc)
