---
title: npm使用记录
description: '-'
tags:
  - WEB开发
  - Web后端
  - NodeJs
abbrlink: acd31464
date: 2021-03-31 20:21:37
---



## npm 缩写写法说明

`i` 是`install` 的简写

`-s`就是`–save`的简写

`-d`就是`–save-dev`的简写

> 默认 `npm install` 安装的是所有包，相当于 `npm install -s` 加 `npm install -d`



## npm install出现错误

1. 需要排除是否是版本问题，这个可以看报错信息。如果是版本过低，可以升级下node和npm。

2. 可能是安装的错误文件还在`node_modules`文件夹内，使用命令`npm cache clean --force`清除该文件夹并使用命令`npm install`重新安装。

3. 需要排查是否`npm源`的问题，原始源可能存在下载部分包错误，可以使用`nrm`来切换源继续第二步。

4. 上面基本可以解决我遇到的大多数问题，如果问题依旧无法解决，使用淘宝的`cnpm`或者`yarn`工具代替`npm`安装。

   ```bash
   npm install cnpm -g
   cnpm install
   ```

> 有的时候也需要删除package-lock.json文件



## 发布个人模块到NPM站点

1. 前往 https://www.npmjs.com/signup 注册账号

   ```bash
   # 1. 进入要发布的工程目录(这里我随意创建一个演示项目，如果项目已存在可以直接跳到第3步。)
   mkdir ionluo-test
   # 2. 初始化项目
   npm init -y
   # 3. 添加用户，输入在NPM官网注册的账户和密码
   # 如果已经添加过了，或者有多个账号，使用npm login登录指定用户账号,通过npm whoami 查看当前登录账号
   npm adduser
   # 4. 发布项目到NPM站点
   npm publish
   ```

   > 注意：
   >
   > ​	1. 如果使用淘宝或者其他源会报错`npm ERR! 403 403 Forbidden - PUT https://registry.npm.taobao.org/myprojects - [no_perms] Private mode enable, only admin can publish this module`，需要切回官方源（切换后别忘了重新登录）。
   >
   >  **这里推荐使用nrm 管理源：**
   >
   >  `nrm ls` ： 列出所有源 
   >
   >  `nrm use npm` 使用npm源
   >
   > **或者直接使用原始命令切换回npm源：**
   >
   > `npm config set registry=http://registry.npmjs.org`
   >
   > 2. 报错`npm ERR! 403 In most cases, you or one of your dependencies are requesting a package version that is forbidden by your security policy.`
   >
   >    百度搜索了很久没有找到问题，根据提示也行不通，后来发现是邮箱没有验证，去注册邮箱验证下就好了。
   >
   >    https://stackoverflow.com/questions/62830477/how-to-debug-npm-err-403-in-most-cases-you-or-one-of-your-dependencies-are-re
   >
   >    ![1609478246532](http://blog.cdn.ionluo.cn/blog/1609478246532.png)

2. 到NPM官网查看项目是否发布成功

   ![1609478452375](http://blog.cdn.ionluo.cn/blog/1609478452375.png)

   https://www.npmjs.com/search?q=ionluo-test
   
3. 扩展：[npm包有何规范？](https://segmentfault.com/q/1010000022977022)

   npm 没有强制要求。

   主要知道三点即可，需要`package.json`文件，`package.json`里面用`main`标记入口文件，使用`.npmignore`屏蔽掉不需要发布的文件。

   一般node项目目录规范：

   - `src`：源码源文件。
   - `lib`：依赖文件（没通过 npm，直接下载源码的那种）。
   - `node_modules`：npm 依赖文件。
   - `bin`：二进制可执行文件。
   - `tests`：单元测试或集成测试文件。
   - `docs`：文档、开发手册。
   - `examples`：示例代码或项目。
   - `build`：构建时所需文件。
   - `dist`：打包后的输出目录。