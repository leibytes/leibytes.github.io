---
title: '【OS_Linux】Linux 基本命令整理'
date: 2019-04-09 13:32:00
categories:
  - OS_Linux
------

1. 查看目录文件：ls  
2. 打印当前工作目录：pwd  
3. 查看文件内容：cat 文件名  
4. 打开编辑器：vim 文件名

|  |  |
| --- | --- |
| 1  2  3  4  5 | `修改：按Insert键`  `退出修改模式：按Esc 键`  `进入输命令模式：同时按键：Shift+；`  `保存退出：wq`  `不保存退出：q` |

5. 删除文件（夹）：rm 文件（夹）（可以多个）  
6. 重命名/剪切：mv 老名 新名  
7. 复制 ：cp 老文件路径+文件名 新文件路径（+文件名）  
8. 进入新路径 cd 路径名  
9. 后退一步：cd ..  
10. 返回刚才工作的目录：cd -  
11. 查找文件的路径： locate 文件名  
12. top 查看所有进程的运行状况  
13. Ctrl + Alt + T 创建新的命令窗口  
14. 查看本文件夹中文件的个数

```
ls   -l   |grep   "^-"   |   wc   -l
```

**说明:**

* **ls   -l**       长列表输出该目录下**文件信息**(这里的文件可能是目录、链接、设备文件等)
* **grep   ^-**   这里将长列表输出信息过滤一部分，只保留**一般文件**                  如果只保留**目录**就是   grep   ^d
* **wc   -l**     统计输出信息的行数，因为已经过滤得只剩一般文件了，所以统计结果就是一般文件信息的行数，又由于一行  信息对应 一个文件，所以也就是文件的个数。

15. 查看文件、目录的大小(du——disk usage)

|  |  |
| --- | --- |
| 1  2 | `参数：-s :仅显示总计    -h :以k、m、g为单位，提高可读性。`  `可这样用：  du  -sh filename/dirname` |

16. 重定向

* $echo "test" > temp.txt会先清空temp.txt然后再写入
* $echo "test" >> temp.txt会以追加的方式写入

17. 打印文件某几行

```
head filename 打印文件前10行
head -n 5 filename  打印文件前5行
tail filename 打印文件后10行
tail -n 5 filename 打印文件后5行
tail -f filename 不断打印文件新增内容
```

18. 统计

![复制代码](/images/OS_Linux/Linux_基本命令整理_001.gif)

```
wc -l 统计行数
wc -w filename 统计单词数
wc -c filename 统计字符数
more  现实一页
more +n      从笫n行开始显示
more -n       定义屏幕大小为n行
```

![复制代码](/images/OS_Linux/Linux_基本命令整理_001.gif)

19. tree 用图形化的树状结构打印文件和目录  
20. 别名 alias 例如：现实文件带颜色

```
alias ls='ls --color=tty'
```

21. VIM怎样在不退出当前编译文件下使用ls / pwd / vim等命令  :!   例如 :!ls

22. 显示2天前的日期

```
day=`date -d"2 day ago" +"%Y-%m-%d"`;  #结果2014-10-19
```

23. 显示linux下默认编码

```
locale
```

案例

|  |  |
| --- | --- |
| 1  2  3  4  5  6  7  8  9  10  11  12  13  14 | `LANG=en_US.UTF``-8`  `LC_CTYPE=``"en_US.UTF-8"`  `LC_NUMERIC=``"en_US.UTF-8"`  `LC_TIME=``"en_US.UTF-8"`  `LC_COLLATE=``"en_US.UTF-8"`  `LC_MONETARY=``"en_US.UTF-8"`  `LC_MESSAGES=``"en_US.UTF-8"`  `LC_PAPER=``"en_US.UTF-8"`  `LC_NAME=``"en_US.UTF-8"`  `LC_ADDRESS=``"en_US.UTF-8"`  `LC_TELEPHONE=``"en_US.UTF-8"`  `LC_MEASUREMENT=``"en_US.UTF-8"`  `LC_IDENTIFICATION=``"en_US.UTF-8"`  `LC_ALL=` |

设定

```
export LC_ALL=en_US.UTF-8
```
