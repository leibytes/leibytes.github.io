---
title: '【前端_js】javascript中数组的map()方法'
date: 2019-07-23 03:12:00
categories:
  - 前端_js
------

数组的map()方法用于遍历数组，每遍历一个元素就调用回调方法一次，并将回调函数的返回结果作为新数组的元素，被遍历的数组不会被改变。

语法：let newAarray = arr.map(function callback(currentValue, index, array) { // Return element for newArray }

示例：

```
let numbers = [1, 5, 10, 15];
let doubles = numbers.map((x) => {
   return x * 2;
});

// doubles is now [2, 10, 20, 30]
// numbers is still [1, 5, 10, 15]

let numbers = [1, 4, 9];
let roots = numbers.map(Math.sqrt);

// roots is now [1, 2, 3]
// numbers is still [1, 4, 9]
```

参考博文：[javascript map()方法解析](https://blog.csdn.net/wangdan_2013/article/details/78316844)
