---
title: '【OS_Linux】三大文本处理工具之sed命令'
date: 2019-04-15 08:14:00
categories:
  - OS_Linux
------

## 1、sed命令的简介及用法

sed：即为流编辑器，“stream editor”的缩写。他先将源文件读取到临时缓存区（也叫模式空间）中，再对满足匹配条件的各行执行sed命令。sed命令只针对缓存区中的副本进行，不会修改或破坏源文件。默认情况下，屏幕上既会打印各行的原始内容又会打印处理后的结果。

如图 1： sed 处理过程

![](/images/OS_Linux/三大文本处理工具之sed命令_005.png)

**sed语法**：sed [选项]  匹配条件及编辑命令 输入文本（文件）

**选项：**

 　　-n∶使用安静(silent)模式，只打印被sed命令处理过的结果行，不打印未被sed命令处理的行。  
        -e∶对模式空间中的各行执行多重编辑，编辑命令的顺序将影响结果。  
        -f∶用一个sed脚本对输入进行编辑， -f filename指定sed脚本的文件名。  
        -r∶编辑条件中使用扩展的正则表达式  
        -i∶直接修改源文件，而不在屏幕上输出

**匹配条件：**

1、采用正则表达式进行匹配（包括字符匹配、次数匹配、位置匹配）

2、行号匹配 （行号从1开始）

   　　　　beginNumber~step：从行号为beginNumber开始，每隔step步长处理一次。例如： sed
-n 2~5p 含义：从第二行开始匹配，隔 5 行匹配一次，即 2,7,12.......。

　　　　　beginNumber, +N：从 beginNumber这行到往下 N 行匹配，总共匹配 N+1 行。

　　　　　beginNumber,  ~N:从addr1行开始，直到行号为N的整数倍时结束。

　　　　　beginNumber, endNumber:行号为beginNumber到endNumber之间的行。

**编辑命令：**

|  |  |
| --- | --- |
| 命 令 | 说 明 |
| i | 在当前行之前插入一行 |
| a | 在当前行之后追加一行 |
| c | 用新文本覆盖（cover）当前行 |
| d | 删除匹配的行 |
| g | 默认只修改各行中首次匹配成功的位置，该参数意为整行搜索和替换 |
| p | 打印匹配的行 |
| q | 结束或退出 sed命令 |
| ！ | 反向匹配，即对未匹配成功的行执行sed命令 |
| s | 用一个字符串替换另一个字符串 |

**案例：**

输入文件为ceshi.txt

**![](/images/OS_Linux/三大文本处理工具之sed命令_002.png)**

**选项-n的作用：**

默认情况下会将未被sed命令处理的行也打印出来

**![](/images/OS_Linux/三大文本处理工具之sed命令_004.png)**

有了选项-n后只打印经sed命令处理的行

![](/images/OS_Linux/三大文本处理工具之sed命令_001.png)

**删除命令d:**删除从第三行到最后一行内容

![](/images/OS_Linux/三大文本处理工具之sed命令_009.png)

**修改命令s:**在以两位数字结尾的行后面追加.5，符号“&”表示取匹配模式。

**![](/images/OS_Linux/三大文本处理工具之sed命令_008.png)**

![](/images/OS_Linux/三大文本处理工具之sed命令_010.png)

**行追加命令a：**

**![](/images/OS_Linux/三大文本处理工具之sed命令_007.png)**

**多个sed命令组成的通道：**前一个sed命令的输出作为下一个sed命令的输入

```
amosli@amosli-pc:~/learn/sed$ cat test.txt 
hi,this is sed command test file
linux world is so amazing

you will LOVE it!
amosli@amosli-pc:~/learn/sed$ cat test.txt | sed 's/linux/LINUX/' | sed 's/sed/SED/'
hi,this is SED command test file
LINUX world is so amazing

you will LOVE it!
```

## 2、用sed命令直接编辑源文件

**法一：**通过选项-i直接编辑源文件，而不是在屏幕上输出

![](/images/OS_Linux/三大文本处理工具之sed命令_003.png)

**法二：**先将sed处理的结果重定向到另外一个零时文件，再覆盖源文件

![](/images/OS_Linux/三大文本处理工具之sed命令_006.png)

 本博文参考了：[linux命令总结sed命令详解](https://www.cnblogs.com/ginvip/p/6376049.html)  [linux shell 脚本攻略学习19--sed命令详解](https://www.cnblogs.com/amosli/p/3497063.html)
