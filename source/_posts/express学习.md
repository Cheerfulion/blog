---
title: express学习
description: '-'
tags:
  - WEB开发
  - Web后端
  - NodeJs
abbrlink: 30df1e17
date: 2021-03-29 22:29:03
---





## Express框架简介



![1595677903075](http://blog.cdn.ionluo.cn/blog/1595677903075.png)

![1595678357322](http://blog.cdn.ionluo.cn/blog/1595678357322.png)

![1595678374027](http://blog.cdn.ionluo.cn/blog/1595678374027.png)



![1595678389646](http://blog.cdn.ionluo.cn/blog/1595678389646.png)



## 中间件

![1595679133946](http://blog.cdn.ionluo.cn/blog/1595679133946.png)





![1595679209599](http://blog.cdn.ionluo.cn/blog/1595679209599.png)



![1595679290736](http://blog.cdn.ionluo.cn/blog/1595679290736.png)



![1595679474809](http://blog.cdn.ionluo.cn/blog/1595679474809.png)





![1595679807103](http://blog.cdn.ionluo.cn/blog/1595679807103.png)





![1595681069012](http://blog.cdn.ionluo.cn/blog/1595681069012.png)





中间键是有顺序的，从上到下

![1595748553068](http://blog.cdn.ionluo.cn/blog/1595748553068.png)



## 构建模块化路由

![1595753364409](http://blog.cdn.ionluo.cn/blog/1595753364409.png)



![1595767197577](http://blog.cdn.ionluo.cn/blog/1595767197577.png)



## Express请求处理



![1595767616613](http://blog.cdn.ionluo.cn/blog/1595767616613.png)

![1595767674511](http://blog.cdn.ionluo.cn/blog/1595767674511.png)

*// 这里通过express的第三方模块body-parser对post请求的参数进行解析。*

*// extended为true时，使用第三方模块qs对参数解析，false时使用nodejs的系统模块querystring进行解析*

*// 正常情况下，使用querystring对参数解析即可，这也是官方的推荐。*

app.**use**(bodyParser.**urlencoded**({extended: false}));



![1595768336055](http://blog.cdn.ionluo.cn/blog/1595768336055.png)



## 对静态资源的处理





![1595768543489](http://blog.cdn.ionluo.cn/blog/1595768543489.png)

![1595768732454](http://blog.cdn.ionluo.cn/blog/1595768732454.png)

> app.**use**('/static', express.**static**(path.**join**(__dirname, 'public')));

上面这样子写的话静态文件就是/static路径了，举个例子：

http://localhost:3000/static/logo.png



## 对app.use()方法的回调函数进行操作



![1595768220015](http://blog.cdn.ionluo.cn/blog/1595768220015.png)



## express-art-template模板引擎



![1595775447334](http://blog.cdn.ionluo.cn/blog/1595775447334.png)

> res.render('模板名称', '模板数据')  





![1595775613661](http://blog.cdn.ionluo.cn/blog/1595775613661.png)





## app.locals对象

存储在这里的数据默认可以在所有模板中获取到，不需要再res.render里面添加



![1595775788572](http://blog.cdn.ionluo.cn/blog/1595775788572.png)





## 密码加密算法



![1596252240622](http://blog.cdn.ionluo.cn/blog/1596252240622.png)



安装环境：

```shell
npm instal bcrypt
npm install -g node-gyp
npm install --global --production windows-build-tools
```



![1596110235504](http://blog.cdn.ionluo.cn/blog/1596110235504.png)



## cookie 和 session



![1596260815827](http://blog.cdn.ionluo.cn/blog/1596260815827.png)

![1596260952370](http://blog.cdn.ionluo.cn/blog/1596260952370.png)





## Joi验证参数

```shell
npm install joi
```



![1596272355704](http://blog.cdn.ionluo.cn/blog/1596272355704.png)

> 默认的参数都是可选的，需要加上required()。另外如果参数对象有规则里面没有定义的key会验证失败。



> 这里的joi的版本太高无法运行，报错  Joi.validate is a not function
>
> 选择版本安装 npm install joi@14.3.1



## 配置chrome浏览器允许安装第三方插件

方法一：

>  chrome://flags/#extensions-on-chrome-urls

![1596279432255](http://blog.cdn.ionluo.cn/blog/1596279432255.png)

重启后，鼠标拖动第三方的`.crx`文件到扩展程序页面就可以显示“拖放以安装”

> 注意：修改后会提示不安全，可以忽略



方法二：

将`.crx`文件修改后缀为`zip`之类的压缩文件后缀，通过解压工具解压成文件夹。在扩展程序页面打开开发者模式，选择“加载已解压的扩展程序”，选中前面解压的文件夹即可导入。



![1596279855179](http://blog.cdn.ionluo.cn/blog/1596279855179.png)







### formidable解析表单 - 文件上传

![1596873065845](http://blog.cdn.ionluo.cn/blog/1596873065845.png)

### 文件读取FileReader

![1596875098423](http://blog.cdn.ionluo.cn/blog/1596875098423.png)

## 分页模块

![1597246414368](http://blog.cdn.ionluo.cn/blog/1597246414368.png)

page: 第几页

size: 一页展示的条数

display： 显示的页码数（比如如果有1000页，那并不需要显示1000的页码，页面显示不了）



![1597246573181](http://blog.cdn.ionluo.cn/blog/1597246573181.png)





## mongoDB数据库添加账号

需要建立超级管理员账号，再创建普通账号。超级管理员账号对应admin数据库，拥有全部的权限。普通账号对应与具体的数据库。

![1597247862502](http://blog.cdn.ionluo.cn/blog/1597247862502.png)

![1597249557919](http://blog.cdn.ionluo.cn/blog/1597249557919.png)





创建超级管理员账号：

![1597248229847](http://blog.cdn.ionluo.cn/blog/1597248229847.png)

```shell
db.createUser({user: '用户名', pwd: "用户密码", roles: [‘用户角色(用户组，系统有内置的值，如root即超级管理员)’]})

# 这里的用户名是root, 密码是root123456
# roles:  值为root指超级管理员
#         值为readWrite指拥有该数据库的读写权限
```



给blog数据库创建普通用户账号：

![1597248480530](http://blog.cdn.ionluo.cn/blog/1597248480530.png)

```shell
db.createUser({user: 'itcast', pwd: 'itcast', roles: ['readWrite']})
```





卸载mongodb服务：

![1597248725448](http://blog.cdn.ionluo.cn/blog/1597248725448.png)



创建并启动mongodb服务：

![1597249181883](http://blog.cdn.ionluo.cn/blog/1597249181883.png)

```shell
mongod --logpath="[日志输出位置]C:\Program Files\MongoDB\Server\4.2\log\mongod.log" --dbpath="[数据库存储目录]C:\Program Files\MongoDB\Server\4.2\data" --install --auth
```



这里的两个目录都是安装MongoDB的默认路径，`--auth`表明必须校验身份才可以登录。



验证数据库操作需要权限：

![1597249430737](http://blog.cdn.ionluo.cn/blog/1597249430737.png)

上面运行了mongoose操作数据库的程序，程序中连接数据库没有加上账号密码，显示是数据库连接成功，但是进行数据库操作的时候就报错了。





在项目中使用账号密码连接数据库：

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://itcast:itcast@localhost:27017/blog', { useNewUrlParser: true, useUnifiedTopology: true }).then(() => {
    console.log('数据库连接成功！')
}).catch(err => console.log("数据库连接失败！", err));
```



## 如何区分开发环境和生产环境

![1597462531287](http://blog.cdn.ionluo.cn/blog/1597462531287.png)

注意设置环境变量后需要重启cmd才可以获取到新加入的系统环境变量



