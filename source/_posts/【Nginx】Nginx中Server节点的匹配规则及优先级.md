---
title: '【Nginx】Nginx中Server节点的匹配规则及优先级'
date: 2026-04-17 05:52:00
categories:
  - Nginx
------

# 一、核心结论
Nginx 选择哪个 `server` 块，\*\*只看 2 步\*\*：
1. \*\*先匹配端口\*\*（listen）
2. \*\*再匹配请求头的Host\*\*（server\_name）
\*\*优先级从上到下，谁先满足就用谁！\*\*
---
# 二、完整匹配流程（官方标准）
当请求进来：`https://zyalmall.com` 或 `https://120.49.66.168`
## 第 1 步：匹配 \*\*listen 端口\*\*
- Nginx 先看请求是 \*\*80 / 443\*\* 等端口
- 找到所有监听该端口的 `server` 块
- 不在该端口的 server \*\*直接排除\*\*
---
## 第 2 步：在这些 server 里，按下面 \*\*优先级顺序\*\* 匹配
### 优先级 1（最高）：\*\*精准匹配 server\_name\*\*
```Plain Text
server\_name zyalmall.com;
```
Host 头完全一样 → 直接选中，结束匹配
---
### 优先级 2：通配符前缀匹配
```Plain Text
server\_name \*.zyalmall.com;
```
---
### 优先级 3：通配符后缀匹配
```Plain Text
server\_name zyalmall.\*;
```
---
### 优先级 4：正则匹配
```Plain Text
server\_name ~^www.zyalmall.com$;
```
---
### 优先级 5（最低）：\*\*默认 server（兜底）\*\*
下面两种情况，都会选中 \*\*默认 server\*\*：
1. 配置了 `default\_server`
```Plain Text
listen 80 default\_server;
```
2. \*\*没有任何 server\_name 匹配，且没有 default\_server\*\*
→ 选中该端口下 \*\*第一个 server\*\*
---
# 三、最关键规则（解释你之前的问题）
## 如果 Host 不匹配任何 server\_name
## → 用该端口下 \*\*default\_server\*\*
## → 没有 default\_server → 用该端口下 \*\*第一个 server\*\*
---
# 四、你那个 HTTPS 配置为什么 IP 能访问？
你的配置：
```Plain Text
server {
server\_name zyalmall.com;
listen 443 ssl;
}
```
请求：`https://120.49.66.168`
1. 端口 443 ✔️
2. Host =120.49.66.168
3. 不匹配 zyalmall.com ❌
4. 没有 `default\_server`
5. \*\*就去匹配 443 端口下唯一的 server\*\*
✅ \*\*这就是答案！\*\*
---
# 五、最直观的例子（一看就懂）
```Plain Text
# 第1个 server
server {
listen 80;
server\_name a.com;
}
# 第2个 server
server {
listen 80 default\_server;
server\_name b.com;
}
```
访问以下域名分别进哪个？
1. `a.com` → 精准匹配 → 进第 1 个 ✔️
2. `b.com` → 精准匹配 → 进第 2 个 ✔️
3. `192.168.1.100` → 无匹配 → 进 default\_server（第 2 个） ✔️
4. `abc.com` → 无匹配 → 进 default\_server（第 2 个） ✔️
---
# 六、最终总结（最强记忆版）
## Nginx server 匹配规则：
1. \*\*先看端口\*\*
2. \*\*再看 server\_name 精准匹配\*\*
3. \*\*再看通配符、正则\*\*
4. \*\*最后看default\_server\*\*
5. \*\*没有 default\_server → 用端口下第一个 server\*\*（最容易踩坑）
---
# 七、你现在的问题怎么解决？
## 给 443 端口加 default\_server 拦截 IP
```Plain Text
server {
listen 443 ssl default\_server;
server\_name \_;
return 403;
}
```
这样：
- 域名访问 → 正常
- IP 访问 → 被default\_server拦截
