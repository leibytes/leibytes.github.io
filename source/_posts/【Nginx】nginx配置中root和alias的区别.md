---
title: '【Nginx】nginx配置中root和alias的区别'
date: 2026-04-14 08:36:00
categories:
  - Nginx
------

## 1.root指令

当用root指令来指定资源路径时，他是将URI完整追加到root指定的路径后后面，然后去寻找资源

例如：

```
 location /images/ {
         root /var/www/html;
     }
```

对于请求http://example.com/images/test.jpg，Nginx会在/var/www/html/images/test.jpg查找文件。

## 2.alias指令

当用alias指令来指定资源路径时，他是将URI中与location匹配上的部分替换掉，然后去寻找资源

例如：

```
location /images/ {
         alias /var/www/html/images/;
     }
```

对于请求http://example.com/images/test.jpg，Nginx会在/var/www/html/images/test.jpg查找文件，注意若location匹配路径后面有斜杠，alias路径后也需要斜杠，因为是做完整替换

**总结：简单理解就是root指令是将URI追加到后面，alias指令是替换掉URI中与location匹配上的部分**
