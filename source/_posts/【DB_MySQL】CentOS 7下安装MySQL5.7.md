---
title: '【DB_MySQL】CentOS 7下安装MySQL5.7'
date: 2026-04-16 07:54:00
categories:
  - DB_MySQL
------

本文是基于rpm包的方式在CentOS 7下安装MySQL5.7版本（含卸载 MariaDB、安装、初始化、改密码、远程连接、防火墙），直接复制执行即可。
## 一、完整步骤
### 1. 环境准备（卸载自带 MariaDB）
```bash
# 1. 停止服务（如果存在）
systemctl stop mariadb mysqld
systemctl disable mariadb mysqld
# 2. 查看已安装 MariaDB/MySQL
rpm -qa | grep -E "mariadb|mysql"
# 3. 卸载所有相关包（避免依赖冲突）
yum remove -y mariadb\* mysql\*
# 4. 删除残留配置和数据（关键，否则初始化报错）
rm -rf /etc/my.cnf /etc/my.cnf.d
rm -rf /var/lib/mysql
rm -rf /var/log/mysqld.log
```
### 2. 下载 MySQL 5.7 RPM 包（x86\_64）
```bash
# 进入安装目录
cd /usr/local/src
# 下载（国内可换清华镜像）
wget https://cdn.mysql.com/Downloads/MySQL-5.7/mysql-5.7.44-1.el7.x86\_64.rpm-bundle.tar
# 解压
tar -xvf mysql-5.7.44-1.el7.x86\_64.rpm-bundle.tar
```
解压后关键包：
- mysql-community-common-5.7.44-1.el7.x86\_64.rpm
- mysql-community-libs-5.7.44-1.el7.x86\_64.rpm
- mysql-community-libs-compat-5.7.44-1.el7.x86\_64.rpm
- mysql-community-client-5.7.44-1.el7.x86\_64.rpm
- mysql-community-server-5.7.44-1.el7.x86\_64.rpm
### 3. 安装依赖与 MySQL（严格按顺序）
```bash
# 1. 安装系统依赖
yum install -y libaio perl net-tools
# 2. 导入官方 GPG 密钥（避免签名错误）
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
# 3. 按依赖顺序安装 RPM 包
# ① common
rpm -ivh mysql-community-common-5.7.44-1.el7.x86\_64.rpm
# ② libs
rpm -ivh mysql-community-libs-5.7.44-1.el7.x86\_64.rpm
# ③ libs-compat（兼容库）
rpm -ivh mysql-community-libs-compat-5.7.44-1.el7.x86\_64.rpm
# ④ client
rpm -ivh mysql-community-client-5.7.44-1.el7.x86\_64.rpm
# ⑤ server（核心）
rpm -ivh mysql-community-server-5.7.44-1.el7.x86\_64.rpm
```
### 4. 初始化与启动 MySQL
```bash
# 1. 初始化（自动生成临时密码）
mysqld --initialize --user=mysql
# 2. 授权数据目录权限
chown -R mysql:mysql /var/lib/mysql
# 3. 启动服务并设置开机自启
systemctl start mysqld
systemctl enable mysqld
systemctl status mysqld # 看到 active (running) 即成功
```
### 5. 获取临时密码并修改 root 密码
```bash
# 1. 查看临时密码
grep 'temporary password' /var/log/mysqld.log
# 输出类似：A temporary password is generated for root@localhost: abcdefg123456
# 2. 登录并修改密码（密码需符合复杂度：大小写+数字+符号）
mysql -u root -p
# 输入上面的临时密码
# 修改密码（示例：MyNewPass@123）
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass@123';
# 刷新权限
FLUSH PRIVILEGES;
exit;
```
### 6. 配置远程连接（允许 root 从任意 IP 登录）
```bash
mysql -u root -p
# 输入新密码
# 授权
GRANT ALL PRIVILEGES ON \*.\* TO 'root'@'%' IDENTIFIED BY 'MyNewPass@123' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit;
```
### 7. 开放防火墙 3306 端口
```bash
# 开放端口
firewall-cmd --add-port=3306/tcp --permanent
# 重载生效
firewall-cmd --reload
# 查看
firewall-cmd --list-ports
```
### 8. 验证安装
```bash
# 查看版本
mysql --version
# 本地登录
mysql -u root -p
# 远程连接（从另一台机器）
mysql -u root -p -h 你的服务器IP
```
## 二、RPM 安装后 MySQL 关键目录查询
### 1. 核心固定路径（RPM 安装默认）
- 配置文件目录：/etc/my.cnf（主配置文件）、/etc/my.cnf.d/（配置文件目录）
- 数据目录（数据库文件存放）：/var/lib/mysql/
- 安装目录（命令、二进制文件）：/usr/bin/（mysql、mysqldump 等命令）、/usr/sbin/（mysqld 服务进程、/usr/share/mysql/（字符集、脚本等）
### 2. 命令查询（权威验证）
```bash
# 1. 查询配置文件路径
mysqld --help --verbose | grep -A 10 "Default options"
# 2. 查询数据目录
mysql -u root -p -e "SHOW VARIABLES LIKE 'datadir';"
# 3. 查询安装相关目录（命令路径）
which mysql
which mysqld
```
### 3. 一句话总结
- 配置文件：/etc/my.cnf
- 数据目录：/var/lib/mysql/
- 命令/安装目录：/usr/bin/、/usr/sbin/
