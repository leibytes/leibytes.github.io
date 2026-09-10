---
title: '【前端_js】js中求json的元素个数'
date: 2020-03-27 15:15:00
categories:
  - 前端_js
------

json同数组不同，数组有表示元素个数的属性length，而json没有，但可以通过如下方式来求json中元素的个数：

var myObject = {'name':'Kasun', 'address':'columbo','age': '29'}

var count = Object.keys(myObject).length;
