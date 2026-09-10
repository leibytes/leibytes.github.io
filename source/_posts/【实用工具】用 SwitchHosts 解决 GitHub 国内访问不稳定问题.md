---
title: '【实用工具】用 SwitchHosts 解决 GitHub 国内访问不稳定问题'
date: 2026-09-10 12:00:00
categories:
  - 实用工具
---

# 用 SwitchHosts 解决 GitHub 国内访问不稳定问题

国内访问 GitHub 经常遇到：网页打不开、git push 超时、图片加载不出来。改 hosts 是最简单有效的方法，但手动改一次只能管一阵子——IP 变了又得重新查。这篇文章记录用 SwitchHosts 实现**自动更新 hosts** 的完整流程。

---

## 一、问题原理：为什么 GitHub 会打不开？

### 1.1 DNS 污染

正常情况下，浏览器访问 `github.com` 时，系统会向 DNS 服务器查询这个域名对应的 IP，然后连接那个 IP。

但在国内，GitHub 的域名 DNS 查询结果经常被**污染**——DNS 服务器返回一个错误的 IP（或者不响应），浏览器拿着错误的 IP 去连接，自然就打不开。

### 1.2 hosts 文件的作用

`hosts` 文件是系统本地的"域名→IP 映射表"，优先级高于 DNS 查询。手动在 hosts 里写死 `github.com` 的正确 IP，系统就不会去问 DNS 了，直接用这个 IP 连接。

Windows 下 hosts 文件位置：

```
C:\Windows\System32\drivers\etc\hosts
```

### 1.3 为什么不能一劳永逸？

GitHub 的服务器 IP 会变（CDN 节点调整、服务器迁移等）。今天能用的 IP，过几天可能就失效了。手动改 hosts 只能管一阵子，IP 一变又得重新查。

**SwitchHosts 解决的就是"自动更新 IP"这个问题**。

---

## 二、SwitchHosts 是什么？

SwitchHosts 是一个开源的 hosts 文件管理工具，核心能力：

- **图形化管理** hosts 规则，不用手动开记事本改系统文件
- **多套规则切换**：可以配置多套 hosts 方案，一键切换（比如开发环境/生产环境）
- **远程订阅**：订阅别人维护的 hosts 列表 URL，软件定期自动拉取最新内容并写入 hosts 文件

> 官网：https://github.com/oldj/SwitchHosts

---

## 三、下载与安装

### 3.1 下载

推荐下载 **4.2.0 便携版**（Electron 版本，兼容性好，免安装）：

- Releases 页面：https://github.com/oldj/SwitchHosts/releases/tag/v4.2.0
- 找文件名：`SwitchHosts_windows_portable_x64_4.2.0.6119.exe`
- 如果 GitHub 下载慢，可以在链接前加加速前缀：
  ```
  https://ghfast.top/https://github.com/oldj/SwitchHosts/releases/download/v4.2.0/SwitchHosts_windows_portable_x64_4.2.0.6119.exe
  ```

> 5.0.x 版本基于 Tauri 重写，只提供安装包，且部分 Windows 系统可能兼容性不好。便携版建议用 4.2.0。

### 3.2 运行

便携版直接双击运行，不需要安装。

> **重要**：修改 hosts 文件需要管理员权限，**右键 → 以管理员身份运行**，否则软件提示 hosts 只读，无法写入。

---

## 四、添加远程订阅规则

### 4.1 新建远程规则

1. 打开 SwitchHosts，左上角点 **+**
2. 选择 **远程**
3. 填写：

| 字段 | 值 |
|---|---|
| 名称 | GitHub 加速 |
| 类型 | 远程 |
| URL | `https://raw.hellogithub.com/hosts` |
| 自动更新 | 勾选，间隔设 **1 小时** |

4. 点 **确定**

> 这个 URL 指向一个社区维护的 GitHub hosts 列表，里面包含 github.com、github.io、raw.githubusercontent.com 等域名的最新可用 IP，每小时自动更新。

### 4.2 启用规则

在左侧规则列表里，点击 **GitHub 加速** 那条规则前面的开关，把它切换到 **ON**。

SwitchHosts 会立刻把这条规则的内容写入系统 hosts 文件。

### 4.3 验证

打开浏览器访问 https://github.com ，应该能正常打开了。

如果还不行，刷新 DNS 缓存：

```cmd
ipconfig /flushdns
```

（CMD 管理员执行）

---

## 五、工作原理总结

整个流程串起来：

```
浏览器访问 github.com
    ↓
查 hosts 文件 → 找到 140.82.112.4 github.com
    ↓
直接连接 140.82.112.4（跳过被污染的 DNS）
    ↓
GitHub 正常访问

SwitchHosts 后台每小时：
    ↓
拉取 https://raw.hellogithub.com/hosts（最新 IP 列表）
    ↓
自动更新写入 hosts 文件
    ↓
GitHub IP 变了也不用手动改
```

---

## 六、注意事项

1. **必须管理员身份运行**：否则 hosts 文件只读，规则无法写入
2. **开机自启**：在 SwitchHosts 设置里勾选"开机启动"，关闭窗口时最小化到托盘，别完全退出，否则自动更新不会跑
3. **规则冲突**：如果同时有多条规则启用，后启用的会覆盖前面的。确保只启用"GitHub 加速"这一条，其他手动加的 hosts 规则可以删掉
4. **验证生效**：改完后如果浏览器还是打不开，执行 `ipconfig /flushdns` 清一下 DNS 缓存

---

## 七、备选远程订阅源

如果上面那个 URL 访问不了，可以换：

```
https://raw.githubusercontent.com/ineo6/hosts/master/hosts
```

（这个本身就在 GitHub 上，需要 hosts 能访问 GitHub 才能拉取，可能存在鸡生蛋的问题）

```
https://hosts.gitcdn.top/hosts.txt
```

---

通过 SwitchHosts + 远程订阅，GitHub 访问问题基本一劳永逸了。以后 GitHub 的 IP 变了，软件自动更新，不用再手动查 IP、改 hosts、清 DNS。
#（注：内容由AI生成）
