---
title: '【前端_js】array.forEach和$.each()及$().each()的用法与区别'
date: 2019-06-28 07:16:00
categories:
  - 前端_js
------

1.$.each()：方法是jQuery中的方法，用于遍历数组或对象。用法：$.each(array,function(index,value){...})，有两个参数，第一个为待遍历的数组或对象,第二个为回调函数，函数中的两个参数，index为当前遍历到的元素下标或对象的key，value为当前遍历到的数组元素或对象的值。  
2.$().each()：一看带有$,顾名思义也是jQuery中的方法，多用于遍历dom数组。用法$('selector').each(function(index,value){...})。

3.forEach：即Array.prototype.forEach，只有数组才有的方法，等同于过去的for循环遍历数组。用法：arr.forEach(function(item,index,array){...}),其中回调函数有3个参数，item为当前遍历到的元素，index为当前遍历到的元素下标，array为数组本身。forEach方法不会跳过null和undefined元素。比如数组[1，undefine，null，，2]中的四个元素都将被遍历到，注意与map的区别。

## 【1】$('selector').each(function(index,value){...})

```
$("input[name='ch']").each(function(index,value){
    if($(this).attr("checked")==true){
        //一些操作代码
    }
}
```

## 【2】`$.each(array,function(index,value){})`

```
遍历对象
var obj = { one:1, two:2, three:3, four:4, five:5 };
$.each(obj, function(key, val) {
console.log(key+":"+val);
});
```

```
遍历数组
var arr1 = [ "one", "two", "three", "four", "five" ];
$.each(arr1, function(){
alert(this);
});
```

## 【3】arr.forEach(function(item,index,array){...})

```
var arr=[1,2,3,4];
arr.forEach(function(val,index,arr){
    arr[index]=2*val;
});
console.log(arr);//结果是修改了原数组，为每个数乘以2
```
