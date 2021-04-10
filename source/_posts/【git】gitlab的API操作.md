---
title: gitlab的API操作
description: '-'
tags:
  - Git
abbrlink: eafa4ec2
date: 2021-03-06 12:57:00
---



## 前言

**环境：**`python3.6` `python-gitlab`

```python
# requirements
certifi==2020.12.5
chardet==4.0.0
idna==2.10
numpy==1.19.5
pandas==1.1.5
python-dateutil==2.8.1
python-gitlab==2.5.0
pytz==2020.5
requests==2.25.1
six==1.15.0
urllib3==1.26.2
```

**文档：**

[gitlab API]( https://docs.gitlab.com/ee/api/)
[python-gitlab]( https://python-gitlab.readthedocs.io/en/latest/api-objects.html)

**生成访问秘钥：**

> 只有用这里生成的token才可以通过api访问gitlab 在gitlab的用户设置菜单里面生成token
>
> 我的token：`ho_XcxK1tWshbMwsrNxg`

![1610073459669](http://blog.cdn.ionluo.cn/blog/1610073459669.png)



## gitlab页面查看提交次数信息

选中项目，Repository  ---  Contributors 就可以选择查看提交次数了。但是除了用于查看比较提交次数貌似没有其他实际意义。所以就利用gitlab提供的[gitlab API]( https://docs.gitlab.com/ee/api/)进行编码获取。



![1610084693548](http://blog.cdn.ionluo.cn/blog/1610084693548.png)



## 利用Gitlab API统计提交详细信息

```python
# coding=utf-8
# author: ionluo

import time
import gitlab
import collections
import pandas


class GitlabAPI(object):

    def __init__(self, *args, **kwargs):
        self.gl = gitlab.Gitlab('https://gitlab.xxx.com/', private_token='ho_XcxK1tWshbMwsrNxg', api_version='4')
        # ssl_verify=False

    def get_all_group(self):
        """
        获取所有群组
        :return:
        """
        #return self.gl.groups.list(all=True)
        return self.gl.groups.list(owned=True, all=True)

    def get_all_project(self):
        """
        获取所有项目
        :return:
        """
        #return self.gl.groups.list(all=True)
        return self.gl.projects.list(owned=True, all=True)

    def get_all_user(self):
        """
        获取所有用户
        :return:
        """
        #return self.gl.groups.list(owned=True, all=True)
        return self.gl.users.list(all=True)

    def get_user_byname(self, username):
        """
        根据用户名获取用户数据
        :return:
        """
        return self.gl.users.list(username=username)[0]

    def get_contributors(self, user_email_list, start_time=None, end_time=None, branch='production'):
        """
        获取仓库production分支的代码贡献量
        user_email_list 本来一个就好，但是我在职期间用了多个账号，我需要统计多个账号的，所以弄成一个list
        :return:
        """
        datas = []
        projects = self.get_all_project()
        for project in projects:
            # 该项目的所有分支(这里我只需要统计production分支，如果需要统计所有分支可以遍历这个)
            # branches =  project.branches.list()
            query_parameters = {
                'since': start_time,
                'until': end_time,
                'ref_name': branch
            }
            commits = project.commits.list(all=True, query_parameters=query_parameters)
            for commit in commits:
                if commit.author_email not in user_email_list:
                    continue

                com = project.commits.get(commit.id)
                datas.append({
                    '分支': branch,  # 分支
                    '项目名': project.path_with_namespace, # 项目名
                    '开发者': com.author_name, # 提交人
                    '添加代码行数': com.stats["additions"],  # 添加代码行数
                    '删除代码行数': com.stats["deletions"],  # 删除代码行数
                    '提交总行数': com.stats["total"],  # 提交总行数
                })
        return datas

    def downloadCSV(self, datas, filename='gitlab.csv'):
        """
        下载数据成csv
        :return:
        """
        df = pandas.DataFrame(datas, columns=["项目名", "开发者", "分支", "添加代码行数", "删除代码行数", "提交总行数"])
        df.to_csv(filename, index=False, encoding="utf_8_sig")

if __name__ == "__main__":
    gl = GitlabAPI()

    my_email_list = ['xxx@gmail.com', 'xxx@163.com', 'xxx@qq.com']
    start_time = '2019-01-01'
    end_time = '2020-01-01'
    contributors = gl.get_contributors(my_email_list, start_time, end_time)
    gl.downloadCSV(contributors)
	
    # 下面是一些常用api，这里写下用法，具体可以查看文档
    # projects = gl.get_all_project()
    # for project in projects:
    #     print(project.name)

    # print('\n\n\n')
    # groups = gl.get_all_group()
    # for group in groups:
    #     print(group.name)
    #
    # print('\n\n\n')
    # users = gl.get_all_user()
    # for user in users:
    #     print(user.name)

```



## 利用git代码统计工具git_stats统计

### Ubuntu安装ruby（不建议）

**尽量使用下面的【linux的经典步骤安装ruby】，不建议使用apt-get的安装方式，后面会无限踩坑**

> 这里安装的版本比较低，不支持后面的操作，后面有附上升级的方法，但是自己实操过后, 遇到问题`ERROR: Failed to build gem native extension.`一直没有办法解决，安装`ruby-dev` 也不行，可能是高版本不能通过安装这个处理，或者是重装卸载了什么必须要的包。总之，**我这里最后是通过Linux的经典安装步骤安装的**。

```bash
# 查看是否安装ruby ( 安装了会提示版本信息，否则提示安装。)
ruby -v

# 安装ruby
sudo apt install ruby

# 查看版本
ruby -v
# ruby 2.3.1p112 (2016-04-26) [x86_64-linux-gnu]
```

如果版本显示小于`2.5`, 则需要升级ruby，否则报错`nokogiri requires Ruby version < 3.1.dev, >= 2.5.`

 1. 添加 PPA 源：

    ```bash
    add-apt-repository ppa:brightbox/ruby-ng
    apt-get update
    ```

 2. 删除旧版本：

    >  千万不要使用网上的一些什么 `sudo apt-get purge --auto-remove ruby`, `auto-remove` 并不智能，可能会删除一些系统文件之类的导致系统彻底完蛋。

    ```bash
    apt-get purge ruby
    ```

 3. 安装新版本：(我这里要2.6版本，可以自定义)

    ```bash
    apt-get install ruby2.6
    ```

 4. 安装后查看版本号：

    ```bash
    ruby -v
    ```



### linux的经典步骤安装ruby

进入官网http://www.ruby-lang.org/en/downloads/, 根据自己的系统进行选择下载（我这里下载了ruby 2.6）。

解压下载的安装包，切换到解压后的安装包目录，执行：

```bash
 # 切换到root用户，之后就不用一直sudo
sudo su
 
# 可以用 --prefix=/usr/local/ruby 指定安装位置，但是如果不知道如何修改环境变量就不要指定了
 ./configure

 # 安装并记录安装日志（方便后期卸载）
 # 卸载命令 xargs rm < make_ruby.log (未尝试)
 make && make install & >|tee make_ruby.log
 
 # 查看安装是否成功
 ruby -v
 
 # 如果没有显示对应的版本信息的话,需要设置环境变量（这里是一次性的生效，要生效环境变量要source激活）
 vim /etc/profile
 # 默认增加，之后保存退出
 export PATH=$PATH:/usr/local/ruby/bin
 # 激活环境
 source /etc/profile
```

 **意外情况处理：**我这里遇到一个问题就是一开始`ruby -v`还是之前`apt-get`安装的2.3版本，但是执行`sudo apt-get purge ruby` 又显示软件未安装。这里我直接暴力解决。`which ruby` 从环境变量中找到`ruby`的位置，然后手动删除掉它 ， `rm -f /usr/bin/ruby`.



### 使用gem安装git_stats

安装git_stats之前，先把gem的源替换成国内源，防止后面安装速度慢或者失败的问题

```bash
# 查看源
gem sources -l
# *** CURRENT SOURCES ***
#
# https://rubygems.org/

# 替换源
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

# 查看是否替换成功
gem sources -l
# *** CURRENT SOURCES ***
# 
# https://gems.ruby-china.com/
```

安装

```bash
sudo gem install git_stats
# 使用下面命令查看是否安装成功
git_stats help
```

> 安装这里我一开始使用ubuntu切换源安装的，报错：`ERROR: Failed to build gem native extension.`
>
> ![1610099326371](http://blog.cdn.ionluo.cn/blog/1610099326371.png)
>
> **解决：**`sudo apt-get install ruby-dev` 
>
> *（奇怪的是貌似官方源下载的可以用这个解决，但是PPA源这个就解决不了了。但是官方源版本太低，所以还是推荐linux经典步骤安装）*
>
> 不行的话可以检查下这几个是否有安装：`sudo apt-get install ruby ruby-dev gcc zlib1g-dev make`



### 生成文档

1. 切换到你要统计的git仓库根目录,即git项目根目录

2. 生成文档命令

   ```bash
   # -o 指定生成文件路径
   # --language 指定生成的语言，有zh_tw,de,en,es,pl,tr几个选择，默认是英语
   git_stats generate -o ./document --language zh_tw
   ```

   > **配置语言包支持中文**
   >
   > 1. 下载语言包  https://github.com/tomgi/git_stats
   >
   > 2. 需要查找本地`git_stats`包位置，默认安装情况下在
   >
   >    在`/usr/local/lib/ruby/gems/2.6.0/gems/git_stats-1.0.17`
   >
   >    也可以用命令搜索`sudo find / -name 'git_stats'`
   >
   >    包位置下的`config/locale`就是语言包文件了，把`github`上`config/locales`下的文件复制粘贴到到本地`config/locales`路径下，本地的`git_stats`就可以支持中文了。

3. 执行命令后的报告文档在 `-o` 指定的位置， 切换到该位置，打开`index.html`即可查看报告。

   ![1610187111063](http://blog.cdn.ionluo.cn/blog/1610187111063.png)



## 利用git代码统计工具GitStats统计

> 这里我试了是可以的，所以直接放原文内容：https://www.jianshu.com/p/eff9e8311035
>
> 但是不得不说，界面和功能都比`git_stats`差很多。

[GitStats](https://link.jianshu.com?t=http://gitstats.sourceforge.net/)为一个用于生成git仓库统计信息的工具。它检查仓库并生成历史数据的统计信息。当前仅输出HTML格式的统计信息。

### 功能描述

当前[GitStats](https://link.jianshu.com?t=http://gitstats.sourceforge.net/)所生成统计信息分为如下几类：

- 通用统计：总的文件数、行数、提交数、创建人数；
- 活动统计：按一天中的每个小时统计、按一周中的明天统计、按一星期中的每小时统计、按一年中的每月统计、按年和月统计、按年统计；
- 创建人统计：统计创建人信息，包括首次提交日期、最后提交日期等，按月统计提交人信息、按年统计提交人信息；
- 文件统计：按日期和扩展统计文件；
- 行数统计：按日期统计文件行数；

### 安装及使用

在Ubuntu下安装[GitStats](https://link.jianshu.com?t=http://gitstats.sourceforge.net/)只需要执行如下命令即可：

```bash
aptitude install gitstats
```

使用[GitStats](https://link.jianshu.com?t=http://gitstats.sourceforge.net/)的方法更简单：

```bash
gitstats repo gen
```

其中`repo`为仓库目录，`gen`为事先创建好的用于存储生成的`HTML`文档的目录。然后在`gen`目录下执行如下代码：

```bash
python -m SimpleHTTPServer 8080
```

既可以通过`http://localhost:8080`查看到生成的页面。

![1610110774728](http://blog.cdn.ionluo.cn/blog/1610110774728.png)

### 示例

官网给出的示例如下所示：

- [http://gitstats.sourceforge.net/examples/gitstats/](https://link.jianshu.com?t=http://gitstats.sourceforge.net/examples/gitstats/)





## 参考文献

[python调用gitlab](https://www.cnblogs.com/yxh168/articles/13675162.html)

[git代码统计工具git_stats](https://blog.csdn.net/think_of_/article/details/85045414)

[GitStats使用](https://www.jianshu.com/p/eff9e8311035) 