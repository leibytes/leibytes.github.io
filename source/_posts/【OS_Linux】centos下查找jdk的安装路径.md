---
title: '【OS_Linux】centos下查找jdk的安装路径'
date: 2020-09-17 07:09:00
categories:
  - OS_Linux
------

```
[root@erp-wx ~]# which java
/java/jdk1.8.0_101/bin/java
[root@erp-wx ~]# ls -lrt /java/jdk1.8.0_101/bin/java
-rwxr-xr-x 1 10 143 7734 Jun 22  2016 /java/jdk1.8.0_101/bin/java
[root@erp-wx ~]# ll /java/jdk1.8.0_101
total 25892
drwxr-xr-x 2 10 143     4096 Jun 22  2016 bin
-r--r--r-- 1 10 143     3244 Jun 22  2016 COPYRIGHT
drwxr-xr-x 4 10 143      122 Jun 22  2016 db
drwxr-xr-x 3 10 143      132 Jun 22  2016 include
-rwxr-xr-x 1 10 143  5090295 Jun 22  2016 javafx-src.zip
drwxr-xr-x 5 10 143      185 Jun 22  2016 jre
drwxr-xr-x 5 10 143      245 Jun 22  2016 lib
-r--r--r-- 1 10 143       40 Jun 22  2016 LICENSE
drwxr-xr-x 4 10 143       47 Jun 22  2016 man
-r--r--r-- 1 10 143      159 Jun 22  2016 README.html
-rw-r--r-- 1 10 143      526 Jun 22  2016 release
-rw-r--r-- 1 10 143 21108487 Jun 22  2016 src.zip
-rwxr-xr-x 1 10 143   110114 Jun 22  2016 THIRDPARTYLICENSEREADME-JAVAFX.txt
-r--r--r-- 1 10 143   177094 Jun 22  2016 THIRDPARTYLICENSEREADME.txt
```
