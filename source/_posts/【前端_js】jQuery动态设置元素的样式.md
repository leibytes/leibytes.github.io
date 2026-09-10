---
title: '【前端_js】jQuery动态设置元素的样式'
date: 2019-07-26 03:44:00
categories:
  - 前端_js
------

### 1、用css()方法查询元素的某个样式

$("div").css("padding-left"));

### 2、用css()方法设置元素的样式

法一：

$("div").css({"background-color":"yellow","font-size":"200%"});

法二、将样式封装成对象传给css方法：

```
var cssobj= {
    background-color: '#EEE',
    height: '500px',
    margin: '10px',
    padding: '2px 5px'
};
$("div").css(cssobj);
```

注意：使用方法一时，样式名称必须用引号引起来。
