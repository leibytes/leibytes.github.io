---
title: '【前端_React】Node.js和webpack的安装'
date: 2019-07-26 02:54:00
categories:
  - 前端_React
------

### 第一步——安装Node.js

首先要安装Node.js，Node.js自带了软件包管理工具npm，可以使用npm安装各种插件。[Node.js的下载地址](http://nodejs.cn/download/)

可以自定义安装到指定的路径下，待安装完成后命令行下检测是否安装成功。

![](/images/前端_React/Node.js和webpack的安装_002.png)

### 第二步——在node的安装目录下新建两个文件夹，用于存放后续以npm安装的全局模块

新建node\_cache文件夹，用于存放缓存

新建node\_global文件夹，用于存放全局模块

![](/images/前端_React/Node.js和webpack的安装_003.png)

新建完成后命令行下配置路径

npm config set cache  D:\ProgramSoftware\nodejs\node\_cache

npm config set prefix  D:\ProgramSoftware\nodejs\node\_global

注意：输入指令后，命令行闪一下，不会有任何提示

### 第三步——全局安装webpack并配置webpack的环境变量

webpack分为全局安装，与项目安装（安装在你指定的文件夹内），我这里使用的是全局安装

启动cmd，运行如下的命令：

npm install webpack -g

点击回车，会有一个进度条，等待进度条完成后，出现如下信息(具体信息可能有差别，但是只要不出现error说明安装成功)：

![](/images/前端_React/Node.js和webpack的安装_006.png)

安装完webpack后，打开D:\ProgramSoftware\nodejs\node\_global目录，会发现里面多了webpack的安装内容

![](/images/前端_React/Node.js和webpack的安装_005.png)

配置webpack的环境变量，这一步至关重要，**当运行webpack命令提示不是内部或外部命令的问题，就是因为环境变量未配置导致的**

将webpack的安装路径：D:\ProgramSoftware\nodejs\node\_global追加到用户环境变量path后面

![](/images/前端_React/Node.js和webpack的安装_001.png)

配置完webpack的环境变量后，即可在命令行下运行webpack命令

![](/images/前端_React/Node.js和webpack的安装_004.png)
