---
title: '【前端_js】js获取url中的参数'
date: 2020-03-15 08:35:00
categories:
  - 前端_js
------

## js获取url路径中的参数

```
<script>
$(function(){
// 返回 web 主机的域名，如：http://127.0.0.1:8080/testdemo/test.html?id=1&name=test
var url = window.location.href;
// 返回当前页面的路径和文件名，如：/testdemo/test.html
var pathname = window.location.pathname;
// 返回 web 主机的端口，如：8080
var port = window.location.port;
// 返回所使用的 web 协议，如：http:
var protocol = window.location.protocol;
// 获取参数，如：?id=1&name=test
var search = window.location.search;
// 获取指定参数值
var id = getUrlParam('id'); // 1
var name = getUrlParam('name'); // test
});
```

```
function getURLParameter(url, paramName) {  
    var urlParams = url.split("?")[1];//或者url.search获取参数字符串  
    var paramArray = urlParams.split("&");  
    var len = paramArray.length;  
    var paramObj = {};//json对象  
    var arr = [];//数组对象  
    for (var i = 0; i < len; i++) {  
        arr = paramArray[i].split("=");  
        paramObj[arr[0]] = arr[1];  
    }  
    for (key in paramObj) {  
        if (key == paramName) {  
            return paramObj[paramName];  
            break;  
        }  
    }  
  
}  
  
</script>
```
