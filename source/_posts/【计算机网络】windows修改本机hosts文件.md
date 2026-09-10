---
title: '【计算机网络】windows修改本机hosts文件'
date: 2019-07-22 06:14:00
categories:
  - 计算机网络
------

### hosts 文件所在的位置

C:/windows/system32/drivers/etc/hosts

### 修改后不必重启立即生效的方法

命令行下运行：   
ipconfig /displaydns

显示所有 dns内容

ipconfig /flushdns

刷新所有 dns内容

注意：修改时ip后面不能跟端口号（例如：127.0.0.1:8080这种是错误的）

[修改window本地hosts文件，修改域名指向](https://blog.csdn.net/u010234516/article/details/52963954)
