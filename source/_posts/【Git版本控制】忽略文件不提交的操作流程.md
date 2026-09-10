---
title: '【Git版本控制】忽略文件不提交的操作流程'
date: 2026-07-09 07:28:00
categories:
  - Git版本控制
------

# git忽略文件不提交的操作流程
> .gitignore 只对\*\*未跟踪(untracked)\*\* 文件生效。
> 一旦文件被 `git add` + `git commit` 进入索引,后续 `.gitignore` 就不再起作用。
## 流程图
```
┌──────────────────────────────────┐
│ 想忽略某个文件/目录 │
└──────────────────────────────────┘
│
▼
┌──────────────────────────────────┐
│ git ls-files <路径> │
│ (有输出 = 已跟踪,无输出 = 未跟踪) │
└──────────────────────────────────┘
│
┌───────────────┴───────────────┐
▼ ▼
┌─────────────┐ ┌─────────────┐
│ A: 未跟踪 │ │ B: 已跟踪 │
│ (ls-files 无)│ │ (ls-files 有)│
└─────────────┘ └─────────────┘
│ │
▼ ▼
┌──────────────────┐ ┌─────────────────────┐
│ 直接把路径加入 │ │ 问:远端仓库要不要保留?│
│ .gitignore 即可 │ └─────────────────────┘
│ (无需 commit │ │
│ .gitignore 本身)│ ┌────────┴─────────┐
└──────────────────┘ ▼ ▼
┌────────┐ ┌────────────┐
│ B1:不保留│ │ B2:要保留 │
│ (远程也要│ │ (仅本地自定义│
│ 删掉) │ │ 不入库) │
└────────┘ └────────────┘
│ │
▼ ▼
┌────────────────┐ ┌──────────────────────┐
│ git rm -r --cached <路径>│ │ git update-index │
│ (生成 delete 暂存)│ │ --skip-worktree <路径>│
└────────────────┘ └──────────────────────┘
│ │
▼ ▼
┌────────────────┐ ┌──────────────────────┐
│ 路径加入 │ │ 效果: │
│ .gitignore 并暂存│ │ - 本地修改不被提交 │
└────────────────┘ │ - git status 不显示 │
│ │ modified │
▼ │ - git add . 不会提交 │
┌────────────────┐ │ - git pull 冲突保护, │
│ git commit │ │ 不会静默覆盖本地 │
│ (一次性提交 │ └──────────────────────┘
│ delete + .gitignore)│
└────────────────┘
```
## 各分支详解
### A — 未跟踪文件(简单)
\*\*判定\*\*:
```bash
git ls-files <路径>
# 无输出 → 未跟踪
```
\*\*操作\*\*:
```bash
# 编辑 .gitignore,追加:
<路径>
# 完事,无需 commit .gitignore(只要不被 add 即可)
```
### B — 已跟踪文件
#### B1 — 已跟踪但远端也要删(常规清理)
\*\*判定\*\*:
```bash
git ls-files <路径>
# 有输出 → 已跟踪
```
\*\*典型场景\*\*:
- 不小心 commit 进去的临时文件 (`backend/tmp/\*.txt`)
- 不该入库的 IDE 配置 (`.idea/`)
- 误提交的环境变量文件 (`.env.local`)
\*\*操作\*\*(3 步):
```bash
# 第 1 步:从索引删除(本地文件保留)
git rm -r --cached <路径>
# 例:git rm -r --cached backend/tmp/
# 第 2 步:把路径加入 .gitignore 并暂存
echo "<路径>" >> .gitignore
git add .gitignore
# 第 3 步:一次性提交 delete + .gitignore
git commit -m "chore: 取消跟踪 <路径>(已加入 .gitignore)"
```
\*\*关键点\*\*:
- `--cached` 必须有:只删索引,不动本地文件
- 必须在 `git rm` 之后再编辑 `.gitignore`,否则 delete 暂存会被 .gitignore 二次忽略导致 commit 时 delta 为空
#### B2 — 已跟踪但仅本地自定义不入库(高级)
\*\*判定\*\*:与 B1 相同,但业务上需要保留远端版本,只让本地修改不入库。
\*\*典型场景\*\*:
- `application-dev.yml` 个人本地配置(密码、端口)
- 团队共享但有个别开发者本地定制
\*\*操作\*\*(单文件,慎用):
```bash
git update-index --skip-worktree <文件>
# 例:git update-index --skip-worktree backend/src/main/resources/application-dev.yml
```
\*\*行为\*\*:
| 操作 | 效果 |
|------|------|
| 本地修改文件 | Git 假装没看到,`git status` 不显示 modified |
| `git add .` | 不会把本地修改加进暂存区 |
| `git commit` | 不会提交本地改动 |
| `git pull` 拉取远端更新 | \*\*会抛冲突,不静默覆盖本地自定义\*\* |
\*\*⚠️ 适用范围\*\*:
- 单个文件级别生效
- 仓库里 \*\*不\*\*提交 `.gitignore`(因为本地要忽略的文件已经入库了)
- 多个文件需要逐个执行
- 团队里其他人\*\*看不到\*\*这个标记(只对本地有效),不适合协作级"忽略"
\*\*解除\*\*(需要恢复跟踪时):
```bash
git update-index --no-skip-worktree <文件>
```
## 同类风险排查脚本
```bash
# 列出所有"已跟踪但本该忽略"的文件(说明被错误跟踪)
git ls-files | xargs -I{} sh -c 'git check-ignore -v "{}" 2>/dev/null && echo {}'
# PowerShell 版本
git ls-files | ForEach-Object {
if (git check-ignore -v $\_ 2>$null) { $\_ }
}
```
输出样例:
```
.gitignore:5:backend/tmp/ backend/tmp/c4.txt
.gitignore:5:backend/tmp/ backend/tmp/compile-out.txt
```
这些文件需要按 B1 处理。
## B1 vs B2 决策速查
| 场景 | 选 B1 | 选 B2 |
|------|-------|-------|
| 临时构建产物 (`target/`, `dist/`) | ✅ | ❌ |
| IDE 配置 (`.idea/`, `.vscode/`) | ✅ | ❌ |
| 团队误提交的文件 | ✅ | ❌ |
| 个人本地环境差异 (端口/密码) | ❌ | ✅ |
| 文件远端有版本但本地定制 | ❌ | ✅ |
| 跨人需要"本地各自忽略" | ❌ | ❌(用 .git/info/exclude 替代) |
## 个人级 ignore(团队外的本地忽略)
不属于本决策树。如果只是想本地临时忽略某个文件,\*\*不入库\*\*也不影响别人,用:
```bash
# .git/info/exclude (本地级,不入版本控制)
echo "<路径>" >> .git/info/exclude
```
这是最稳的"本地忽略"方案,优先级高于 `.gitignore`,且只对本人生效。
## 参考
- [Git 官方文档 - gitignore](https://git-scm.com/docs/gitignore):只对 untracked 文件生效
- [Git 官方文档 - git-update-index](https://git-scm.com/docs/git-update-index):`--skip-worktree` 选项详解
- [Git 官方文档 - git-rm](https://git-scm.com/docs/git-rm):`--cached` 行为说明
