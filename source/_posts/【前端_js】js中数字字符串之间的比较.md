---
title: '【前端_js】js中数字字符串之间的比较'
date: 2019-05-28 05:20:00
categories:
  - 前端_js
------

js中字符串间的比较是按照位次优先，比较各字符的ASCII大小，包括数字字符串之间的比较。

1.console.log("1"<"3");//true 

2.console.log('5'<'10');//false  

3.console.log("123"<"123");//false

4.console.log("abc"<"aad");//false
