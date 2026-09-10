---
title: '【前端_React】npm常用命令'
date: 2019-07-26 03:17:00
categories:
  - 前端_React
------

### 安装模块（包）：

```
//全局安装
$ npm install 模块名 -g
//本地安装
$ npm install 模块名
//一次性安装多个
$ npm install 模块1 模块2 模块n --save
//安装运行时依赖包
$ npm install 模块名 --save
//安装开发时依赖包
$ npm install 模块名 --save-dev
```

### 卸载模块（包）：

```
//卸载本地模块
$ npm uninstall 模块名
//卸载全局模块
$ npm uninstall -g 模块名
```

### 查看已安装的模块（包）：

```
npm list -g --depth 0
```

[npm常用命令](https://www.jianshu.com/p/087d839e1d0c)
