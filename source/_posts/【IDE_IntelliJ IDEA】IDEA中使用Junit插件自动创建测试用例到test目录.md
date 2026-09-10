---
title: '【IDE_IntelliJ IDEA】IDEA中使用Junit插件自动创建测试用例到test目录'
date: 2019-08-20 08:22:00
categories:
  - IDE_IntelliJ IDEA
------

### 第一步

从插件资源库中搜索JunitGenerator V2.0插件并安装

![](/images/IDE_IntelliJ_IDEA/IDEA中使用Junit插件自动创建测试用例到test目录_003.png)

### 第二步

配置测试用例的生成目录

1.打开File->Settings

2.搜索junit，找到JUnit Generator

3.Properties选项卡里的Output Path为测试用例生成的目录，修改为test目录：${SOURCEPATH}/../../test/java/${PACKAGE}/${FILENAME}

4.切换到JUnit 4选项卡，可以修改生成测试用例的模板，比如类名、包名等

![](/images/IDE_IntelliJ_IDEA/IDEA中使用Junit插件自动创建测试用例到test目录_002.png)

### 第三步

为指定的方法创建自动创建测试用例

![](/images/IDE_IntelliJ_IDEA/IDEA中使用Junit插件自动创建测试用例到test目录_001.png)
