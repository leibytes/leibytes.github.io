---
title: '【OS_Linux】Linux下安装软件的方法'
date: 2019-04-24 02:01:00
categories:
  - OS_Linux
------

# **在 Linux 系统中`rpm`、`yum`、源码编译是三种最核心的软件安装方式**

## 一、核心原理与本质区别

通过一张表快速掌握核心差异：

| 特性 | RPM 安装 | YUM 安装 | 源码编译安装 |
| --- | --- | --- | --- |
| **本质** | 安装预编译的 `.rpm` 二进制包 | 基于 RPM 包的自动化包管理器 | 手动编译源代码生成可执行程序 |
| **依赖处理** | 手动解决（仅提示依赖缺失） | 自动下载并安装所有依赖 | 手动安装所有编译 / 运行依赖 |
| **安装路径** | 固定（遵循 FHS 标准，不可改） | 固定（同 RPM） | 完全自定义（通过 `--prefix` 指定） |
| **灵活性** | 低（只能用预编译的配置） | 低（同 RPM） | 极高（可定制编译参数、功能模块） |
| **易用性** | 中等（需手动处理依赖） | 高（全自动） | 低（需懂编译原理、解决编译报错） |
| **维护性** | 中等（手动升级 / 卸载） | 高（一键升级 / 卸载） | 低（无统一管理，需手动清理） |
| **适用系统** | CentOS/RHEL/Fedora（RPM 系） | CentOS/RHEL/Fedora（RPM 系） | 所有 Linux 发行版 |

## 二、详细拆解与使用示例

### 1. RPM 安装（预编译包底层二进制包安装）

**核心特点**

* RPM 是 RedHat 系系统的底层包格式，`.rpm` 文件是预编译好的二进制包（类似 Windows 的 `.exe`）；
* 安装时直接解压包到系统固定目录，但不会自动解决依赖（这是最大痛点）；
* 适合离线安装、单包安装，但依赖问题需手动处理。

**常用命令示例**

```
# 安装本地 RPM 包（-ivh：安装+详细+进度）
sudo rpm -ivh nginx-1.24.0-1.el7.x86_64.rpm

# 卸载 RPM 包（需指定包名，不是文件名）
sudo rpm -e nginx

# 查看已安装的 RPM 包
rpm -qa | grep nginx
```

### 2. YUM 安装（RPM 包的自动化管理）

**核心特点**

* YUM 是 RPM 包的上层管理工具，本质还是安装 RPM 包，但解决了 “依赖地狱”；
* 会从配置的 YUM 仓库自动下载包，并递归安装所有依赖；
* 支持一键安装、升级、卸载，是 RedHat 系系统日常安装的首选。

**常用命令示例**

```
# 安装软件（自动解依赖）
sudo yum install -y nginx

# 升级软件
sudo yum update -y nginx

# 卸载软件
sudo yum remove -y nginx

# 查看可安装的版本
yum list | grep nginx
```

### 3. 源码编译安装（手动编译源代码）

**核心特点**

* 下载软件的源代码（`.tar.gz`/`.tar.xz` 格式），手动编译成二进制程序；
* 可自定义安装路径、启用 / 禁用功能模块、优化编译参数；
* 门槛高，但灵活性最高，适合定制化需求或无预编译包的场景。

**通用编译步骤（以 Nginx 为例）**

```
# 1. 安装编译依赖（需手动查依赖列表）
sudo yum install -y gcc pcre-devel zlib-devel openssl-devel

# 2. 下载并解压源码包
wget https://nginx.org/download/nginx-1.24.0.tar.gz
tar -zxvf nginx-1.24.0.tar.gz
cd nginx-1.24.0

# 3. 配置编译参数（自定义安装路径、功能）
./configure --prefix=/usr/local/nginx  # 指定安装到 /usr/local/nginx
           --with-http_ssl_module     # 启用 SSL 模块
           --without-http_gzip_module # 禁用 gzip 模块

# 4. 编译（-j 后接CPU核心数，加速编译）
make -j4

# 5. 安装
sudo make install

# 6. 启动（需手动指定路径）
/usr/local/nginx/sbin/nginx
```

### **三、优缺点对比**

