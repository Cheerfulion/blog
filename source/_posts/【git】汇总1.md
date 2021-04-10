---
title: 汇总1
description: '-'
tags:
  - Git
abbrlink: 6b82922c
date: 2021-03-17 21:02:32
---



1、首先，我们这里选用GitHub作为代码存储仓库，git bash是我们常用的git操作窗口（cmd也可以，建议用git bash）。

GitHub：https://github.com/

Git工具：https://git-scm.com/download/win

注册GitHub账号并安装好git就可以在项目目录下右键打开git bash了。

![img](http://blog.cdn.ionluo.cn/blog/2018090817414519.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

2、设置git用户名和邮箱：

git config --global user.name "username"

git config --global user.email "email"

查看用户名和邮箱：

git config user.name

git config user.email



\3. 接下来就要连接仓库了，这时候就需要SSH Key添加到项目owner（项目拥有者）的GitHub上。

创建SSH Key。在项目主目录或者系统的目录（ C:\Users\ion\.ssh ）下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
ssh-keygen -t rsa -C "[youremail@example].com"
```

这里的youremail@example.com是GitHub的注册邮箱。如果一切顺利的话，可以在项目主目录或者（ C:\Users\ion\.ssh ）里找到`.ssh`目录，里面有**id_rsa和id_rsa.pub**两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人，所以大胆地把`id_rsa.pub`告诉我吧。

3、owner根据你的**GitHub账号（注册邮箱）和id_rsa.pub**就能将你拉入项目组里面了，然后团队开发的环境就建立起来了。

（如何加入SSH Key和创建项目组多翻翻GitHub就可以知道了，或者看下我写的另一篇博客。这里是给项目成员而非项目拥有者专门写的教程）

4、接下来描述一下团队开发方式。

​     第一步：若本地没有从GitHub上下载过代码，那么请在一个文件夹下打开git bash ，直接使用下面命令拿取代码到当前目录：

```
git clone git@github.com:Cheerfulion/scut-v2.0.git
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

git clone 命令后面的 git@github.com:Cheerfulion/scut-v2.0.git 这么一串是项目的git地址，可以在任何一个git的托管平台项目上看到，而且不需要建立链接（即不需要添加你的ssh key到他的项目上）就能下载代码。GitHub的git地址显示位置如下：

![img](http://blog.cdn.ionluo.cn/blog/20180908181636512.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**查看分支：git branch** 

![img](http://blog.cdn.ionluo.cn/blog/20180908183437297.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

前面带*号的是远程分支。默认是查看本地分支，带上参数 -r是查看远程分支，-a是远程和本地的所有分支



**创建分支：git branch <new branch name> <come from where>**

```
//示例1 从本地某一分支继承代码创建新分支
git branch new-branch-name dev

//示例2 从远程某一分支继承代码创建分支
git branch new-branch-name origin/dev

//示例3 不指定继承分支，默认继承当前所在分支
git branch new-branch-name
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**切换分支：git checkout <branch-name>**

![img](http://blog.cdn.ionluo.cn/blog/20180908183926373.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**创建并切换分支：git checkout -b <new_branch_name>**

![img](http://blog.cdn.ionluo.cn/blog/20180913094929489.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**删除分支：git branch -d <branch_name>**

**![img](http://blog.cdn.ionluo.cn/blog/20180913095207414.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**

若你的代码不是从远程克隆（git clone）下来的，提交代码到远程时就会因为没有关联到远程库而失败，关联远程库使用下面命令（后面的依旧是git地址，和git clone倒是很相似）：

**git remote add origin git@[server-name]:[path]/[repo-name].git；**

![img](http://blog.cdn.ionluo.cn/blog/2018091310222837.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

之后就可以使用 **git push -u origin <origin_branch_name>** 使进行提交了。当然提交前要add并且commit才能提交自己的修改哦。



备注：今天这样子执行遇到一个问题，如下：

![img](http://blog.cdn.ionluo.cn/blog/20181014165823625.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

解决方法来自：https://jingyan.baidu.com/article/f3e34a12a25bc8f5ea65354a.html



今天（2019-06-06 23:38）又遇到了上面这个问题，无法使用git pull --rebase origin master 解决。

![img](http://blog.cdn.ionluo.cn/blog/20190606233930688.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



解决方法：

![img](http://blog.cdn.ionluo.cn/blog/20190606234126886.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](http://blog.cdn.ionluo.cn/blog/20190606234220530.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](http://blog.cdn.ionluo.cn/blog/20190606234242231.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

参考：https://blog.csdn.net/u012145252/article/details/80628451





下面是正常情况下多人合作开发的一般步骤：

1、若本机没有下载项目文件，则 git clone 下载。

2、在项目上创建新的分支，在新的分支上进行代码调整，调整完成后到第三步。

3、先add并且commit提交本次修改后使用git pull拉取远程代码到相应项目位置。或者使用git fetch和git merge，提交修改到本地（add-->commit）需要再git merge前面执行。其实这里的git pull就是git fetch和git merge的集合。

4、最后使用git push提交当前分支到远程仓库就完成。注意：一般在正式项目上是不允许提交的当前分支是dev或者master分支的，这两个分支提交到远程也是对应远程的dev和master分支，而这两个分支通常都是要保证绝对无误的，也就是要测试部门测试通过才能合并。



使用情况小结：

git在本地修改文件后放弃修改或者删除文件如何从远程拉去该文件？

```
单个文件： git checkout  <filename>
当前目录： git checkout .
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

注意最好选好修改文件目录位置下执行，否则filename还要包括路径。

 删除误增加的文件：

```
git clean -f　　
#删除当前目录下所有没有track过的文件. 他不会删除.gitignore文件里面指定的文件夹和文件, 不管这些文件有没有被track过
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

https://blog.csdn.net/leon1741/article/details/54314565

删除本次未add的所有修改（增删改）

```Go
git checkout . && git clean -f
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

git bash 使用小技巧：

clear 清屏





\1.  已经push到远程服务器的代码,要回退需要git reset本地仓库,之后再通过git push --force强制推到非保护分支.

步骤:

① git log 查看要回退到的版本号

②git reset [--hard | --soft] <版本号>

   --hard 撤销工作区的修改,工作区内容和版本保持一致.

  --soft 保留工作区修改,以便重新提交

③git push --force 因gitlab的提交历史在本地的前面,需要加上--force强制推送,需要注意的是对于保护分支依旧会报错,这时需要管理员暂时取消分支保护.



\2. git远程分支覆盖本地分支（超级好用）

有时候同一个分支，远程的和本地的都被修改的面目全非了，如果想要把本地的替换成远程的，用下面的命令

```bash
git fetch --all
git reset --hard origin/master (这里master要修改为对应的分支名)
git pull (解决可能的冲突)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





\3. 忘记切换分支,误将代码提交到了别的远程分支（超级好用）

```bash
# 回滚提交 reset

# 首先我想到的是 reset 命令将我最近一次提交放回暂存区, 并取消此次提交.

git reset HEAD~1

# 将更改到缓存区

git add .

# 将被跟踪的内容stash, 新建的文件不用担心, 依旧是未跟踪文件.

git stash 

# 切换到正确分支（或者创建一个分支）

git checkout -t origin/production -b issue-0

# 将stash的内容pop出来

git stash pop

# 提交和推送了

git add .

git commit -m "提交信息"

git push origin issue-0 

# 但原来的分支production依旧是多了一次提交, 这时候需要切回去, 提交一次

git checkout production 

git push origin production -f    # 这里要加上-f强制提交
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





\4. 代码提交到了线上master分支上时，因为线上分支不能强制推送，要回退版本要在master上revert，从最新的开始revert到你需要回退到的位置。当然即使这样子，当需求较多而测试跟不上时，大量的分支混在master分支上并不好回退，所以最好每次都从正式拿取代码，测试时更新到master．当需要更新指定的内容时只需要更新相应的分支到正式就行了，就不从master更新到正式



\5.  error: object file .git/objects/xx/xxxxx is empty

![img](http://blog.cdn.ionluo.cn/blog/20191020145826297.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 备份.git目录

```bash
cp -a .git .git-old
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 根据修复提示删除空对象文件。根据最早的空文件提示也删除那个文件。

```bash
git fsck --full
rm .git/objects/19/b45524e14258043923c2bc4d336269f1abc67f
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 删除后再次查看修复提示,说明Head commit无效。

```bash
git fsck --full
// 段错误 (核心已转储)% (3/256)   
git reflog
// fatal: bad object HEAD
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 找到当前分支Head的前两条数据。

```bash
tail -n 2 .git/logs/refs/heads/master
//注意，如果不是master分支，则.git/logs/refs/heads/branchName

// a6f48ebd79ef39b2c43e5810fc96e95cc0867389 ed41eac57be18e8d112ca5bb898dd77c3b120321 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 第一条Head是无效的。我们需要确认第二条是我们的上次失败的commit的前一个提交。

```bash
git show ed41eac57be18e8d112ca5bb898dd77c3b120321 
//输出信息为日志详细信息。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 设置确认的commit为HEAD commit

```bash
git update-ref HEAD ed41eac57be18e8d112ca5bb898dd77c3b120321 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 再次使用 git status 和 git reflog 看看功能是否正常了。如果还不行。重启下电脑。

参考：[https://git-scm.com/book/zh/v1/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E7%BB%B4%E6%8A%A4%E5%8F%8A%E6%95%B0%E6%8D%AE%E6%81%A2%E5%A4%8D](https://git-scm.com/book/zh/v1/Git-内部原理-维护及数据恢复)

https://stackoverflow.com/questions/11706215/how-to-fix-git-error-object-file-is-empty





# window下配置.gitignore

\1. 在仓库目录下打开GitBash，输入命令 **touch .gitignore**

\2. 常用的.gitignore配置

```bash
.mvn
.iml
mvnw
mvnw.cmd
target/
help.md
.cache
.project
.settings
.classpath

*.log
logs/
 
.idea
.idea/

# dependencies
node_modules/

# production
build/
dist/

# misc
.idea/
.happypack
.DS_Store

npm-debug.log*
yarn-debug.log*
yarn-error.log*

application.properties
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 配置无效几种情况

- 命令格式错误
- 在配置语句的前后面添加空格、Tab、注释等，会导致当前行的配置语句失效
- 配置语句对已经add、commit的文件无效（.gitignore只能忽略原来没有被追踪的文件，如已被纳入了版本管理中，则修改.gitignore是无效的。）
- 对于提交到了远程仓库的文件，解决方法参考：https://segmentfault.com/q/1010000000430426

第三种情况解决办法：删除本地缓存

. 可以改成不希望关联的文件

```bash
git rm -r --cached .
git add .

git commit -m 'update .gitignore'
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



## 如何快速关联/修改Git远程仓库地址

参考: https://www.cnblogs.com/youcong/p/10809085.html

删除本地仓库当前关联的无效远程地址，再为本地仓库添加新的远程仓库地址

```html
git remote -v //查看git对应的远程仓库地址
git remote rm origin //删除关联对应的远程仓库地址
git remote -v //查看是否删除成功，如果没有任何返回结果，表示OK
//重新关联git远程仓库地址
//这里需要注意，关联https的话部署秘钥是无效的，还是要通过账号密码去拉取代码。
//如果一定要关联https的路径的话，可以百度下缓存账号密码的方式
git remote add origin git@gitlab.xxx.cn:integrated-projects/korea.git 
// 测试是否可以连接到远程仓库，如果没有welcome，需要配置秘钥（参考上面的ssh key生成）
ssh -T git@gitlab.xxx.cn
git branch --set-upstream-to=origin/master master // 设置关联，这里是关联本地master分支和远程master分支
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

其实不仅仅上述这一种方式，还有如下几种方式:

直接修改本地仓库所关联的远程仓库的地址

```html
git remote  //查看远程仓库名称：origin 
git remote get-url origin //查看远程仓库地址
git remote set-url origin https://github.com/developers-youcong/Metronic_Template.git  ( 如果未设置ssh-key，此处仓库地址为 http://... 开头)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

修改 .git 配置文件

```html
cd .git  //进入.git目录
vim config  //修改config配置文件，快速找到remote "origin"下面的url并替换即可实现快速关联和修改
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





## git常见后悔药


撤销本地全部没有git add过的修改
git checkout -- .

使用库上文件覆盖本地修改（当然是指用本地库覆盖）git checkout file_name

回退掉某一次commit，回退方式是自动生成一个反向的commit，不会影响其他commmitgit revert commitID

将git库状态强制回退到某个节点号，这个节点号之后的commit全部丢失git reset --hard commitID

将远端库强制覆盖到本地，放弃本地全部修改git reset --hard origin 分支名

回退最近一次的commit，且该次commit所作的修改会退到没有被add的状态git reset --mixed HEAD~1

回退最近的一次commit，回退后该次commit所作的修改仍处于add过了的状态，可以通过git status查看状态，git reset --soft HEAD~1

回退最近一次的commit，回退的同时working tree也会被修改，也就是回退的这次的commit所做的修改都会消失git reset --hard HEAD~1


原文链接：https://blog.csdn.net/qq_15437667/article/details/52762196





# [删除untracked files](https://www.cnblogs.com/smallyi/p/6727664.html)

```bash
# 删除 untracked files
git clean -f
 
# 连 untracked 的目录也一起删掉
git clean -fd
 
# 连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）
git clean -xfd
 
# 在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



# 去除gitlab对分支的保护

有时候对master分支进行强制push【git push -f】时会报错

![img](http://blog.cdn.ionluo.cn/blog/20191223103325936.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

这是因为master分支受保护，处理方式是去掉这个保护。解决方法如下：
找到Branches----project setting --Protected Branches–点击master的unprotect即可。

![img](http://blog.cdn.ionluo.cn/blog/20191223103548786.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](http://blog.cdn.ionluo.cn/blog/20191223103613185.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](http://blog.cdn.ionluo.cn/blog/20191223103636686.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





# git的默认编辑器是nano，关于如何使用请看

http://www.vpser.net/manage/nano.html





# 解决 git pull/push 每次都要输入用户名密码的问题

原文链接：https://www.jianshu.com/p/5b81c9ce505c

**首先明确一点：出现这种问题的原因都是因为使用 http 的方式拉取代码才出现的，如下图所示：**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTdmZjQ2MWRkODQxNjNlYTMucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNTM5L2Zvcm1hdC93ZWJw.webp)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**第一种解决方法（windows）**

出现上面这种情况 **先按提示输入用户名和密码**，接着执行 **git config --global credential.helper store**
这句命令的意义是在本地生成包含 git 账号和密码的文件，具体操作如下图：

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTczNDg0ZDVlNjU3YjYwNDkucG5n.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**检验方式：C:\Users\你的电脑名; 这个文件夹(如下)下面是否能找到.git-credentials文件，如果文件的内容是有关你的gitlab的设置，格式为：http://{用户名}:{密码}@{git 网址}**

再次执行 git pull 操作就不需要再输入用户名和密码了

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LThkYTA0Nzc4MTQzZDEzOGUucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNTQ4L2Zvcm1hdC93ZWJw.webp)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**第二种解决方法（通用）**

切换 git 的拉取方式，将 http 改为 ssh 的方式
**1、查看clone 地址：git remote -v**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTU5MTJkZjdiYmM3OWEzMjgucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNjU1L2Zvcm1hdC93ZWJw.webp)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**2、移除 http 的方式：git remote rm origin**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTk5YjVlYzgyYTIzYWRiY2EucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNTU5L2Zvcm1hdC93ZWJw.webp)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

移除完之后再次查看拉取方式会发现为空，此时我们需要添加 ssh 的拉取方式


**3、换成 ssh方式： git remote add origin [git 地址]**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTI2YjJhOGU3ZDkxOTYzMGMucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNjk3L2Zvcm1hdC93ZWJw.webp)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



此时通过 git remote -v 查看会发现成功的从 http 拉取方式切换为 ssh 拉取方式了
大功告成！



# [Git删除暂存区或版本库中的文件](https://www.cnblogs.com/cposture/p/git.html)

https://www.cnblogs.com/cposture/p/git.html





Q&A:

\1. fatal: Out of memory, malloc failed (tried to allocate 2000000000 bytes)

![img](http://blog.cdn.ionluo.cn/blog/2020082319132032.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

原因：因用了其他博客的解决方案使用了下面命令导致，修改成正常大小即可。

git config --global http.postBuffer 20000000

解决：git config --global http.postBuffer 125000





## Git 查看两个版本的差异和修改了那些文件

查看两个提交版本id的修改记录差异
$ git diff [commit-id1] [commit-id2]

![img](http://blog.cdn.ionluo.cn/blog/20201030104949335.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

查看两个提交版本id修改了那些文件，可以使用
$ git diff [commit-id1] [commit-id2] --stat

![img](http://blog.cdn.ionluo.cn/blog/20201030105012221.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





## git clone是输错账号密码导致后面clone报错

## fatal: Authentication failed for 'https://gitlab.cyagen.cn/integrated-projects/cyagen-ap.git/

解决方法：删除git的window凭据

![img](http://blog.cdn.ionluo.cn/blog/20201208142852525.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](http://blog.cdn.ionluo.cn/blog/20201208143017180.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





## Git命令行显示中文

git默认是不能识别中文的。需要在终端修改能识别中文。

```bash
git config --global core.quotepath false
```

`core.quotepath`设为`false`的话，就不会对`0x80`以上的字符进行编码。中文显示正常。

![1609771436187](http://blog.cdn.ionluo.cn/blog/1609771436187.png)

