---
title: '【前端_css】网页常用的字体'
date: 2018-11-27 08:35:00
categories:
  - 前端_css
------

# **1. 网页常用字体通常分为5类**

serif 字体在字符笔画末端有叫做衬线的小细节，这些细节在大写字母中特别明显。  
 sans-serif 字体在字符笔画末端没有任何细节，与serif字体相比，他们的外形更简单。  
monospace 字体，每个字母的宽度相等，通常用于计算机相关书籍中排版代码块。  
fantasy 和 cuisive 字体在浏览器中不常用，在各个浏览器中有明显的差异。

# 2. 各种字体的效果

代码demo

![](/images/前端_css/网页常用的字体_001.gif)![](/images/前端_css/网页常用的字体_004.gif)

```
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8">
<title>font</title>
<meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
<meta name="author" content="@my_programmer">
<style type="text/css">
/*重置{*/
*{ padding:0;margin:0;} img{border:0;} li{list-style:none;}
/*}重置*/
div{font-size:1.2em;}
</style>
</head>
<body>
    <br/>
    --网页字体通常分为5类--
    <div style="font-family: sans-serif;">你好 hello world  &nbsp;&nbsp; sans-serif(无衬线) </div>
    <div style="font-family: serif;">你好 hello world &nbsp;&nbsp;  serif(衬线) </div>
    <div style="font-family: monospace;">你好 hello world  &nbsp;&nbsp; monospace(等宽) </div>
    <div style="font-family: fantasy;">你好 hello world  &nbsp;&nbsp;&nbsp;&nbsp; fantasy(梦幻) </div>    
    <div style="font-family: cuisive;">你好 hello world  &nbsp;&nbsp; cuisive(草体) </div>
    <br/>
    --无衬线类--
    <div style="font-family: Helvetica, sans-serif;">你好 hello world  &nbsp;&nbsp; Helvetica </div>
    <div style="font-family: Arial, sans-serif;">你好 hello world &nbsp;&nbsp;  Arial </div>
    <div style="font-family: 'Lucida Grande', sans-serif;">你好 hello world  &nbsp;&nbsp; Lucida Grande </div>
    <div style="font-family: Verdana,sans-serif;">你好 hello world  &nbsp;&nbsp; Verdana </div>    
    <div style="font-family: Tahoma, sans-serif;">你好 hello world  &nbsp;&nbsp; Tahoma </div>
    <div style="font-family: 'Trebuchet MS', sans-serif;">你好 hello world  &nbsp;&nbsp; Trebuchet MS </div>
    <br/>
    --衬线类--
    <div style="font-family: Georgia, serif;">你好 hello world &nbsp;&nbsp;  Georgia </div>
    <div style="font-family: Times, serif;">你好 hello world  &nbsp;&nbsp; Times </div>
    <br/>
    --中文字体--
    <div style="font-family: 宋体">你好 hello world  &nbsp;&nbsp; 宋体 </div>
    <div style="font-family: 微软雅黑">你好 hello world  &nbsp;&nbsp; 微软雅黑 </div>
    <div style="font-family: 华文细黑">你好 hello world  &nbsp;&nbsp; 华文细黑 </div>
    <div style="font-family: 黑体">你好 hello world  &nbsp;&nbsp; 黑体 </div>

</body>
</html>
```

View Code

**在chrome上显示的结果**

![](/images/前端_css/网页常用的字体_003.png)

******在ie8上显示的结果******

******![](/images/前端_css/网页常用的字体_002.png)******
