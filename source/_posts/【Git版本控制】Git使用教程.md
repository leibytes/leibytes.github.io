---
title: '【Git版本控制】Git使用教程'
date: 2019-05-21 02:32:00
categories:
  - Git版本控制
------

# 1.Git的综述

 SVN是集中式版本控制系统，版本库集中放在中央服务器上，而干活时用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作。

 Git则是分布式版本控制系统，即它就没有中央服务器，每个人的电脑就有一个属于自己的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。最终，各自将本地分支先合并，再将合并后的分支推向同名的远程库。

Git和GitHub的区别：Git是分布式版本管理工具，而GitHub是一个网站，可以在github上建立一个网上仓库，然后提交自己的代码到该网上仓库，这样你的每次提交，别人也都可以看到你的代码，同时别人也可以帮你修改你的代码，这种开源的方式非常方便程序员之间的交流和学习。

GitHub是一个非常适合程序员交流的网站，很多国际上的技术大牛都在github上有自己的开源代码，其他人只要申请个账号就可以随意的看到这些大牛写的程序。同时国内的很多互联网公司如百度，阿里等也在github上公布有开源的代码。

工作区、本地库、远程库三者之间的工作原理及流程如下图：

![](/images/Git版本控制/Git使用教程_002.png)

# 2.Git本地库管理

## 2.1.Git的分支管理策略

Git之所以能实现分布式版本控制，就是基于其分支管理策略。本地库所做的操作都是在某分支上进行的，不同分支之间可以合并。可以拉取远程分支到本地，也可以推送本地分支到远程。

git branch <branchName> #创建新的分支

git branch -d <branchName> #删除已有分支

git checkout <branchName> #切换到其它分支上

git merge <branchName> #合并branchName分支到当前分支上

git branch -r #查看远程分支

git branch -a #查看远程及本地的所有分支，当前分支前加"\*"号

### 2.1.1.主分支master

Git的分支管理策略中，代码库应该有且仅有一个稳定的主分支，通常是默认的master分支。主分支上不做开发，仅用作发布新版本，故主分支也叫做版本分支。

### 2.1.2.开发分支develop

通常，开发应在另一个develop分支上完成。如果想正式对外发布，就在master分支上对develop分支进行"合并"（merge）。

#从master上分出一个develop分支

git checkout -b develop master

#将develop分支合并到master分支的命令如下两个：

#切换到master分支

git checkout master

#对develop分支进行合并

git merge --no-ff develop

【注】git checkout -b <newBranch> <originalBranch>命令从原有分支originalBranch上分出一个新分支newBranch，并切换到该分支上。

默认情况下，Git执行"快进式合并"（fast-farward merge），会直接将master分支指向develop分支。使用--no-ff参数后，会执行正常合并，在master分支上生成一个新节点，这可以清晰地保留版本演进过程。

## 2.2.提交文件到当前分支

工作区、暂存区以及版本库三者之间的关系。

**工作区：**就是你在电脑上看到的目录，比如目录testgit下的所有文件(.git版本库目录除外)。或者以后需要再新建的目录文件等等都属于工作区范畴。

**暂存区(stage)：**工作区有一个.git隐藏目录,这个不属于工作区，这是版本库。其中版本库里面存了很多东西，其中最重要的就是stage(暂存区)，还有就是Git为我们自动创建的第一个master分支。

在Git中每次提交都分为两个步骤：

第一步：用add命令将文件添加到版本库的暂存区

第二步：用commit命令把暂存区的所有内容提交到当前分支上

![](/images/Git版本控制/Git使用教程_003.jpg)

![](/images/Git版本控制/Git使用教程_001.jpg)

# 3.本地库与远程库之间同步

## 3.1.GitHub上进行SSH认证

由于本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以必须要让github仓库认证你的SSH key，在此之前，必须要生成SSH key。

第一步：生成SSH Key

在windows下查看[c盘->用户->自己的用户名->.ssh]下是否有id\_rsa、id\_rsa.pub文件，如果没有需要手动生成。打开git bash，在控制台中输入以下命令：$ ssh-keygen -t rsa -C [youremail](mailto:youremail@example.com)

 参数-t用于指定密钥类型，若未指定则默认生成用于SSH-2的RSA密钥，这里使用的是rsa。

