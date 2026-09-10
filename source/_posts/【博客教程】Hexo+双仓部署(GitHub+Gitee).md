---
title: '【博客教程】Hexo + 双仓部署 GitHub + Gitee（共用一套配置）'
date: 2026-09-10 18:30:00
categories:
  - 博客教程
---

# Hexo + Butterfly 双仓部署 GitHub + Gitee（共用一套配置）

> GitHub 国内访问不稳定，Gitee 国内快。最优方案：**两个平台建同名仓库，一套 `_config.yml` 同时推双仓**，一条 `hexo d` 同步部署。

---

## 一、方案思路

| 仓库 | 地址 | root |
|---|---|---|
| GitHub | github.com/用户名/blog | /blog/ |
| Gitee | gitee.com/用户名/blog | /blog/ |

两个平台**仓库名相同**，都是子路径 `/blog/`，所以 `_config.yml` 只写一份配置，`hexo d` 同时推两个仓库，无需切换。

---

## 二、前置准备

1. 本地已搭好 Hexo + Butterfly
2. 两个平台分别创建**同名仓库**：
   - GitHub：`blog`（或其他名字，两个平台保持一致）
   - Gitee：`blog`（和 GitHub 同名）
3. 安装部署工具：

```bash
npm i hexo-deployer-git
```

---

## 三、修改 `_config.yml`

只改一处配置：

```yaml
# 两个平台都是子路径，root 统一写仓库名
url: https://你的GitHub用户名.github.io/blog/
root: /blog/

# deploy 配两个仓库，一次推送两个平台
deploy:
  type: git
  repo:
    github: https://github.com/你的用户名/blog.git,main
    gitee: https://gitee.com/你的用户名/blog.git,master
```

> 注意：
> - GitHub 分支是 `main`，Gitee 分支是 `master`
> - url 里写 GitHub 地址即可，Gitee 会自动用它自己的域名访问
> - 图片路径统一用 `/blog/images/...`（因为是子路径）

---

## 四、批量修改图片路径

因为是子路径部署，正文里的图片引用要加 `/blog` 前缀：

```bash
find source/_posts -name "*.md" -exec sed -i 's|/images/|/blog/images/|g' {} +
```

---

## 五、一键部署

```bash
hexo clean && hexo g && hexo d
```

一条命令同时推到 GitHub 和 Gitee。

部署完成后：
- GitHub：仓库 Settings → Pages → Source 选 **Deploy from a branch** → main / root
- Gitee：仓库 → 服务 → **Gitee Pages** → 点 **更新**（首次需实名认证）

---

## 六、日常更新

以后写完文章：

```bash
hexo clean && hexo g && hexo d
```

然后去 Gitee 仓库点一下 Gitee Pages 的"更新"按钮即可。

---

## 七、效果

| 平台 | 访问地址 | 特点 |
|---|---|---|
| GitHub | `https://用户名.github.io/blog/` | 国外访问快，国内偶尔抽风 |
| Gitee | `https://用户名.gitee.io/blog/` | 国内访问快，需手动更新 Pages |

---

## 八、常见问题

### Q1：两个仓库名不一样行不行？

行，但 root 和图片路径就要分开配两套，麻烦。建议同名仓库，一套配置搞定。

### Q2：Gitee Pages 显示 404

确认：
- 仓库是公开的
- Gitee Pages 已点"启动"
- `_config.yml` 的 `root: /blog/` 和仓库名一致

### Q3：图片 404

子路径部署，正文图片路径必须是 `/blog/images/...`，不是 `/images/...`。用上面的 sed 命令批量改。

### Q4：hexo d 只推了一个仓库

检查 `_config.yml` 的 deploy.repo 格式，必须是：

```yaml
deploy:
  type: git
  repo:
    github: 地址,分支
    gitee: 地址,分支
```

两个仓库名（github/gitee）只是标签，可以随便写，关键是逗号后面跟分支名。

### Q5：GitHub Pages 选了 GitHub Actions 怎么办

如果之前用过 GitHub Actions，改成 Deploy from a branch 即可。hexo d 推的是静态文件到 main 分支，直接从分支部署。