---
title: 【计算机网络】Linux 单公网IP主机配置SNAT实现内网共享上网
date: 2026-11-12 10:00:00
categories:
  - 计算机网络
description: 一台带公网IP的Linux主机作为网关，内网多台无公网IP服务器通过SNAT/MASQUERADE透明访问外网，同时兼容DNAT端口转发。
---
# Linux 单公网IP主机配置SNAT实现内网共享上

> 适用场景：一台带公网IP的Linux网关主机 \+ 多台无公网IP内网服务器，内网机器通过这台网关统一访问外网；同时兼容原有 DNAT 端口转发（公网端口映射到内网服务）。
> 
> 原理：开启内核 ip 转发 \+ iptables MASQUERADE/SNAT，内网服务器网关指向网关主机内网IP，透明上网，无需在内网机器配置代理。
> 
> 测试系统：CentOS7 / CentOS Stream9 / RockyLinux / Ubuntu 20\.04\+
> 
> 

## 拓扑示例

```
公网IP：36.133.232.252（网关主机外网网卡 eth0）
网关主机内网网卡：eth1，内网IP：192.168.10.1
内网服务器A：192.168.10.10
内网服务器B：192.168.10.11
内网服务器C：192.168.10.12
内网网段：192.168.10.0/24
```

> 网卡名称、网段、IP 按照你自己环境替换。
> 
> 

## 一、网关主机配置步骤

### 1\. 开启内核 IPv4 转发（必须）

内核转发只是允许数据包跨网卡转发，单独这一步无法上网，需要配合 nat 伪装规则。

```bash
# 写入配置文件永久生效
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
# 加载生效
sysctl -p
# 查看是否开启，输出1代表成功
cat /proc/sys/net/ipv4/ip_forward
```

### 2\. 配置 iptables NAT 规则

两种模式二选一：

- `MASQUERADE`：推荐，公网IP为动态IP（云主机重启、重分配IP自动适配）

- `SNAT`：公网IP固定不变时使用，性能略好

```bash
# 方式1：MASQUERADE（推荐，绝大多数云主机场景）
iptables -t nat -A POSTROUTING -o eth0 -s 192.168.10.0/24 -j MASQUERADE

# 方式2：SNAT（固定公网IP场景，替换为你的公网IP）
# iptables -t nat -A POSTROUTING -o eth0 -s 192.168.10.0/24 -j SNAT --to-source 36.133.232.252
```

参数解释：

- `-o eth0`：出站网卡，也就是连接公网的网卡

- `-s 192.168.10.0/24`：允许这个内网网段走NAT伪装

- POSTROUTING链：数据包离开主机之前做源地址转换

> 如果你同时需要 DNAT（公网端口转发到内网服务），示例DNAT规则一并放这里，可共存：
> 
> 

```bash
# 示例：公网9500端口转发到 192.168.10.10:9500
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 9500 -j DNAT --to-destination 192.168.10.10:9500
```

### 3\. 放行转发流量（filter表）

iptables 默认策略如果是 DROP，需要额外放行转发流量：

```bash
iptables -A FORWARD -s 192.168.10.0/24 -j ACCEPT
iptables -A FORWARD -d 192.168.10.0/24 -j ACCEPT
```

## 二、内网服务器配置（三台内网机都操作）

内网机器网卡网关指向网关主机内网IP `192.168.10.1`。

DNS推荐：223\.5\.5\.5、114\.114\.114\.114。

### CentOS（nmcli示例）

```bash
# 查看网卡名
nmcli device status
# 修改网关和DNS，网卡名替换为实际网卡
nmcli connection modify eth0 ipv4.gateway 192.168.10.1
nmcli connection modify eth0 ipv4.dns "223.5.5.5,114.114.114.114"
nmcli connection up eth0
```

### Ubuntu netplan示例

编辑 `/etc/netplan/00-installer-config.yaml`：

```yaml
network:
  ethernets:
    eth0:
      addresses: [192.168.10.10/24]
      gateway4: 192.168.10.1
      nameservers:
        addresses: [223.5.5.5,114.114.114.114]
  version: 2
```

应用配置：

```bash
netplan apply
```

### 连通性测试（内网机执行）

```bash
# ping外网
ping 223.5.5.5
# 域名解析测试
curl baidu.com
```

## 三、iptables 规则持久化（防止重启丢失）

不同系统持久化方式不一样，分开说明。

### CentOS 7

```bash
# 安装iptables持久化工具
yum install iptables-services -y
# 停止firewalld（firewalld和iptables-services冲突，二选一）
systemctl stop firewalld
systemctl disable firewalld
# 保存当前iptables规则
service iptables save
# 开机自启iptables服务
systemctl enable iptables
```

规则保存文件路径：`/etc/sysconfig/iptables`

### CentOS Stream 8/9 / RockyLinux

```bash
# 安装工具
dnf install iptables-services -y
systemctl stop firewalld
systemctl disable firewalld
# 保存规则
service iptables save
systemctl enable iptables
```

### Ubuntu / Debian

```bash
# 安装iptables-persistent
apt update && apt install iptables-persistent -y
# 保存ipv4规则（执行时会写入文件）
iptables-save > /etc/iptables/rules.v4
# 恢复规则命令（手动恢复用）
iptables-restore < /etc/iptables/rules.v4
```

> 安装iptables\-persistent的时候，弹窗选择Yes，保存当前规则。
> 
> 

### 通用手动备份/恢复命令（全Linux通用）

```bash
# 导出当前所有iptables规则到文件备份
iptables-save > /root/iptables-nat-backup.v4
# 需要的时候恢复
iptables-restore < /root/iptables-nat-backup.v4
```

## 四、云主机专属注意事项（重点）

1. **关闭源/目的地址校验（源目检查）**：云厂商默认开启源目校验，SNAT/DNAT转发会失效。

    - 阿里云：实例详情 → 配置 → 源/目的地址检查 → 关闭

    - 腾讯云：实例详情 → 网络 → 源目地址检测 → 关闭

2. **云安全组放行**：网关主机安全组出站全部放行；内网之间网段放行；需要对外暴露的端口放行。

3. 不要 firewalld 和 iptables\-services 混用，容易规则冲突。

## 五、SNAT透明上网 vs Nginx正向代理

|方案|特点|适用场景|
|---|---|---|
|iptables SNAT/MASQUERADE|四层透明NAT，TCP/UDP/ICMP全部支持，不需要程序配置代理|服务器yum更新、ssh、数据库访问外网、ping等通用场景|
|Nginx正向代理|应用层代理，仅支持HTTP/HTTPS，需要程序手动配置proxy|仅需要http访问、需要日志、URL过滤场景|

> 推荐优先使用SNAT，业务服务器上网最省心。
> 
> 

## 六、常用排错命令

```bash
# 查看nat表规则
iptables -t nat -L -n -v
# 查看filter转发规则
iptables -L FORWARD -n -v
# 抓包调试（网关主机公网网卡）
tcpdump -i eth0 icmp
# 查看内核转发是否开启
cat /proc/sys/net/ipv4/ip_forward
```

## 七、补充说明

1. 单点风险：网关主机停机，所有内网服务器都无法访问外网；生产环境可考虑冗余网关。

2. 流量审计：iptables SNAT四层转发，无法记录http访问URL；需要URL审计，叠加squid正向代理。

3. 端口冲突：DNAT转发端口不要和网关主机自身监听端口冲突。