---
title: '【OS_Windows】Windows设置多用户同时远程登录'
date: 2020-08-04 09:14:00
categories:
  - OS_Windows
------

1、按下Win+R键(也可以在开始菜单右键然后选择运行)，在运行的输入框里面输入"gpedit.msc"命令、然后点击确定，打开本地组策略编辑器

![](/images/OS_Windows/Windows设置多用户同时远程登录_004.png)

 2、在“计算机组策略”中依次展开 计算机配置-->管理模板--->windows组件--->然后在右边的菜单中选择远程桌面服务；双击打开

![](/images/OS_Windows/Windows设置多用户同时远程登录_008.png)

  3、选择“远程桌面会话主机”，双击打开，如下图所示

![](/images/OS_Windows/Windows设置多用户同时远程登录_001.png)

  4、打开“远程桌面会话主机”界面后，双击打开“连接”。

![](/images/OS_Windows/Windows设置多用户同时远程登录_003.png)

  5、打开后，选中“将远程桌面服务用户限制到单独的远程桌面服务会话”，双击打开。

![](/images/OS_Windows/Windows设置多用户同时远程登录_002.png)

  6、打开以后，勾上“已禁用”，然后点击确认后，关闭。

![](/images/OS_Windows/Windows设置多用户同时远程登录_007.png)

  7、回到“连接”界面，选中“限制连接的数量”，双击打开。

![](/images/OS_Windows/Windows设置多用户同时远程登录_005.png)

 8、在“允许的RD最大连接数”中输入你希望同时远程连接的数量，输入后勾上“已启用”后确认。

![](/images/OS_Windows/Windows设置多用户同时远程登录_006.png)

 参考文章：[Windows设置多用户同时远程登录](https://www.cnblogs.com/chuhongyun/p/11400970.html)
