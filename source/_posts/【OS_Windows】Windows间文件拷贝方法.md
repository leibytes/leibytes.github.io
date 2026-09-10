---
title: '【OS_Windows】Windows间文件拷贝方法'
date: 2021-01-05 02:01:00
categories:
  - OS_Windows
------

针对重要的文件有时候需要异地备份，即将一个Windows主机上的文件定时的同步到另一个Windows主机。

```
set USER=administrator
set PASSWORD=  
set Server1_IP=192.182.1.211
set remoteDataDir=OracleDataBackup\jcgm
if not exist "D:\OracleDataBackup\jcgm"       mkdir D:\OracleDataBackup\jcgm
set DATADIR=D:\OracleDataBackup\jcgm
set SERVER_ROOT=%Server1_IP%\E$\%remoteDataDir%  
  
echo make the netbios connection to server
net use \\%SERVER_ROOT% %PASSWORD% /user:%USER%

echo copy MPFTP to local
xcopy /d /y /e /q \\%SERVER_ROOT% "%DATADIR%"
forfiles /p "%DATADIR%" /s /m *.* /d -7 /c "cmd /c del @path"
echo client side script finished.
```

![](/images/OS_Windows/Windows间文件拷贝方法_001.png)

**备注：**

1.将该脚本在备份机上设置为定时任务，就会定时的将远程主机E:\OracleDataBackup\jcgm目录向备份机同步；

2.net use \\%SERVER\_ROOT% %PASSWORD% /user:%USER%这句让当前电脑登录远程 Windows 服务器，拿到访问共享文件夹的权限。需注意SERVER\_ROOT指向的必须是目录，不能是具体的文件；

3.如果net use登录能成功，但xcopy命令拷贝文件报“无效驱动器规格”，是因为远程服务器上对应的目录没有为登录用户开启共享权限，通过下面命令来开启

```
net share D$=D:\
```
