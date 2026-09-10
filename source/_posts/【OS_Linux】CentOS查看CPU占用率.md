---
title: '【OS_Linux】CentOS查看CPU占用率'
date: 2026-05-12 05:48:00
categories:
  - OS_Linux
------

# CentOS查看CPU占用率
## 方法一：top 实时查看（最常用）
```bash
top
```
进入 top 界面后，直接按 \*\*大写 P\*\*
- 自动按 \*\*CPU 使用率从高到低\*\* 排序
- 按 `q` 退出
### 补充常用快捷键
- 按 \*\*1\*\* ：查看所有 CPU 核心负载
- 按 \*\*M\*\* ：改成按内存排序
- 按 \*\*P\*\* ：切回按 CPU 排序
---
## 方法二：ps 一次性列出（不用进交互界面）
### 按 CPU 从高到低排序
```bash
ps -aux --sort=-pcpu
```
### 只看前 10 个占用最高进程
```bash
ps -aux --sort=-pcpu | head -10
```
字段简单说明：
- `%CPU` ：进程 CPU 占用率
- `USER` ：所属用户
- `COMMAND` ：进程命令
---
## 方法三：查看某个程序的 CPU 占用
```bash
ps -aux | grep mysql
```
---
### 极简记住
- 实时看：`top` 然后按 \*\*P\*\*
- 一次性列表：`ps -aux --sort=-pcpu | head -10`
