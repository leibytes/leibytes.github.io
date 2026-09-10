---
title: '【OS_Linux】Linux删除指定日期之前的文件'
date: 2020-09-13 15:40:00
categories:
  - OS_Linux
------

语法：

find dir[#指定查找目录] -mtime +days[#指定保留天数] -type f[#查找类型] -name "\*.\*" -exec rm -rf {} \;

```
dir：指定查找目录  
-mtime：指定保留的天数，即多少天之前的文件全部删除。例如只保留历史7天内的文件，则-mtime参数就设置为7  
-type：指定查找类型，如查找文件该参数就设置为f  
-name：指定文件名，则采用正则匹配  
-exec：针对查找到的目标执行的操作  
示例：
```

```
find /u01/app/oracle/admin/orcl/dpdump -mtime +7 -name "jslt*.log" -exec rm {} \;
```

参考博文：[linux 删除指定日期之前的文件](https://blog.csdn.net/weixin_44567104/article/details/90747061)
