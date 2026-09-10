---
title: '【OS_Linux】centos中查看已有用户信息'
date: 2020-08-03 04:31:00
categories:
  - OS_Linux
------

```
１，查看用户 cat /etc/passwd

２，删除用户 userdel  用户名

３，查看组 cat /etc/group

4，删除组 groupdel 组名

5，查看可以登录系统的用户：cat /etc/passwd | grep -v /sbin/nologin | cut -d : -f 1
```