| 安装方式 | 优点 | 缺点 |
| --- | --- | --- |
| RPM | 安装速度快、离线可用、操作简单 | 依赖需手动解决、路径固定、灵活性低 |
| YUM | 自动解依赖、一键安装 / 升级、易维护 | 依赖网络（仓库）、路径固定、定制化差 |
| 源码编译 | 完全自定义、功能可裁剪、性能可优化 | 步骤繁琐、易出编译报错、维护成本高 |

### 四、选型建议（新手必看）

1. **优先选 YUM：**日常安装软件（如 MySQL、Nginx、Docker），只要有官方 YUM 源，就用 YUM —— 省心、不易出错，后续升级 / 卸载也方便。
2. **次选 RPM**：离线环境（无网络）、只有单个 RPM 包时使用，需提前手动下载所有依赖包。
3. **源码编译仅用于特殊场景：**
   * 需要自定义安装路径（如 `/data/nginx`）；
   * 需要启用 / 禁用软件的特定功能（如 Nginx 新增第三方模块）；
   * 系统无预编译包（如小众软件、最新测试版软件）。

### 五、补充说明

* **YUM 与 RPM 的关系：**YUM 是 RPM 的 “超集”，YUM 安装的本质是下载 RPM 包并自动安装，你可以理解为 “YUM = RPM + 自动依赖管理 + 仓库下载”。
* **源码编译的注意事项**：编译前必须安装编译器（如 `gcc`）和依赖库的开发包（如 `pcre-devel`，带 `-devel` 后缀的包），否则会编译失败。
* **不同版本的Linux系统底层预编译包的格式和上层包管理工具不同**：

| 发行版系列 | 预编译包格式 | 底层包管理命令 | 上层自动化包管理器 | 常用系统示例 |
| --- | --- | --- | --- | --- |
| RedHat / CentOS / RHEL / Fedora / Oracle Linux | `.rpm` | `rpm` | `yum` / `dnf` | CentOS 7、CentOS Stream、RHEL 8/9 |
| Debian / Ubuntu / Linux Mint | `.deb` | `dpkg` | `apt` / `apt-get` | Ubuntu 18.04/20.04/22.04 |
| SUSE / SLES / openSUSE | `.rpm` | `rpm` | `zypper` | SUSE Enterprise Server |
| Arch Linux / Manjaro | `.pkg.tar.zst` | `pacman` | `pacman` | Arch Linux、Manjaro |
| Alpine Linux（轻量） | `.apk` | `apk` | `apk` | Alpine 3.x |

### 六、不同系统包管理工具核心命令对比（最实用）

| 功能 | RedHat/CentOS (yum/dnf) | Debian/Ubuntu (apt) | SUSE (zypper) | Arch (pacman) | Alpine (apk) |
| --- | --- | --- | --- | --- | --- |
| 安装软件 | `yum install nginx` | `apt install nginx` | `zypper in nginx` | `pacman -S nginx` | `apk add nginx` |
| 卸载软件 | `yum remove nginx` | `apt remove nginx` | `zypper rm nginx` | `pacman -R nginx` | `apk del nginx` |
| 更新软件包缓存 | `yum makecache` | `apt update` | `zypper ref` | `pacman -Sy` | `apk update` |
| 升级所有软件 | `yum update` | `apt upgrade` | `zypper up` | `pacman -Syu` | `apk upgrade` |
| 搜索仓库中是否有某个软件的安装包 | `yum search nginx` | `apt search nginx` | `zypper se nginx` | `pacman -Ss nginx` | `apk search nginx` |
| 查看所有已安装软件 | `yum list installed` | `apt list --installed` | `zypper pa -i` | `pacman -Q` | `apk info` |
| 查询指定软件是否安装 | `yum list installed | grep [软件名]` | `apt list --installed | grep [软件名]` | `zypper se -i [软件名]` | pacman -Q [软件名] | `apk info [软件名]` |

**总结**  

1. **易用性排序：**YUM > RPM > 源码编译；
2. **灵活性排序**：源码编译 > RPM > YUM；
3. **选型核心：**日常用 YUM，离线用 RPM，定制化需求用源码编译；
4. **核心关系：**YUM 基于 RPM 实现自动化，源码编译完全脱离预编译包体系，是最灵活但门槛最高的方式。

参考博文：[yum安装软件](https://www.cnblogs.com/leiblog/p/10757427.html "yum命令安装软件")    [CentOS7安装软件包的方法](https://www.cnblogs.com/wucongzhou/p/12579440.html)    [yum和apt-get有什么区别](https://www.cnblogs.com/zzPrince/p/5643671.html)
