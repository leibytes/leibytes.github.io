---
title: '【Git版本控制】git pull 完整的内在执行流程（两种模式示意图）'
date: 2026-07-22 06:14:00
categories:
  - Git版本控制
------

# git pull 完整的内在执行流程（两种模式示意图）
```bash
# 默认模式
git pull = git fetch + git merge
# 变基模式
git pull --rebase = git fetch + git rebase
```
> 前置环境：当前处于 `perm-v2`，上游追踪 `origin/perm-v2`
>
>
## 一、默认模式：`git pull`（fetch \+ merge）分步拆解
### 第 1 步：`git fetch origin perm-v2`
\*\*仅下载数据，不改动工作区、暂存区、本地分支 HEAD\*\*
1. 本地 Git 与远端 Gitee 建立连接；
2. 比对本地已知远端提交哈希 和 远端仓库 `perm-v2` 最新提交；
3. 将本地缺失的所有提交、文件对象下载存入本地 `.git/objects` 对象数据库；
4. 更新本地\*\*远程追踪指针\*\* `origin/perm-v2`，指向远端最新 commit；
日志特征（代表 fetch 成功）：
```Plain Text
From shturl.cc/EEhC6BqlIHi9VgGpR60bZQ2l
bcbe1af..56c9fea perm-v2 -> origin/perm-v2
```
✅ 关键点：远端代码已经永久保存在本地仓库，哪怕后续合并中断，对象不会丢失。
### 第 2 步：自动执行 `git merge origin/perm-v2`
尝试将远程追踪分支合并进当前本地分支，分为 3 种场景：
#### 场景 A：Fast\-forward 快进合并（无分叉）
本地 `perm-v2` 提交历史是 `origin/perm-v2` 的祖先，没有分叉。
👉 只单纯移动本地 `perm-v2` 指针，\*\*不产生新合并提交，无冲突\*\*。
```Plain Text
A -- B -- C(perm-v2, origin/perm-v2)
```
#### 场景 B：历史分叉，但自动合并成功（无内容冲突）
本地、远端各自产生新提交，提交线分叉；Git 自动融合两边文件差异；
✅ 自动创建一条\*\*合并提交\*\*，移动 HEAD，pull 流程结束。
```Plain Text
D (perm-v2 本地新增)
/
A--B--C
\
E (origin/perm-v2)
==合并后==
D ----\
/ \
A--B--C M(perm-v2)
\ /
E(origin/perm-v2)
```
`M` = 自动生成的合并提交。
#### 场景 C：自动合并失败（【内容冲突】场景）
同一文件同一区域两边都修改 → Git 无法自动判定最终代码
1. 遇到冲突终止合并，无冲突的合并暂存未提交；
2. \*\*工作区文件写入冲突标记\*\* `<<<<<<< HEAD ======= >>>>>>> 远端哈希`；
3. Git 内部标记仓库状态：`merging（合并进行中）`；
4. 本地分支 `perm-v2` 的 HEAD\*\*保持不动\*\*，依旧停留在 pull 之前旧提交；
##### 冲突后的人工闭环流程
1. 打开文件，删除冲突标记，编辑为最终想要的代码；
2. `git add 冲突文件`：通知 Git「冲突处理完成」，文件存入暂存区；
3. `git commit`：生成合并提交 M，移动本地分支 HEAD，\*\*退出 merging 状态\*\*；
> 放弃合并退路：`git merge --abort`
> 撤销所有合并改动，文件回退到 pull 执行之前，退出合并状态。
>
>
## 二、变基模式：`git pull --rebase`（fetch \+ rebase，推荐日常开发）
### 执行流程
1. 同样先执行 `git fetch`，下载远端代码、更新 `远程追踪指针origin/perm-v2`；
2. \*\*不执行 merge\*\*，走 rebase 逻辑：
- 临时摘取你本地独有的所有提交，保存起来；
- 将本地分支指针直接移动到远端最新 `origin/perm-v2`；
- 按顺序\*\*重新逐个应用\*\*你本地之前的提交；
冲突区别：
冲突修复完成后，使用 `git rebase --continue`，\*\*不是 git commit\*\*；
最终效果：\*\*无合并分叉线，提交历史是一条直线\*\*。
### 提交历史对比
原始分叉状态：
```Plain Text
D (perm-v2 本地提交)
/
A--B--C
\
E--F (origin/perm-v2)
```
✅ pull \-\-rebase 最终形态（线性历史）
```Plain Text
A--B--C--E--F--D'(perm-v2)
```
> D'：D 提交被重新应用后的新哈希，原 D 提交不会直接复用。
>
>
⚠️ 重要约束：\*\*已经 push 到远端共享分支，不要随便 rebase！\*\*
## 三、高频误区（结合你实际踩坑总结）
### 误区 1：`git pull` 会直接覆盖本地未提交文件
❌
merge 开始前 Git 会做安全检测：
如果工作区有未提交的修改，合并操作会终止 → \*\*直接 Abort 终止整个 pull\*\*，不会破坏本地改动（就是你最早遇到的报错：Your local changes would be overwritten by merge）
### 误区 2：合并冲突中断 = 远端代码没有拉到本地
❌
只要出现 `From xxx ... -> origin/perm-v2`，代表 fetch 执行完毕；
远端所有代码快照已经存在本地 `.git` 对象库；只是\*\*没有合并应用到工作区与本地分支\*\*。
### 误区 3：冲突后执行 `git add`，改动已经提交到本地分支
❌
`git add` 仅仅把修复后的文件存入\*\*暂存区\*\*；
只有执行 `git commit`（merge 模式）/ `git rebase --continue`（rebase 模式），分支指针才会移动，变更才算固化到本地分支。
### 误区 4：设置 `skip-worktree` 的文件会阻止 pull 更新
❌
不会阻止 fetch 下载远端更新；
当远端修改该 yml 文件，merge 阶段触发 modify/modify 冲突，\*\*不会静默覆盖你本地自定义配置\*\*，这是它优于 `assume-unchanged` 的核心优势。
## 四、默认 git pull 极简流程总图
```Plain Text
git pull
│
▼
git fetch ───> 下载远端对象 + 更新 origin/perm-v2
│
▼
git merge origin/perm-v2
├─无冲突 → 自动完成，分支指针前进，结束
└─冲突发生 → 暂停，进入merging状态
│
▼
人工修复冲突
│
▼
git add
│
▼
git commit （生成合并提交，HEAD前进，退出合并状态）
│
▼
pull流程全部结束
```
## 五、验证调试命令（PowerShell）
```powershell
# 只拉取远端，不执行合并，单独观察origin指针变化
git fetch origin perm-v2
# 图形化查看所有分支提交结构
git log --oneline --all --graph
# 查看当前是否处于合并/变基状态
git status
```
