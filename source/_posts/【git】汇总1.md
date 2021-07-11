---
title: git使用问题汇总
description: '-'
tags:
  - Git
abbrlink: 6b82922c
date: 2021-03-17 21:02:32
---



## Git基础使用

1. 首先，我们这里选用`GitHub`作为代码存储仓库，`git bash`是我们常用的git操作窗口（cmd也可以，建议用git bash）。

   > GitHub：https://github.com/
   >
   > Git工具：https://git-scm.com/download/win

   注册GitHub账号并安装好git就可以在项目目录下右键打开git bash了。

   ![img](http://blog.cdn.ionluo.cn/blog/2018090817414519.png)

2. 设置/查看git用户名和邮箱：

   ```bash
   # 设置用户名和邮箱
   git config --global user.name "username"
   git config --global user.email "email"
   
   # 查看用户名和邮箱：
   git config user.name
   git config user.email
   ```

   

3. 接下来就要连接仓库了，这时候就需要SSH Key添加到项目owner（项目拥有者）的GitHub上。

   - **创建SSH Key**

     在项目主目录或者系统的目录（ `C:\Users\[账号，默认是Administrator]\.ssh` ）下，看看有没有`.ssh`目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果有，跳到下一步。如果没有，打开Shell（或者Git Bash），创建SSH Key：

     ```bash
     ssh-keygen -t rsa -C "[youremail@example].com"
     ```

     这里的`youremail@example.com`是GitHub的注册邮箱。如果一切顺利的话，可以在项目主目录或者（ `C:\Users\[账号，默认是Administrator]\.ssh` ）里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人，所以大胆地把`id_rsa.pub`告诉我吧。

4. owner根据你的**GitHub账号（注册邮箱）和id_rsa.pub**就能将你拉入项目组里面了，然后团队开发的环境就建立起来了。

5. 接下来描述一下团队开发方式。

    第一步：若本地没有从GitHub上下载过代码，那么请在一个文件夹下打开git bash ，直接使用下面命令拿取代码到当前目录：

   ```bash
   git clone git@github.com:Cheerfulion/scut-v2.0.git
   ```

   git clone 命令后面的 `git@github.com:Cheerfulion/scut-v2.0.git` 这么一串是项目的git地址，可以在任何一个git的托管平台项目上看到，而且不需要建立链接（即不需要添加你的ssh key到他的项目上）就能下载代码。GitHub的git地址显示位置如下：

   ![img](http://blog.cdn.ionluo.cn/blog/20180908181636512.png)

   第二步：在项目上创建新的分支，在新的分支上进行代码调整，调整完成后到第三步。

   第三步：提交前担心有冲突可以先git pull同步下代码，然后add并且commit提交本次修改。（git pull就是git fetch和git merge的集合）

   第四步：最后使用git push提交当前分支到远程仓库就完成。(注意：一般在正式项目上是不允许提交的当前分支是dev或者master分支的，这两个分支提交到远程也是对应远程的dev和master分支，而这两个分支通常都是要保证绝对无误的，也就是要测试部门测试通过才能合并。)



### Git 基础命令

**查看分支：git branch** 

![img](http://blog.cdn.ionluo.cn/blog/20180908183437297.png)

前面带*号的是远程分支。默认是查看本地分支，带上参数 -r是查看远程分支，-a是远程和本地的所有分支



**创建分支：git branch <new branch name> <come from where>**

```bash
# 示例1 从本地某一分支继承代码创建新分支
git branch new-branch-name dev

# 示例2 从远程某一分支继承代码创建分支
git branch new-branch-name origin/dev

# 示例3 不指定继承分支，默认继承当前所在分支
git branch new-branch-name
```



**切换分支：`git checkout <branch-name>`**

![img](http://blog.cdn.ionluo.cn/blog/20180908183926373.png)



**创建并切换分支：`git checkout -b <new_branch_name>`**

![img](http://blog.cdn.ionluo.cn/blog/20180913094929489.png)



**删除分支：`git branch -d <branch_name>`**

![img](http://blog.cdn.ionluo.cn/blog/20180913095207414.png)



**查看两个版本的差异：**

查看两个提交版本的修改记录差异：`git diff [commit-id1] [commit-id2]`

![img](http://blog.cdn.ionluo.cn/blog/20201030104949335.png)

查看两个提交版本修改文件差异：`git diff [commit-id1] [commit-id2] --stat`

![img](http://blog.cdn.ionluo.cn/blog/20201030105012221.png)





**若你的代码不是从远程克隆（git clone）下来的，提交代码到远程时就会因为没有关联到远程库而失败，关联远程库使用下面命令（后面的依旧是git地址，和git clone倒是很相似）：git remote add origin git@[server-name]:[path]/[repo-name].git；**

![img](http://blog.cdn.ionluo.cn/blog/2018091310222837.png)

之后就可以使用 **git push -u origin <origin_branch_name>** 使进行提交了。



备注：今天这样子执行遇到一个问题，如下：

![img](http://blog.cdn.ionluo.cn/blog/20181014165823625.png)

解决方法来自：https://jingyan.baidu.com/article/f3e34a12a25bc8f5ea65354a.html



今天（2019-06-06 23:38）又遇到了上面这个问题，无法使用git pull --rebase origin master 解决。

![img](http://blog.cdn.ionluo.cn/blog/20190606233930688.png)



解决方法：

![img](http://blog.cdn.ionluo.cn/blog/20190606234126886.png)

![img](http://blog.cdn.ionluo.cn/blog/20190606234220530.png)

![img](http://blog.cdn.ionluo.cn/blog/20190606234242231.png)

参考：https://blog.csdn.net/u012145252/article/details/80628451







### git三种回退类型和两种回退方式

**回退的三种类型：soft  mixed（默认）  hard**

在本地git会分三个区：工作区、暂存区、本地库。

- hard
  ①移动本地库HEAD指针

  ②重置暂存区

  ③重置工作区

  意思就是，回滚后，工作区和你回退版本的代码是一致的。

- soft
  ①移动本地库HEAD指针

  意思就是，回滚后，仅仅是把本地库的指针移动了，而暂存区和你本地的代码是没有做任何改变的。

- mixed
  ①移动本地库HEAD指针

  ②重置暂存区

  意思就是，回滚后，不仅移动了本地库的指针，同时暂存区的东西也没了。

  

**回退的两种方式  HEAD~n  commitID**

- HEAD~n

  回退到倒数第n个版本，如强制回退到上个版本是:

  ```bash
  git reset --hard HEAD~1
  ```

- commitID

  回退到指定版本

  ```bash
  # 通过 git log 或者 git reflog 拿到 commitID
  git log
  # commit 8a56a0d19766c237328c2941794769219053cf5f (HEAD -> master, origin/master)
  git reset --hard 8a56a0d19766c237328c2941794769219053cf5f
  ```

  

### window下配置.gitignore

- **在仓库目录下打开GitBash，输入命令 `touch .gitignore`**

- **常用的.gitignore配置**

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

- 保存，提交到本地仓库

  

**配置无效几种情况**

- 命令格式错误

- 在配置语句的前后面添加空格、Tab、注释等，会导致当前行的配置语句失效

- 配置语句对已经add、commit或者远程仓库存在的文件无效

  > .gitignore只能忽略原来没有被追踪的文件，如已被纳入了版本管理中，则修改.gitignore是无效的

- 对于提交到了远程仓库的文件，解决方法参考这里：https://segmentfault.com/q/1010000000430426

> 这里提供对于已经关联git的文件忽略的处理方法，比较难以理解的就是用上面最后一点文章里面提到的 “删除本地缓存” 的方法，即：
>
> ```bash
> # . 可以改成不希望关联的文件
> git rm -r --cached .
> git add .
> git commit -m 'update .gitignore'
> ```
>
> 我通常使用的最直接暴力的方法，备份文件，删除文件，重新提交并push到远程后（让文件不再被track），再复制备份文件回来
>
> ```bash
> mv node_modules /var/backup/node_modules
> rm -rf node_modules
> git add .
> git commit -m "rm node_modules"
> git push
> mv /var/backup/node_modules node_modules
> ```



### Git删除工作区或暂存区或版本库中的文件

> 原文链接：https://www.cnblogs.com/cposture/p/git.html, 略做补充修改。

**基础说明：**

  我们知道Git有三大区（**工作区、暂存区、版本库**）以及几个状态（**untracked、unstaged、uncommited**），下面只是简述下Git的大概工作流程，详细的可以参见本博客的其他有关Git的文章[【链接】。](http://www.cnblogs.com/cposture/category/642672.html) 

- 打开你的项目文件夹，除了隐藏的.git文件夹，其他项目文件位于的地方便是工作区，工作区的文件需要添加到Git的暂存区（git add），随后再提交到Git的版本库（git commit）。

- 首次新建的文件都是untracked状态（未跟踪），此时需要git add到暂存区，Git便会在暂存区中生成一个该文件的索引，文件此时处于uncommited状态，需要git commit生成版本库。添加到了版本库之后，再对文件进行修改，那么文件的状态会变为unstaged状态。

  简单的认识了Git的工作流程，接下来便可以看看如何删除错误添加到暂存区或版本库里的文件了！



**删除修改错误的工作区文件**

```bash
# 单个文件没有add过的修改撤销
git checkout  <filenpath>
# 当前目录下的所有文件没有add过的修改撤销
git checkout .
```



**删除错误添加到暂存区的文件**

有时你在工作区新建了文件TestFile，并且已经将它添加到了暂存区，git会告知，现有有一个文件未提交到版本库，如下图：

![img](https://images2015.cnblogs.com/blog/695731/201510/695731-20151017163046179-733384065.png)

 

- 仅仅删除暂存区里的文件  

   `git rm --cache 文件名`

  > 上面的命令仅仅删除暂存区的文件而已，不会影响工作区的文件，此时输入`git status`，git会告知有一个未跟踪的文件。

 

- 删除暂存区和工作区的文件

  `git rm -f 文件名`

  

 **删除错误提交的commit**

  有时，不仅添加到了暂存区，而且commit到了版本库，这个时候就不能使用`git rm`了，需要使用`git reset`命令。

  错误提交到了版本库，此时无论工作区、暂存区，还是版本库，这三者的内容都是一样的，所以在这种情况下，只是删除了工作区和暂存区的文件，下一次用该版本库回滚那个误添加的文件还会重新生成。

  这个时候，我们必须撤销版本库的修改才能解决问题！

  git reset有三个选项，`--hard、--mixed、--soft`。

```bash
# 仅仅只是撤销已提交的版本库，不会修改暂存区和工作区
git reset --soft 版本库ID
```

 

```bash
# 仅仅只是撤销已提交的版本库和暂存区，不会修改工作区
git reset --mixed 版本库ID
```

 

```bash
# 彻底将工作区、暂存区和版本库记录恢复到指定的版本库
git reset --hard 版本库ID
```

  那我们到底应该用哪个选项好呢？

  （1）如果你是在提交了后，对工作区的代码做了修改，并且想保留这些修改，那么可以使用git reset --mixed 版本库ID，注意这个版本库ID应该不是你刚刚提交的版本库ID，而是**刚刚提交版本库的上一个版本库**。如下图：

  （2）如果不想保留这些修改，可以直接使用彻底的恢复命令，git reset --hard 版本库ID。

  （3）为什么不使用--soft呢，因为它只是恢复了版本库，**暂存区仍然存在你错误提交的文件索引**，还需要进一步使用上一节的删除错误添加到暂存区的文件，详细见上文。

![img](https://images2015.cnblogs.com/blog/695731/201510/695731-20151017164205272-1808101337.png)



### nano如何使用?

git的默认编辑器是nano，使用方法见下文。

http://www.vpser.net/manage/nano.html



## 在gitlab创建项目仓库后提示记录

```bash
## Git global setup
# git config --global user.name "【你的账号】"
# git config --global user.email "【你的邮箱】"

# Create a new repository
git clone git@gitlab.xxx.com:projects/xxx.git
cd xxx
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

# Existing folder
cd existing_folder
git init
git remote add origin git@gitlab.xxx.com:projects/xxx.git
git add .
git commit -m "Initial commit"
git push -u origin master

# Existing Git repository
cd existing_repo
git remote rename origin old-origin
git remote add origin git@gitlab.xxx.com:projects/xxx.git
git push -u origin --all
git push -u origin --tags
```









## Git使用场景处理方法

### [删除untracked files(略)](https://www.cnblogs.com/smallyi/p/6727664.html)

```bash
## 删除 untracked files
git clean -f
 
## 连 untracked 的目录也一起删掉
git clean -fd
 
# 连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）
git clean -xfd
 
## 在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd
```

> 危险操作，了解即可



### 放弃本地全部修改，将远端库强制覆盖到本地

```bash
git fetch --all
git reset --hard origin/master (这里master要修改为对应的分支名)
git pull (解决可能的冲突)
```

>有时候同一个分支，远程的和本地的都被修改的面目全非了，这时候想要把本地的替换成远程的用该方式非常方便



### 忘记切换分支,误将代码提交到了别的远程分支

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



### 如何快速关联/修改Git远程仓库地址

1. **方法一（最推荐）: 把修改旧Git地址，添加新的Git远程仓库地址**

   ```bash
   git remote rename origin old-origin
   git remote add origin git@gitlab.xxx.cn:xxx/xxx.git 
   git push -u origin --all
   git push -u origin --tags
   # 查看结果
   git remote -v
   ```

   

2. **方法二：删除本地仓库当前关联的无效远程地址，再为本地仓库添加新的远程仓库地址**

   ```bash
   # 查看git对应的远程仓库地址
   git remote -v
   # 删除关联对应的远程仓库地址
   git remote rm origin
   # 查看是否删除成功，如果没有任何返回结果，表示OK
   git remote -v
   # 重新关联git远程仓库地址
   # 这里需要注意，关联https的话部署秘钥是无效的，还是要通过账号密码去拉取代码。
   # 如果一定要关联https的路径的话，可以百度下缓存账号密码的方式
   git remote add origin git@gitlab.xxx.cn:xxx/xxx.git 
   # 测试是否可以连接到远程仓库，如果没有welcome，需要配置秘钥（参考上面的ssh key生成）
   ssh -T git@gitlab.xxx.cn
   # 设置关联，这里是关联本地master分支和远程master分支
   git branch --set-upstream-to=origin/master master
   ```

   

3. **方法三：直接修改本地仓库所关联的远程仓库的地址**

   ```bash
   # 查看远程仓库名称：origin 
   git remote
   # 查看远程仓库地址
   git remote get-url origin
   #  如果未设置ssh-key，此处仓库地址为 http://... 开头
   git remote set-url origin https://github.com/developers-youcong/Metronic_Template.git
   ```

   

4. **方法四：修改 .git 配置文件**

   ```bash
   # 进入.git目录
   cd .git
   # 修改config配置文件，快速找到remote "origin"下面的url并替换即可实现快速关联和修改
   vim config
   ```

   



### 撤回远程仓库里面的提交记录(略)

- git log 查看要回退到的版本号

- git reset [--soft| --mixed | --hard] <版本号>

- git push --force 

  > 因gitlab的提交历史在本地的前面,需要加上--force强制推送,需要注意的是对于保护分支依旧会报错,这时需要管理员暂时取消分支保护。
  >
  > 当然这种做法不可取，但是如果你不小心在远程仓库的提交信息里面写了老板的坏话的话还是用该方法保命吧，哈！一般来说，分支的合并会有专人审核，所以基本上该方法不会使用到，但是了解总归没错的。





### 去除gitlab对分支的保护(略)

有时候对master分支进行强制push【git push -f】时会报错

![img](http://blog.cdn.ionluo.cn/blog/20191223103325936.png)

这是因为master分支受保护，处理方式是去掉这个保护。解决方法如下：
找到Branches----project setting --Protected Branches–点击master的unprotect即可。

![img](http://blog.cdn.ionluo.cn/blog/20191223103548786.png)

![img](http://blog.cdn.ionluo.cn/blog/20191223103613185.png)

![img](http://blog.cdn.ionluo.cn/blog/20191223103636686.png)



### [解决 git pull/push 每次都要输入用户名密码的问题](https://www.jianshu.com/p/5b81c9ce505c)

**首先明确一点：出现这种问题的原因都是因为使用 http 的方式拉取代码才出现的，如下图所示：**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTdmZjQ2MWRkODQxNjNlYTMucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNTM5L2Zvcm1hdC93ZWJw.webp)

**第一种解决方法（windows）**

出现上面这种情况 **先按提示输入用户名和密码**，接着执行 **git config --global credential.helper store**
这句命令的意义是在本地生成包含 git 账号和密码的文件，具体操作如下图：

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTczNDg0ZDVlNjU3YjYwNDkucG5n.png)

**检验方式：C:\Users\你的电脑名; 这个文件夹(如下)下面是否能找到.git-credentials文件，如果文件的内容是有关你的gitlab的设置，格式为：http://{用户名}:{密码}@{git 网址}**

再次执行 git pull 操作就不需要再输入用户名和密码了

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LThkYTA0Nzc4MTQzZDEzOGUucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNTQ4L2Zvcm1hdC93ZWJw.webp)



**第二种解决方法（通用）**

切换 git 的拉取方式，将 http 改为 ssh 的方式
**1、查看clone 地址：git remote -v**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTU5MTJkZjdiYmM3OWEzMjgucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNjU1L2Zvcm1hdC93ZWJw.webp)



**2、移除 http 的方式：git remote rm origin**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTk5YjVlYzgyYTIzYWRiY2EucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNTU5L2Zvcm1hdC93ZWJw.webp)

移除完之后再次查看拉取方式会发现为空，此时我们需要添加 ssh 的拉取方式


**3、换成 ssh方式： git remote add origin [git 地址]**

![img](http://blog.cdn.ionluo.cn/blog/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDAxNjY0LTI2YjJhOGU3ZDkxOTYzMGMucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvNjk3L2Zvcm1hdC93ZWJw.webp)



此时通过 git remote -v 查看会发现成功的从 http 拉取方式切换为 ssh 拉取方式了
大功告成！





### Git命令行中文乱码

git默认是不能识别中文的。需要在终端修改能识别中文。

```bash
git config --global core.quotepath false
```

`core.quotepath`设为`false`的话，就不会对`0x80`以上的字符进行编码。中文显示正常。

![1609771436187](http://blog.cdn.ionluo.cn/blog/1609771436187.png)



### 本地Git仓库关联多个远程仓库的两种方法

> 原文链接：https://zhuanlan.zhihu.com/p/82388563



**背景**

通常情况下，一个本地Git仓库对应一个远程仓库，每次`pull`和`push`仅涉及本地仓库和该远程仓库的同步；然而，在一些情况下，一个本地仓库需要同时关联多个远程仓库，比如：同时将一个项目发布在[Github](https://link.zhihu.com/?target=https%3A//github.com/)和[Coding](https://link.zhihu.com/?target=https%3A//coding.net/)上，以兼顾国内外的访客。

那么，如何让一个本地仓库同时关联多个远程仓库呢？



**方法1：每次`push`、`pull`时需分开操作**

首先，查看本地仓库所关联的远程仓库：(假定最初仅关联了一个远程仓库)

```bash
$ git remote -v
origin  git@github.com:keithnull/keithnull.github.io.git (fetch)
origin  git@github.com:keithnull/keithnull.github.io.git (push)
```

然后，用`git remote add <name> <url>`添加一个远程仓库，其中`name`可以任意指定（对应上面的`origin`部分），比如：

```bash
$ git remote add coding.net git@git.coding.net:KeithNull/keithnull.github.io.git
```

再次查看本地仓库所关联的远程仓库，可以发现成功关联了两个远程仓库：

```bash
$ git remote -v
coding.net      git@git.coding.net:KeithNull/keithnull.github.io.git (fetch)
coding.net      git@git.coding.net:KeithNull/keithnull.github.io.git (push)
origin  git@github.com:keithnull/keithnull.github.io.git (fetch)
origin  git@github.com:keithnull/keithnull.github.io.git (push)
```

此后，若需进行`push`操作，则需要指定目标仓库，`git push <repo> <branch>`，对这两个远程仓库分别操作：

```bash
$ git push origin master
$ git push coding.net master
```

同理，`pull`操作也需要指定从哪个远程仓库拉取，`git pull <repo> <branch>`，从这两个仓库中选择其一:

```bash
$ git pull origin master
$ git pull coding.net master
```



**方法2：`push`和`pull`无需额外操作** 

在方法1中，由于我们添加了多个远程仓库，在`push`和`pull`时便面临了仓库的选择问题。诚然如此较为严谨，但是在许多情况下，我们只需要保持远程仓库完全一致，而不需要进行区分，因而这样的区分便显得有些“多余”。

同样地，先查看已有的远程仓库：(假定最初仅关联了一个远程仓库)

```bash
$ git remote -v
origin  git@github.com:keithnull/keithnull.github.io.git (fetch)
origin  git@github.com:keithnull/keithnull.github.io.git (push)
```

然后，**不额外添加远程仓库，而是给现有的远程仓库添加额外的URL**。使用`git remote set-url -add <name> <url>`，给已有的名为`name`的远程仓库添加一个远程地址，比如：

```bash
$ git remote set-url --add origin git@git.coding.net:KeithNull/keithnull.github.io.git
```

再次查看所关联的远程仓库：

```bash
$ git remote -v
origin  git@github.com:keithnull/keithnull.github.io.git (fetch)
origin  git@github.com:keithnull/keithnull.github.io.git (push)
origin  git@git.coding.net:KeithNull/keithnull.github.io.git (push)
```

可以看到，我们并没有如方法1一般增加远程仓库的数目，而是给一个远程仓库赋予了多个地址（或者准确地说，多个用于`push`的地址）。

因此，这样设置后的`push` 和`pull`操作与最初的操作完全一致，不需要进行调整。



**总结**

以上是给一个本地仓库关联多个远程仓库的两种方法，二者各有优劣，不过出于简便考虑，我最终采用了方法2。

此外，上述内容中涉及到的Git指令略去了许多不常用的参数，如需更加详细的说明，可以查阅[Git文档](https://link.zhihu.com/?target=https%3A//git-scm.com/docs/git-remote)，或者直接在命令行运行`git remote --help`。



## 报错处理

1. **`fatal: Authentication failed for 'https://xxx.git/`**

   报错原因：使用https验证的git仓库，同步代码需要输入账号密码，而window系统输入一次账密后是保存在window凭据中的，下次执行拉取代码会自行使用Window中的凭据

   解决方法：删除git的window凭据，重新拉取输入正确的账号密码

   ![img](http://blog.cdn.ionluo.cn/blog/20201208142852525.png)

   ![img](http://blog.cdn.ionluo.cn/blog/20201208143017180.png)







2. **`fatal: Out of memory, malloc failed (tried to allocate 2000000000 bytes)`**

![img](http://blog.cdn.ionluo.cn/blog/2020082319132032.png)

​	原因：因用了其他博客的解决方案使用了下面命令导致，修改成正常大小即可。

​	解决：`git config --global http.postBuffer 125000`





3. **`error: object file .git/objects/xx/xxxxx is empty`**

![img](http://blog.cdn.ionluo.cn/blog/20191020145826297.png)

- 备份.git目录

```bash
cp -a .git .git-old
```

- 根据修复提示删除空对象文件。根据最早的空文件提示也删除那个文件。

```bash
git fsck --full
rm .git/objects/19/b45524e14258043923c2bc4d336269f1abc67f
```

- 删除后再次查看修复提示,说明Head commit无效。

```bash
git fsck --full
// 段错误 (核心已转储)% (3/256)   
git reflog
// fatal: bad object HEAD
```

- 找到当前分支Head的前两条数据。

```bash
tail -n 2 .git/logs/refs/heads/master
//注意，如果不是master分支，则.git/logs/refs/heads/branchName

// a6f48ebd79ef39b2c43e5810fc96e95cc0867389 ed41eac57be18e8d112ca5bb898dd77c3b120321 
```

- 第一条Head是无效的。我们需要确认第二条是我们的上次失败的commit的前一个提交。

```bash
git show ed41eac57be18e8d112ca5bb898dd77c3b120321 
//输出信息为日志详细信息。
```

- 设置确认的commit为HEAD commit

```bash
git update-ref HEAD ed41eac57be18e8d112ca5bb898dd77c3b120321 
```

- 再次使用 git status 和 git reflog 看看功能是否正常了。如果还不行。重启下电脑。

参考：[https://git-scm.com/book/zh/v1/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E7%BB%B4%E6%8A%A4%E5%8F%8A%E6%95%B0%E6%8D%AE%E6%81%A2%E5%A4%8D](https://git-scm.com/book/zh/v1/Git-内部原理-维护及数据恢复)

https://stackoverflow.com/questions/11706215/how-to-fix-git-error-object-file-is-empty



**4. `There are too many unreachable loose objects; run 'git prune' to remove them.`**

> 此处是git仓库太大导致的报错，关于git的清理（gc），目前了解不多，可以参考下面方式处理，代深入了解后再整理归纳说明。

```bash
(web) ion@ubuntu:~/projects/web/app/static$ git pull
Warning: the RSA host key for 'gitlab.xxx.net' differs from the key for the IP address '172.xx.xx.xx'
Offending key for IP in /home/ion/.ssh/known_hosts:2
Matching host key in /home/ion/.ssh/known_hosts:4
Are you sure you want to continue connecting (yes/no)? yes
自动在后台执行仓库打包以求最佳性能。
手工维护参见 "git help gc"。
error: 最后一次 gc 操作报告如下信息。请检查原因并删除 .git/gc.log。
在该文件被删除之前，自动清理将不会执行。

warning: There are too many unreachable loose objects; run 'git prune' to remove them.

Already up-to-date.
(web) ion@ubuntu:~/projects/web/app/static$ git count-objects
193628 objects, 850024 kilobytes
(web) ion@ubuntu:~/projects/web/app/static$ git gc --prune=now 
对象计数中: 244216, 完成.
Delta compression using up to 4 threads.
压缩对象中: 100% (97590/97590), 完成.
写入对象中: 100% (244216/244216), 完成.
Total 244216 (delta 160722), reused 219325 (delta 142055)
正在删除重复对象: 100% (256/256), 完成.
(web) ion@ubuntu:~/projects/web/app/static$ git count-objects
0 objects, 0 kilobytes
```




