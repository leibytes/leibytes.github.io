---
title: '【OS_Linux】Linux配置环境变量的方法'
date: 2026-04-02 01:39:00
categories:
  - OS_Linux
------

# 1. **设置环境变量的方法**

设置环境变量有两种方法，一种是临时设置，一种是通过配置文件永久设置。

## 1.1. **临时设置**

临时环境变量关闭终端后失效，直接在终端执行 export命令：

```
# 语法：export 变量名=值
export MY_VAR="Hello World"
# 追加路径到现有的PATH环境变量
export PATH=$PATH:/opt/myapp/bin  也可以分赋值+导出两步进行
VAR_NAME="value" # 先赋值（局部变量）
export VAR_NAME # 再导出为环境变量
```

## 1.2. **永久设置**

通过修改环境配置文件来设置永久生效的环境变量，配置文件分为用户级配置文件和系统级配置文件。用户级配置文件仅对当前用户生效，系统级配置文件对所有用户生效。环境配置文件的位置和加载顺序与系统版本、解释器类型、登录方式都有关系。常见的解释器有Bash、Zsh、Fish、Dash等，登录方式分为登录shell（如通过ssh远程登录、putty登录、su -或 su -l）和非登录 Shell（如图形界面打开的终端、bash命令启动、su不带 -）。

### 1.2.1. **Linux环境变量****文件****分两类**

1.系统级：对所有用户生效（/etc 开头）

2.用户级：只对当前用户生效（~ 开头）

#### 1.2.1.1. **系统级（全局，所有用户）**

这些文件修改后，**所有用户登录**都会加载：

· /etc/profile系统登录时第一个加载的环境变量文件

· /etc/bashrc / /etc/bash.bashrc系统级 bash 配置，开终端就加载

· /etc/environment系统最顶层环境变量，**所有进程都能读到**

#### 1.2.1.2. **用户级（仅当前用户）**

只对你自己账号生效：

· ~/.profile

· ~/.bash\_profile

· ~/.bashrc（最常用！）

### 1.2.2. **配置文件生效的规则**

#### 1.2.2.1. **登录式Shell**

清除掉所有变量通过文件重新读入，具体场景有：

(1) SSH登录

(2) 服务器本地登录

(3) 使用su - username切换的用户

登录式Shell配置文件的加载顺序：/etc/profile → ~/.bash\_profile → ~/.profile → ~/.bashrc

#### 1.2.2.2. **非登录****式****Shell**

会继承上一个shell的全部变量，具体场景有：

(1) 图形界面下打开的终端

(2) su UserName

(3) 执行脚本（每当我们执行脚本的时候就已经进入到了一个子shell）

非登录式Shell配置文件的加载顺序：/etc/bashrc → ~/.bashrc

#### 1.2.2.3. **系统全局（所有进程都生效）**

etc/environment

(1) 不受登录 / 非登录影响

(2) 系统启动就加载

(3) 所有进程、所有用户都能读

**注：查询是登录shell还是非登录shell的方法**

# 通过环境变量0的值来查看当前是登录 Shell还是非登录 Shell

```
echo $0
```

# 若为登录Shell 显示：-bash

# 非登录 Shell 显示：bash

### 1.2.3. **修改环境****变量****配置文件**

#### 1.2.3.1. **修改用户级配置文件**

# 编辑配置文件

```
vim ~/.bash_profile

# 或

nano ~/.bashrc

# 在文件末尾添加

export JAVA_HOME=/usr/lib/jvm/java-11-openjdk

export PATH=$PATH:$JAVA_HOME/bin

export PATH=$PATH:/home/user/custom/bin

#重启终端或者执行source ~/.bash_profile两种方法都可以生效新的配置文件

source ~/.bash_profile
```

1.2.3.2. **修改系统级配置文件**

① /etc/environment（系统级配置，需管理员权限才能修改，不需要export）

```
# 编辑文件
sudo vim /etc/environment
# 格式（不用写export）
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"
JAVA_HOME="/usr/lib/jvm/java-11-openjdk"
# 重启或重新登录后生效
```

② /etc/profile（对所有用户生效，修改需管理员权限）

```
# 编辑文件
sudo vim /etc/profile
# 添加
export PATH=$PATH:/usr/local/custom/bin
export LANG=zh_CN.UTF-8
# 立即生效
source /etc/profile
```