参数-C用来指定对该密钥的注释，方便用户标识这个密钥，指出密钥的用途或其它的信息，这里一般输入在GitHub上注册时填的邮箱作为该密钥的标识。

第二步：登录github，打开setting->SSH keys，点击右上角New SSH key，把公钥id\_rsa.pub的全部内容放进key输入框，再为当前的key起一个title来标识每个key。

## 3.2.由远程库向本地库同步

由远程库向本地库同步时远程库充当的是来源，本地库充当的是去向。

### 3.2.1.clone命令克隆远程库到本地

$ git clone <远程库的网址>

$ git clone <版本库的网址> <本地库目录名>

$ git clone –o 远程主机名 <版本库的网址> <本地库目录名>

**注：**该命令会在当前目录生成一个新目录，与远程主机的版本库同名。如果要指定不同的目录名，可以将目录名作为git clone命令的第二个参数。并在克隆的时候将远程主机默认命名为origin，如果想用其它的远程主机名，需要用git clone命令的-o选项指定。实例：

$ git clone -o jQuery <https://github.com/jquery/jquery.git>

### 3.2.2.拉取远程分支到本地并建立关联

语法：

git checkout -b 本地分支名 origin/远程分支名

这个将会自动创建一个新的本地分支，并与指定的远程分支关联起来。

### 3.2.3.pull命令拉取远程分支并与本地分支合并

git pull命令先拉取远程分支到本地，再与本地指定的分支合并。

语法：$ git pull <远程主机名> <远程分支名>:<本地分支名>

实例：$ git pull origin next:master

如果是将远程分支与本地当前分支合并，则冒号后面的目的分支可省略。

实例：$ git pull origin next

上面命令表示取回origin/next分支，再与当前分支合并。实质上，这等同于先做git fetch，再做git merge。

$ git fetch origin

$ git merge origin/next

【注】1.从远程fetch到的分支在本地都是以"远程主机名/分支名"表示的。

2.如果远程主机删除了本地所关联的分支，git pull在拉取远程分支时不会删除所关联的本地分支。这是为了避免，由于其他人删除了远程主机的分支，导致git pull时不知不觉删除了与本地关联的分支。

3.从远程克隆仓库到本地时默认给远程主机命名为origin，可以通过选项-o来指定远程主机名。

## 3.3.由本地向远程同步

### 3.3.1.remote命令管理远程主机名

为了便于管理，Git要求每个远程主机都必须指定一个主机名。git remote命令就是用于管理远程主机名的，这包括对远程主机名的增、删、改、查操作。

添加远程主机

语法：$ git remote add <远程主机名> <网址>

删除远程主机

语法：$ git remote rm <远程主机名>

修改远程主机名

语法：$ git remote rename <原主机名> <新主机名>

查询远程主机

查询所有与本地库关联的远程主机名

语法：$ git remote

查询所有与本地库关联的远程主机名及其地址

语法：$ git remote –v

实例：

origin [git@github](mailto:git@github).com:jquery/jquery.git (fetch)

origin [git@github](mailto:git@github).com:jquery/jquery.git (push)

查看某远程主机的详细信息

语法：$ git remote show <远程主机名>

### 3.3.2.push命令将本地分支推送到远程主机上

当本地分支有了更新，就需要将其推送到远程与之关联的分支上

语法：$ git push <远程主机名> <本地分支名>:<远程分支名>

如果略去了冒号后的远程分支名，则表示将本地分支推送到远程与之关联的分支上；若该远程分支不存在，则会在远程主机新建该分支；若省略本地分支名，则表示删除指定的远程分支。

实例:

#推送master分支到origin主机的master分支上

$ git push origin master

#本地分支为空，表示删除远程的master分支

$ git push origin :master

# 等同于

$ git push origin --delete master

【注】1.从远程向本地拉取时远程分支是来源，由本地向远程推送时本地分支是来源

2.由远程向本地拉取时，若省略冒号前的来源分支（即远程分支）则表示拉取与当前分支关联的远程分支进行合并；若省略冒号后的目的分支（即本地分支），则视当前分支为目的分支。但是，由本地向远程push时，若省略冒号前的来源分支（即本地分支）则表示删除冒号后的远程分支。
