---
title: '【前端_js】ajax的应用'
date: 2019-05-30 12:28:00
categories:
  - 前端_js
------

1.设置请求头部

```
function makeRequest() {
    alert("inside makeRequest()");
    var settings = {
        type: "GET",
        url:URL+"?"+REQUEST,
        dataType:"xml",
        error: function(XHR,textStatus,errorThrown) {
            alert ("XHR="+XHR+"\ntextStatus="+textStatus+"\nerrorThrown=" + errorThrown);
        },
        success: function(data,textStatus) {
            $("body").append(data);
        },
        headers: {
            "Access-Control-Allow-Origin":"http://example.edu",
            "Access-Control-Allow-Headers":"X-Requested-With"
        }
    };
    $.ajax(settings);
}
```

2.ajax参数详解：[Ajax最详细的参数解析和场景应用](https://blog.csdn.net/u011277123/article/details/53500913)

3.ajax请求错误调试分析：[jQuery中ajax错误调试分析](https://www.jb51.net/article/98808.htm)

4.[JQuery中$.get、$.post、$.getJSON、$.ajax 方法详解](https://blog.csdn.net/huileiforever/article/details/12163385)
