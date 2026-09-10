---
title: '【IDE_IntelliJ IDEA】IntelliJ IDEA 中自定义模板代码的缩写'
date: 2019-02-27 08:12:00
categories:
  - IDE_IntelliJ IDEA
------

# 方法一：新建 Live Template

step1.

点击 File – Setting

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_001.png)

step2.  
选择 Live Template，点击右侧的+号，选择 Template Group

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_002.png)

step3.  
输入自定义的名称，然后点击OK。

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_003.png)

step4.  
选中刚创建的 Template Group，再次点击右侧的+号，选择Live Template

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_004.png)

step5.  
填入缩写词、描述、模版内容、生效文件范围等

配置模板时，变量名以$ $包围的字符的形式出现,调用的方法也是先以变量形式出现，写完模板信息后去编辑变量(Edit variables)。

$END$是一个特殊的预定义变量，表示光标最后跳转的位置。

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_008.png)

step6.  
点击右边的Edit variables（这个只有在使用了预定义函数的时候才能点击和设置）

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_005.png)

step7.  
点击 Apply ，OK，完成！

# 方法二：使用 IDEA 自带的 Live Template

psvm 生成 main 方法  
fori 生成 for 循环  
sout 生成 System.out.println();  
…

# 方法三：修改 IDEA 自带的 Live Template

以将 psvm 修改成 main 和 sout 修改成 syso 为例：

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_006.png)

![](/images/IDE_IntelliJ_IDEA/IntelliJ_IDEA_中自定义模板代码的缩写_007.png)

本文转载于：[使用 IDEA 的 Live Template 实现自动提示代码功能](https://blog.csdn.net/WJJPro/article/details/78452914%20)
