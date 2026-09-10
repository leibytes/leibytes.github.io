---
title: '【计算机网络】Session机制'
date: 2019-04-16 06:46:00
categories:
  - 计算机网络
------

# 1. Http请求中Session机制

先简单说一下HTTP请求中的Session机制：Session数据保存在服务器端，SessionID保存在客户端的Cookies中（关闭浏览器时过期）。当客户端向服务器发送的HTTP请求时，SessionID通过Cookies被发送到服务器，服务器端根据Cookies中的SessionID获取获取此用户的Session信息。如此一来同一用户的多次请求就能公用session数据
