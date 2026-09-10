---
title: '【OS_Windows】bat编程基础'
date: 2026-04-02 01:55:00
categories:
  - OS_Windows
------

# 1. **变量的定义与使用**

## 1.1. **定义变量**

一般通过set关键字来定义变量并赋值，通常变量值不需要加引号。

```
set name=John Doe
```

如果加了引号会使变量值带有引号，使用时需要用%~变量来去除引号。例如set var="value"会让变量值包含引号，这时候引用时可能需要用%~var来去除。

## 1.2. **引用变量**

### 1.2.1. **变量引用时是否要加引号**

变量的值如果包含空格、特殊字符（如&、|、>、<、^）时，引用变量要加引号。

```
set file_path=C:\My Documents\test.txt
:: 错误：Documents 会被视为独立命令（报错）
copy %file_path% D:\Backup\  
:: 正确：路径作为整体传递
copy "%file_path%" D:\Backup\
```

在条件判断语句中（如 if、exist）​涉及文件/目录存在性检查、字符串比较时，路径或字符串变量必须加引号，避免空格干扰判断。

```
set target=C:\Program Files\app.exe
:: 错误：Program 和 Files 被拆分，无法正确检查文件
if exist %target% (
echo 存在)
else
echo 不存在  
:: 正确：路径作为整体检查
if exist "%target%" (echo 存在)
else (echo 不存在)
```

1.2.2. **变量的延迟扩展机制**

批处理脚本在执行时默认会率先将所有的变量引用解析成值，但if、for代码块中定义的变量是在执行时才赋值的，因此引用代码快中定义的变量时会出现空值的问题。

要启用「延迟扩展setlocal enabledelayedexpansion」并通过!号代替%号来引用变量解决即时扩展带来的问题。

1.未启用延迟扩展导致变量引用为空

```
@echo off
if 1==1 (
    set my_var=Hello  :: 变量my_var是在执行到该分支语句时才定义和赋值的
    echo 变量值：%my_var%  :: 由于即使扩展的原因导致变量为空值，输出为空
)
Pause
```

2.启用延迟扩展

```
@echo off
setlocal enabledelayedexpansion  :: 启用延迟扩展
if 1==1 (
    set my_var=Hello  :: 定义变量
    echo 变量值：！my_var！  :: 用 ! 替代 % 动态获取值
)
endlocal  :: 关闭延迟扩展（可选，释放资源）
pause
```

2. **输入输出**

## 2.1. **输入**

### 2.1.1. **基础输入：set /p​**

set /p用于提示用户输入，并将输入内容保存到变量中。

语法​：set /p 变量名=提示信息

示例：

```
@echo off
set /p username=请输入用户名：  :: 显示提示，等待用户输入后存入%username%
echo 您输入的用户名是：%username%
pause
```

2.1.2. **限制选项输入：choice​**

choice用于让用户从有限选项中选择（需Windows XP及以上系统支持）。

​语法​：choice /c 选项列表 /n /m 提示信息

​示例​：

```
@echo off
choice /c YN /n /m "是否继续？(Y/N)"  :: 选项为Y和N，不显示选项列表（/n），提示信息为"是否继续？(Y/N)"
if errorlevel 2 (
    echo 您选择了N，退出...
    exit /b
)
if errorlevel 1 (
    echo 您选择了Y，继续...
)
pause
```

**参数说明****：**

/c YN：选项为 Y和 N（用户输入Y或N）；

/n：不显示选项列表（否则会显示 [Y,N]?）；

/m：自定义提示信息；

errorlevel：返回选择的索引（从1开始，选第一个选项返回1，第二个返回2，依此类推）。

## 2.2. **输出**

通过 echo、print（不推荐）等命令向屏幕输出内容，支持文本、变量值、特殊符号等。

### 2.2.1. **基础输出：echo命令​**

echo用于输出文本或变量值，是最常用的输出命令

l **输出固定文本**

```
echo Hello World       :: 输出：Hello World
echo 这是一行测试文本  :: 输出中文（需确保编码为ANSI或UTF-8 with BOM）
```

l **​输出变量值**

```
set name=Alice
echo 用户名：%name%    :: 输出：用户名：Alice
```

l **关闭/开启回显**

```
@echo off关闭脚本自身的命令回显（默认会显示执行的命令）；
echo on重新开启回显。
@echo off  :: 脚本运行时不显示执行的命令（如 set、echo 等）
echo 静默执行...
```

### 2.2.2. **输出重定向（保存到文件）****​**

l **通过 >（覆盖）或 >>（追加）将输出保存到文件**

覆盖写入时文件不存在则创建，存在则清空

```
echo Hello > output.txt  :: 文件内容：Hello（无换行）
echo World >> output.txt :: 追加内容：Hello,追加后文件内容：HelloWorld（注意无换行）
```

l **带换行输出**

用 echo.或 echo后加换行符：

```
echo Hello > output.txt
echo. >> output.txt       :: 插入空行
echo World >> output.txt  :: 最终内容：Hello

                            ::          （空行）

                            ::           World
```

l **将命令的执行结果输出到文件**

```
dir C:\ >> dir_list.txt  :: 将C盘目录列表追加到dir_list.txt
```

3. 读取多行输入（高级）​​

若需读取用户输入的多行内容（如一段文本），可通过循环结合 set /p实现：

```
@ echo off
setlocal enabledelayedexpansion
set input_lines=
set count=0
:input_loop
set /p line=请输入第;!count!*行（直接回车结束）：
if "!line!"=="" goto :end_input
set input_lines=!input_lines!!line!
set /a count+=1
goto :input_loop
:end_input
echo 您输入的内容是：！input_lines！
pause
```

## 2.3. **读写文件**

批处理可通过重定向、for /f循环等方式读取文件内容，或向文件写入数据。

### 2.3.1. **读取文件内容到变量​**

l **读取单行**：用 set /p从文件中读取一行：

```
@echo off
set /p first_line=<C:\test.txt  :: 读取test.txt的第一行存入%first_line%
echo 第一行内容：！first_line！
pause
```

l **读取所有行（循环处理）：**用 for /f逐行读取文件：

```
@echo off
for /f "delims=" %%i in (C:\test.txt) do (
    echo 读取到行：%%i
)
pause
"delims="：禁用分隔符，保留行内所有字符（包括空格）；
%%：循环变量（批处理文件中用%%，命令行中用%）。
```

### 2.3.2. **写入内容到文件​**

l **覆盖写入：**用 >或 echo命令：

```
echo 第一行内容 > C:\output.txt
echo 第二行内容 >> C:\output.txt  :: 追加第二行
```

l **写入多行内容**：用 for循环或文本块重定向（需结合 echo和换行符）

```
(

    echo 第一行
    echo 第二行
    echo 第三行
) > C:\multi_line.txt  :: 写入三行内容（每行后自动换行）
```

**总结批处理的输入输出核心操作：**

输入：set /p获取用户输入，choice选择选项；

输出：echo文本/变量，>/>>重定向到文件，color设置颜色；

文件I/O：set /p <文件读单行，for /f循环读多行，>/>>写文件。
