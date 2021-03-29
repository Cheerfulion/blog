---
title: 问题记录
description: '-'
tags:
  - Git
abbrlink: 1ddb9d4e
date: 2021-03-17 23:58:04
---



## 用production分支的内容替换master分支的内容，并备份master分支

1. 首先备份`master`分支到`master-old`分支

   ```shell
   # 切换到master分支
   git checkout master
   # 创建分支master-old并切换到master-old分支
   git checkout -b master-old
   # 上传分支
   git push --set-upstream origin master-old
   ```

2. 用production分支的内容替换master分支的内容

   ```shell
   # 切换到master分支
   git checkout master
   # 将本地的master分支内容重置成远程production分支的内容
   git reset --hard origin/production
   # 推送到远程仓库（如果master是保护分支需要先取消保护）
   git push --force 
   ```



## Error: You have not concluded your merge (MERGE_HEAD exists).

**解决办法一:** 保留本地的更改,中止合并->重新合并->重新拉取

```shell
git merge --abort
git reset --merge
git pull
```

**解决办法二:** 舍弃本地代码,远端版本覆盖本地版本(慎重)

```shell
git fetch --all
git reset --hard origin/master
git fetch
```



## git pull代码很慢问题

近来日本站点出现git拉取代码经常 `Connection timed out` 问题，因为是国外站点，访问搭建在国内的git仓库网络慢导致，但是今天连续拉取了三四次都是超时，不得不关注起来这个问题。

首先，我们的git仓库对外是不能ping通的，所以不好看延时。但是通过 `telnet gitlab.xxx.com 22` 看到SSH的端口是开放的。

![1608295464276](http://blog.cdn.ionluo.cn/blog/1608295464276.png)

于是我疑惑了，感觉没有突破口，无意中看了下绑定的远程仓库位置， `git remote -v`。 发现居然是使用的https来链接远程仓库的，估计之前后台搭建的时候使用的是账密拉取的方式。于是，我又通过telnet测试下服务器的https端口是否可以访问，`telnet gitlab.xxx.com 443`, 果然，一直`Trying`中。于是把仓库改成ssh关联就可以了。

如何修改可以看我的这篇文章中的【如何快速关联/修改Git远程仓库地址】：

https://blog.csdn.net/ion_L/article/details/82531585



## 清除仓库的所有提交信息

最近基于旧项目新开发了一个新的站点，上传git后发现以前的提交信息还在，因此想到重置仓库，最简单的就是删除 `.git` 文件夹，重新通过 `git init` 创建仓库。但是这样子也会丢失所有分支和tag的信息，因此可以通过下面命令把主分支`master`的旧记录清空就好了。



> 正如上面所说，该方法只是清除主分支master的记录，甚至可以说此master非彼master，查看master的历史记录是空的，但是并没有给.git文件件瘦身的效果哦。

1.切换到新的分支

```bash
git checkout --orphan latest_branch
```

2.缓存所有文件（除了.gitignore中声明排除的）

```bash
 git add -A
```

3.提交跟踪过的文件（Commit the changes）

```bash
 git commit -am "commit message"
```

4.删除master分支（Delete the branch）

```bash
git branch -D master
```

5.重命名当前分支为master（Rename the current branch to master）

```bash
 git branch -m master
```

6.提交到远程master分支 （Finally, force update your repository）

```bash
 git push -f origin master
```



## `.git`文件夹比代码文件大得多

这个问题无法避免，因为`.git`文件主要用来记录每次提交的变动，当我们的项目越来越大的时候， `.git`文件也越来越大。
很大的可能是因为提交了大文件，如果你提交了大文件 A，那么即使你在之后的版本中将其删除，但实际上，记录中的大文件仍然存在。
原因在于虽然你在后面的版本中删除了大文件A，但是Git是有版本倒退功能，那么如果大文件A不记录下来，git拿什么来回退呢？
git给出了解决方案，使用git branch-filter来遍历git history tree, 可以永久删除history中的大文件，达到让.git文件瘦身的目的。

```bash
# 下面方法未尝试（参考：https://blog.csdn.net/qq_39054069/article/details/107401872）

#1.找出大文件的前5个
git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -5
#2.找出大文件的文件名
git rev-list --objects --all | grep 8f10eff91bb6aa2de1f5d096ee2e1687b0eab007
#3.根据HSA值找到对应文件名
git rev-list --objects --all | grep 1ada5755215275b7b8c8cfad079bf1edc1322ff2
#4.清除该文件的所有历史记录并强制刷新到所有分支(慎重,需要管理员权限,否则报错)
git filter-branch --index-filter 'git rm --cached --ignore-unmatch <your-file-name>'
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git fsck --full --unreachable
git repack -A -d
git gc --aggressive --prune=now
git push --force [remote] master
```



**因为我这里git历史对于我没有什么用，所以我选择了重置git仓库的方法**。

```bash
rm -rf .git

git init 

git add .

git commit -m "重置git仓库，解决.git文件夹超过500M问题"

git remote add origin git@gitee.com:cheerfulion/my_notes.git

git push --force orign master
```



另外，gitee等仓库也会提供清空代码等功能，不需要命令操作。



> PS: 上面的方法都是治标不治本，需要解决.git文件夹过大的问题，还是要平时尽量注意把无关的内容（如npm包）和二进制文件不要纳入版本管理。文本文件可以通过gc合并操作达到减少版本文件来实现缩减硬盘空间，但是二进制文件是无法通过gc合并的。建议把静态资源等使用如七牛云来存储，也方便后期添加CDN加速功能。