---
title: '【Nginx】Linux（CentOS）安装 Nginx'
date: 2026-04-27 03:25:00
categories:
  - Nginx
------

# Linux（CentOS）安装 Nginx
CentOS 版本：CentOS 7
Nginx 版本：1\.26\.2
两种安装方式：
一、通过 yum 安装，最简单，一键安装，全程无忧。
二、通过编译源码包安装，需具备配置相关操作。
最后附 \+：设置 Nginx 服务开机启动
---
## 一、通过 yum 安装
需要 root 权限，普通用户使用 sudo 进行命令操作
安装参考信息：[https://nginx\.org/en/linux\\_packages\.html\#RHEL](https://nginx.org/en/linux\_packages.html#RHEL)
### 1、安装依赖
```bash
sudo yum install yum-utils
```
### 2、创建仓库文件
在 /etc/yum\.repos\.d 目录下创建仓库文件 nginx\.repo，并在文件中添加以下内容：
```bash
sudo vim /etc/yum.repos.d/nginx.repo
```
写入内容：
```ini
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx\_signing.key
module\_hotfixes=true
[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx\_signing.key
module\_hotfixes=true
```
保存退出：`:wq`
### 3、安装 Nginx
默认情况下，安装 Stable version 当前稳定版本
```bash
sudo yum install nginx
```
当提示接受 GPG key 时，验证指纹匹配 `573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62`，接受即可。
### 查看 nginx 安装目录
```bash
whereis nginx
```
- /usr/sbin 目录存放 nginx 启动程序
- /etc/nginx 目录存放 nginx 配置文件
- /usr/share/html 目录存放网站项目文件
### 4、开放端口 80
检查端口是否开放
```bash
sudo firewall-cmd --zone=public --query-port=80/tcp
```
开放端口
```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
```
重新加载
```bash
sudo firewall-cmd --reload
```
再检查端口是否开放
```bash
sudo firewall-cmd --zone=public --query-port=80/tcp
```
### 5、启动 Nginx
```bash
sudo nginx
```
### 6、验证 Nginx
在远程终端，打开浏览器，输入服务器 IP，回车，看到 `Welcome to nginx\!` 即为安装成功。
---
## 二、通过编译源码包安装
需要 root 权限，使用 root 用户进行命令操作
编译源码包：\.tar\.gz 或 \.tar 包文件
### 1、下载 Nginx
可在 Nginx 官网：[https://nginx\.org/](https://nginx.org/) 下载，也可使用 wget 命令直接下载到 CentOS（需先安装 wget）
```bash
wget https://nginx.org/download/nginx-1.26.2.tar.gz
```
### 2、上传 Nginx 文件到 CentOS
使用 FinalShell 等远程登录工具，以 root 用户连接，将下载好的 nginx 源码包上传至服务器 /root 目录。
### 3、解压 Nginx
创建目录 /export/server
```bash
mkdir -p /export/server
```
解压到目录 /export/server
```bash
tar -zxvf nginx-1.26.2.tar.gz -C /export/server
```
查看解压后情况
```bash
ls -l /export/server
```
### 4、安装编译环境和依赖
安装 gcc\-c\+\+ 编译器
```bash
yum -y install gcc-c++
```
安装 openssl 相关依赖
```bash
yum install -y openssl openssl-devel
```
安装 pcre
```bash
yum install -y pcre pcre-devel
```
安装 zlib
```bash
yum install -y zlib zlib-devel
```
### 5、安装 Nginx
进入到解压后的 nginx 目录
```bash
cd /export/server/nginx-1.26.2
```
查看目录内容
```bash
ls -l
```
配置安装目录，将安装到 /export/server/nginx 这个目录下
```bash
./configure --prefix=/export/server/nginx
```
执行 make 编译
```bash
make
```
执行 make install 安装
```bash
make install
```
查看安装后的情况
```bash
ls -l /export/server
ls -l /export/server/nginx
```
### 6、开放端口 80
检查端口是否开放
```bash
firewall-cmd --zone=public --query-port=80/tcp
```
开放端口
```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
重新加载
```bash
firewall-cmd --reload
```
再检查端口是否开放
```bash
firewall-cmd --zone=public --query-port=80/tcp
```
### 7、启动 Nginx
进入到 nginx 安装目录下的 sbin
```bash
cd /export/server/nginx/sbin
ls -l
```
启动 nginx
```bash
./nginx
```
查看 nginx 相关的进程
```bash
ps -ef | grep nginx
```
### 8、验证 Nginx
浏览器输入服务器 IP，看到 `Welcome to nginx\!` 即为安装成功。
### 9、删除编译源码包和安装解压目录
删除编译源码包
```bash
rm /root/nginx-1.26.2.tar.gz
```
删除安装解压目录
```bash
rm -rf /export/server/nginx-1.26.2
```
---
## 附 \+：设置 Nginx 服务开机启动
需要 root 权限，使用 root 用户进行命令操作
原理：利用 systemctl 管理服务
### 1、新建服务
在 /usr/lib/systemd/system 目录下，新建 nginx\.service 文件
```bash
vim /usr/lib/systemd/system/nginx.service
```
配置内容：
```ini
[Unit]
Description=Nginx Server
After=network.target
[Service]
User=root
Group=root
Type=forking
ExecStart=/export/server/nginx/sbin/nginx -c /export/server/nginx/conf/nginx.conf
ExecStop=/export/server/nginx/sbin/nginx -s quit
ExecReload=/export/server/nginx/sbin/nginx -s reload
Restart=on-failure
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```
### 2、重新加载 systemctl
```bash
systemctl daemon-reload
```
### 3、启动 Nginx
```bash
systemctl start nginx.service
ps -ef | grep nginx
```
### 4、设置 Nginx 开机启动
设置开机启动
```bash
systemctl enable nginx.service
```
查看设置情况
```bash
systemctl is-enabled nginx
```
### 5、重启计算机
```bash
reboot
```
### 6、验证 Nginx 开机启动
```bash
systemctl status nginx.service
ps -ef | grep nginx
```
---
## 附 \+：Nginx 的其他命令
1、取消 Nginx 开机启动
```bash
systemctl disable nginx.service
```
2、重启 Nginx
```bash
systemctl restart nginx.service
```
3、重新加载配置
```bash
systemctl reload nginx.service
```
4、停止 Nginx
```bash
systemctl stop nginx.service
```
