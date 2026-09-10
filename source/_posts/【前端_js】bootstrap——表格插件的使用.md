---
title: '【前端_js】bootstrap——表格插件的使用'
date: 2019-07-03 03:37:00
categories:
  - 前端_js
------

## BootStrap Table显示行号，并且分页后依然递增

```
{  
    title: '序号',  
    field: '',  
    formatter: function (value, row, index) {  
        return index+1;  
    }  
}
```

以上方式不能满足分页后依然递增，下面这种方法可以

```
columns: [
    {
        title: '序号',
        field: '',
        align: 'center',
        formatter: function (value, row, index) {
            var pageSize = $('#table的id').bootstrapTable('getOptions').pageSize;//通过table的#id 得到每页多少条
            var pageNumber = $('#table的id').bootstrapTable('getOptions').pageNumber; //通过table的#id 得到当前第几页
            return pageSize * (pageNumber - 1) + index + 1; // 返回每条的序号： 每页条数 *（当前页 - 1 ）+ 序号
        }
    }
]
也可以这样写
columns: [
    {
        title: '序号',
        field: '',
        align: 'center',
        formatter:rowNumber
 } ]
function rowNumber(value, row, index) {
            var pageSize = $('#table的id').bootstrapTable('getOptions').pageSize; //通过table的#id 得到每页多少条
            var pageNumber = $('#table的id').bootstrapTable('getOptions').pageNumber; //通过table的#id 得到当前第几页
            return pageSize * (pageNumber - 1) + index + 1;    // 返回每条的序号： 每页条数 *（当前页 - 1 ）+ 序号
        }
```

[Bootstrap Table配置教程](http://www.itxst.com/Bootstrap-Table/)

[bootstrap 分页表格插件](https://www.cnblogs.com/yingsong/p/6180441.html)
