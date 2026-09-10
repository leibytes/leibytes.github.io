---
title: '【OS_Windows】Windows下解除指定端口的占用'
date: 2021-05-12 11:48:00
categories:
  - OS_Windows
------

第一步：通过命令查看指定端口对应的进程号

```
netstat -ano|findstr "8027"
```

回车执行该命令，最后一列就是PID, 这里是 18176

![](/images/OS_Windows/Windows下解除指定端口的占用_001.png)

**杀掉对应进程的两种方法：**

方法一：打开任务管理器切换到【详细信息】选项卡，对PID排序后找到对应的进程，右键结束任务

![](/images/OS_Windows/Windows下解除指定端口的占用_003.png)

方法二：通过命令来结束对应的进程

```
taskki11 /f /pid 20736
```

强制（/F参数）杀死对应的进程包括子进程（/T参数）。

PS:如果要查看进程对应的程序，可用下面的命令

```
tasklist|findstr "18176"
```

![](/images/OS_Windows/Windows下解除指定端口的占用_002.png)

输入端口号关闭对应进程的脚本，实现交互式端口解除占用

```
@echo off  
setlocal EnableDelayedExpansion
title kill_port
set /p port=请输入要关掉的端口号(0~65535):
set pid=0
for /f "tokens=2,5" %%b in ('netstat -ano ^| findstr ":%port%"') do (
    set temp=%%b
    for /f "usebackq delims=: tokens=1,2" %%i in (`set temp`) do (
        if %%j==%port% (
            taskkill /f /pid %%c
            set pid=%%c
            echo portName [ %port% ] had been killed
        ) else (
            echo This port [ %port% ] is not used by this machine
        )
    )
)
if !pid!==0 (
   echo portName [ %port% ] is not used
)
echo complete operations
pause
```

参考博文：[Windows下如何查看某个端口被谁占用](https://www.runoob.com/w3cnote/windows-finds-port-usage.html)
