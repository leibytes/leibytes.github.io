---
title: '【实用工具】Hexo + GitHub Pages 搭建个人博客完整教程'
date: 2026-09-10 16:00:00
categories:
  - 实用工具
---

# Hexo + GitHub Pages 搭建个人博客完整教程

> Windows 环境实测通过，从零开始搭建个人技术博客，免费托管在 GitHub。

---

## 一、环境准备

搭建博客前需要安装两个软件：Node.js 和 Git。

### 1.1 安装 Node.js

1. 访问 [nodejs.org](https://nodejs.org)，下载 LTS 版本（建议 v20 以上）
2. 双击安装包，一路下一步即可
3. 安装完成后打开 Git Bash，验证：

```bash
node -v    # 显示 v20.x.x 即成功
npm -v     # 显示 10.x.x 即成功
```

### 1.2 安装 Git

1. 访问 [git-scm.com](https://git-scm.com)，下载 Windows 版本
2. 安装时保持默认选项，完成后右键桌面应看到 "Git Bash Here"
3. 验证：

```bash
git --version    # 显示 git version 2.x.x 即成功
```

> **国内加速**：npm 默认源在国内较慢，建议先切换镜像：
> ```bash
> npm config set registry https://registry.npmmirror.com
> ```

---

## 二、Hexo 初始化

在你想放博客的目录（如 `D:\blog`）空白处右键 → Git Bash Here，执行：

```bash
# 1. 全局安装 hexo 命令行工具
npm install -g hexo-cli

# 2. 初始化博客项目（目录名自定义）
hexo init leiblog

# 3. 进入目录并安装依赖
cd leiblog
npm install
```

### 目录结构说明

```
leiblog/
├── _config.yml          # 站点主配置（标题、作者、部署等）
├── package.json         # 项目依赖
├── scaffolds/           # 文章模板
├── source/
│   ├── _posts/          # 文章放这里（.md 文件）
│   └── images/          # 图片放这里
└── themes/              # 主题目录（npm 安装的主题在 node_modules）
```

---

## 三、Butterfly 主题安装

Butterfly 是一款高颜值 Hexo 主题，基于 npm 安装方式最方便。

```bash
# 1. 安装主题及渲染器
npm i hexo-theme-butterfly hexo-renderer-pug hexo-renderer-stylus

# 2. 复制主题配置文件到根目录
cp node_modules/hexo-theme-butterfly/_config.yml _config.butterfly.yml
```

### 3.1 修改主配置 `_config.yml`

打开根目录 `_config.yml`，修改以下几项：

```yaml
# 主题必须改成 butterfly（默认是 landscape）
theme: butterfly

# 站点信息
title: 我的博客
author: 你的昵称
language: zh-CN
url: https://你的用户名.github.io/
```

### 3.2 修改主题配置 `_config.butterfly.yml`

常用配置项：

```yaml
# 主页副标题
subtitle: 记录成长

# 头像（把图片放进 source/images/）
avatar:
  img: /images/avatar.png
  effect: true

# 导航菜单（格式：菜单名: 路径 || 图标）
menu:
  首页: / || fas fa-home
  分类: /categories/ || fas fa-folder-open
  归档: /archives/ || fas fa-archive
  标签: /tags/ || fas fa-tags
```

> **注意：菜单格式**
>
> Butterfly 的菜单必须是「菜单名: 路径 || 图标」这种字符串格式，不能用数组格式（`- name: / path:`），否则会报 `label.split is not a function` 错误。

---

## 四、本地启动预览

初始化完成后，先本地预览确认效果：

```bash
# 清理旧生成文件并重新构建
hexo clean && hexo g

# 启动本地服务器
hexo s
```

浏览器打开 [http://localhost:4000](http://localhost:4000)，应看到 Butterfly 风格首页。

### 创建分类页和标签页（重要）

Hexo 不会自动生成分类总览页和标签总览页，需要手动创建：

```bash
# 创建分类页
hexo new page categories

# 创建标签页
hexo new page tags
```

然后编辑 `source/categories/index.md`，加上 `type: categories`：

```markdown
---
title: 分类
date: 2026-09-10 12:00:00
type: categories
---
```

标签页 `source/tags/index.md` 同理，加 `type: tags`。

---

## 五、添加文章

### 5.1 新建文章

```bash
hexo new "文章标题"
```

会在 `source/_posts/` 下生成 `文章标题.md`。

### 5.2 文章格式

每篇文章由 front-matter（头部元数据）和正文组成：

```markdown
---
title: 文章标题
date: 2026-09-10 12:00:00
categories:
  - 分类名
tags:
  - 标签1
  - 标签2
---

正文从这里开始，支持 Markdown 语法。

## 二级标题

![](/images/分类/图片名.jpg)
```

### 5.3 放图片

- 把图片放到 `source/images/分类名/` 目录
- 正文引用：`![](/images/分类名/图片名.jpg)`
- 图片路径以 `/` 开头，Hexo 会自动映射到 source 目录

### 5.4 本地预览文章

```bash
hexo clean && hexo g && hexo s
```

---

## 六、部署 GitHub Pages 两种方案

### 6.1 准备工作：创建 GitHub 仓库

1. GitHub 新建仓库，仓库名必须是 `你的用户名.github.io`（如 `leibytes.github.io`）
2. 选 Public，不勾选初始化 README

### 6.2 方案 A：GitHub Actions（推荐）

原理：本地只推源码，GitHub 云端自动构建并部署。

**步骤 1**：在博客根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm
      - run: npm ci
      - run: npx hexo generate
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

**步骤 2**：仓库 Settings → Pages → Source 选 **GitHub Actions**

**步骤 3**：推送源码

```bash
git init
git add -A
git commit -m "init blog"
git branch -M main
git remote add origin https://github.com/用户名/用户名.github.io.git
git push -u origin main
```

> **日常发布**：以后写完文章只需：
> ```bash
> git add -A && git commit -m "每次修改的备注" && git push
> ```
> Actions 自动构建部署。

### 6.3 方案 B：hexo-deployer-git

原理：本地构建静态文件后直接推送到 GitHub。

**步骤 1**：安装部署工具

```bash
npm i hexo-deployer-git
```

**步骤 2**：修改 `_config.yml`

```yaml
deploy:
  type: git
  repo: https://github.com/用户名/用户名.github.io.git
  branch: main
```

**步骤 3**：部署

```bash
hexo clean && hexo g && hexo d
```

**步骤 4**：仓库 Settings → Pages → Source 选 **Deploy from a branch** → main / root

### 6.4 两种方案对比

| | 方案 A：GitHub Actions | 方案 B：hexo-deployer-git |
|---|---|---|
| **原理** | 本地推源码，云端自动构建 | 本地构建后推静态文件 |
| **本地依赖** | 不需要装 Hexo（云端构建） | 必须装 Node.js + Hexo |
| **在线编辑** | 支持（GitHub 网页直接写 md） | 不支持 |
| **源码管理** | 仓库里是源码，不怕本地丢 | 仓库里是静态文件，本地源码另存 |
| **部署速度** | 1-2 分钟（Actions 构建） | 即时（本地构建完即推） |
| **适合场景** | 多设备写文章、在线编辑 | 本地深度调试主题 | |

---

## 七、Hexo 常用命令速查

| 命令 | 作用 | 使用场景 |
|---|---|---|
| `hexo init <目录>` | 初始化新博客项目 | 第一次搭建时执行 |
| `hexo new "标题"` | 新建一篇文章 | 写新文章，生成 source/_posts/标题.md |
| `hexo new page "分类名"` | 新建一个独立页面 | 创建分类页、标签页、关于页 |
| `hexo clean` | 清理 public 目录和缓存 | 改了配置或文章不生效时先执行 |
| `hexo g`（generate） | 生成静态文件到 public/ | 部署前执行，每次改完都要跑 |
| `hexo s`（server） | 启动本地预览服务器 | 本地预览，默认 http://localhost:4000 |
| `hexo d`（deploy） | 部署到远程仓库 | 方案 B 专用，配合 hexo-deployer-git |
| `hexo g -d` | 生成并部署（一条命令） | 方案 B 的快捷操作 |
| `hexo clean && hexo g && hexo s` | 清理+生成+预览三连 | 日常调试最常用组合 |

---

## 八、常见问题

### Q1：首页显示默认主题（landscape）

检查 `_config.yml` 中 `theme: butterfly` 是否改了（默认是 landscape）。

### Q2：分类页 / 标签页 404

需要手动创建：`hexo new page categories`，并在生成的 `index.md` 中加上 `type: categories`。标签页同理。

### Q3：文章图片 404

- 确认图片放在 `source/images/` 目录下
- 正文引用路径以 `/` 开头：`![](/images/xxx.jpg)`
- 子路径部署（仓库名不是用户名.github.io）需在 `_config.yml` 设 `root: /仓库名/`，图片路径也要加前缀

### Q4：label.split is not a function

菜单配置格式错误。Butterfly 菜单必须是 `菜单名: 路径 || 图标` 字符串格式，不能用数组格式。

### Q5：GitHub 网页打不开

国内访问 GitHub 不稳定，可改 hosts 文件。用 SwitchHosts 工具订阅自动更新的 GitHub hosts 列表，每小时自动更新 IP。

### Q6：hexo d 报错 Spawn failed

Git 未加入系统 PATH。在 Git Bash 中执行：`export PATH="$PATH:/c/Program Files/Git/cmd"`，或将 Git 的 cmd 目录加入 Windows 系统环境变量。

### Q7：中文标题 URL 是一串编码

正常现象。浏览器会自动 URL 编码解码，GitHub Pages 支持中文路径。

### Q8：部署后白屏或样式丢失

检查 `_config.yml` 的 `url` 和 `root` 是否正确：

- 主站（用户名.github.io）：`root: /`
- 子路径（用户名.github.io/仓库名）：`root: /仓库名/`
