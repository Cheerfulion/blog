---
title: 【nodejs】使用swagger生成api文档
tags:
  - nodejs
  - express
abbrlink: 186bff9a
date: 2021-07-15 16:32:27
---



## 前言

目前我们很多项目采用的都是新建一个markdown，写文档，每次改接口，就打开旧的markdown=>编辑=>保存=>复制并发布到项目wiki。

> 这种方式面临的问题：
>
> 1. 撰写方式没有具体的规范，每个后端都有自己的写法，不利于前端理解、项目交接。
> 2. 由于API文档往往涉及到复杂的参数说明、返回值说明，markdown显示复杂的文档其实并不美观、可读性不高。
> 3. 接口越来越多，markdown不能自动分类生成导航、自动折叠，前端找接口很麻烦，后端也难于维护。
> 4. 改了接口不止要改markdown，还要复制到wiki，容易忘记或者不同步。
> 5. 不能根据API自动生成Mock Server，在后端做好接口开发前，前端需要自己写假数据开发，费时费力。

> 以上问题总结起来，解决方案需要满足以下几点：
>
> 1. 一个完整、统一的文档撰写规范
> 2. 易于阅读的展示方式
> 3. 便于维护、不需要多处修改的撰写方式
> 4. 能够根据API文档生成Mock Server，以便于前端开发

Swagger Editor可以解决1、3、4，不止具有语法提示、语法检测，还支持定义对象，一处定义多处使用，减少重复编写，写好后可以一键生成Mock Server，而且支持生成多种语言的：





## 说明

Swagger是一个编写API文档的套件组合，而不是一个单一的工具。具体可以在官网看到。

 Swagger可以实现很多功能，最基础、常用的有：

1. API文档撰写 —— [Swagger Editor](https://editor.swagger.io/)
2. API文档的显示 —— [Swagger UI](https://github.com/swagger-api/swagger-ui.git)
3. 生成Mock服务 ——  [Swagger Editor](https://editor.swagger.io/)

4. 描述文件编写规则见：

   https://swagger.io/docs/specification/2-0/describing-request-body/





## 在express中使用

### 方法1：在代码中定义接口描述

1. 安装nodejs和express（略）

2. 使用express生成项目结构

   ```bash
   express
   ```

3. 安装依赖包

   ```bash
   npm install -S swagger-jsdoc@4.0.0 swagger-ui-express@4.1.4
   ```

4. 设置swagger的访问入口

   ```bash
   vim app.js
   ```

   加入下面代码

   ```javascript
   const swaggerJsdoc = require('swagger-jsdoc')
   const swaggerui = require('swagger-ui-express')
   
   const options = {
     definition: {
       // openapi: '3.0.0',
       swagger: '2.0',
       info: {
         title: 'ionluo站点的API文档',
         version: '1.0.0',
         description: 'This is a website backend api document making by swagger-ui ',
         contact: {
           // name: 'ionluo',
           email: 'xxxx@qq.com'
         },
         license: {
           name: 'Apache 2.0',
           url: 'http://www.apache.org/licenses/LICENSE-2.0.html'
         }
       },
       externalDocs: {
         description: 'Find out more about Swagger',
         url: 'http://swagger.io'
       },
       host: app.get('env') === 'development' ? 'localhost:8080' : 'wwww.ionluo.cn',
       basePath: '/api/v1',
       schemes: [
         'http',
         'https'
       ],
       consumes: [
         'application/json'
       ],
       produces: [
         'application/json'
       ],
       securityDefinitions: {
         token: {
           type: 'apiKey',
           name: 'token',
           in: 'header'
         }
       },
       tags: [
         {
           name: 'user',
           description: '关于用户的路由，登录，注册，获取当前用户等等'
         },
         {
           name: 'post',
           description: '关于文章的一些操作信息'
         },
         {
           name: 'file',
           description: '关于文件上传的一些操作信息'
         }
       ]
     },
     apis: [
       path.join(__dirname, 'routers/*.js'),
       path.join(__dirname, 'models/*.js')
     ]
   }
   const swaggerSpec = swaggerJsdoc(options)
   app.use('/api-docs', swaggerui.serve, swaggerui.setup(swaggerSpec))
   ```

5. 安装项目依赖并启动项目

   ```bash
   npm install
   npm run start
   ```

   通过访问 http://localhost:3000/api-docs 即可访问。

6. 下面说明如何编写接口文档

   如上`app.js`中配置的`apis`，在接口文件里面写上注释，程序会自动从注释中生成api文档，其实相当于把方法2中的`swagger.json`写到代码注释中。

   下面是一个举例：

   ```javascript
   /**
    * @swagger
    * definition:
    *   User:
    *     properties:
    *       id:
    *         type: 'number'
    *         description: '用户id'
    *       username:
    *         type: 'string'
    *         description: '用户名'
    *       password:
    *         type: 'string'
    *         description: '用户密码'
    *       email:
    *         type: 'string'
    *         description: '用户邮箱'
    *       phone:
    *         type: 'string'
    *         description: '用户电话'
    *       role:
    *         type: 'string'
    *         description: '用户角色'
    *       information:
    *         type: 'string'
    *         description: '用户简介'
    *       avatar:
    *         type: 'string'
    *         description: '用户头像'
    *       created_on:
    *         type: 'string'
    *         format: "date-time"
    *         description: '创建时间'
    *       updated_on:
    *         type: 'string'
    *         format: "date-time"
    *         description: '更新时间'
    *       last_login:
    *         type: 'string'
    *         format: "date-time"
    *         description: '上次登录时间'
    *       is_active:
    *         type: 'number'
    *         description: '是否激活'
    */
   
   /**
    * @swagger
    * /users/:
    *   get:
    *     tags:
    *       - user
    *     security:
    *     - tooken: []
    *     summary: 用户查询
    *     description: |-
    *       GET -->  /api_v1/users/?limit=15&offset=0&query=xxx
    *       GET -->  /api_v1/users/?limit=15&offset=0&username=xxx&email__icontainer=yyy&phone__container=zzz&limit=10*offset=0
    *       __icontainer表示不区分大小写的模糊搜索, __container表示区分大小写的模糊搜索
    *     produces:
    *       - application/json
    *     parameters:
    *       - name: query
    *         in: query
    *         description: 查找的内容，详情
    *         required: false
    *         type: string
    *     responses:
    *       200:
    *         description: 返回用户数组
    *         schema:
    *           properties:
    *             meta:
    *               type: object
    *               description: '用于分页的数据'
    *               properties:
    *                 limit:
    *                   type: number
    *                   description: '返回条数'
    *                 offset:
    *                   type: number
    *                   description: '从第几条开始返回'
    *                 total:
    *                   type: number
    *                   description: '总条数'
    *             objects:
    *               type: array
    *               description: '查询结果数组'
    *               $ref: '#/definitions/User'
    */
   router.get('/', async function (req, res, next) {
     // GET -->  /api_v1/users/?limit=15&offset=0&query=xxx
     // GET -->  /api_v1/users/?limit=15&offset=0&username=xxx&email__icontainer=yyy&phone__container=zzz
   
     const sql = queryToSQL('user', req.query, {
       // 对应select * from table_name 中的 *
       selectKeys: [
         'id', 'username', 'email', 'phone', 'role', 'information',
         'avatar', 'created_on', 'updated_on', 'is_active', 'last_login'
       ],
       // 查询url中允许的搜索值，其他会被忽略掉(limit offset query除外)
       allowsKeys: ['id', 'username', 'email', 'phone', 'role', 'information', 'is_active'],
       // query时查询的几个字段，关系 或
       queryParams: ['username__icontainer', 'email__container', 'phone__container', 'role']
     })
   
     const results = await queryDataBySQL(sql)
     // 通过 express-async-errors , 不需要代码去捕获错误给next传递了
     // .catch(error => {
     //   return next({ error: error, responseType: 'json' })
     // })
   
     const total = await queryDataBySQL('SELECT COUNT(*) as total FROM user').then(res => res[0].total)
     let limit = sql.match(/(?<=limit\s)\d+/)
     let offset = sql.match(/(?<=offset\s)\d+/)
     limit = limit ? parseInt(limit[0].trim()) : null
     offset = offset ? parseInt(offset[0].trim()) : null
   
     res.json({
       meta: {
         limit: limit,
         offset: offset,
         total: total
       },
       objects: results
     })
   })
   ```



编写规则见：https://swagger.io/docs/specification/2-0/describing-request-body/

其实上面的配置语法非常好理解，具体可以多观察对比下即可。作为粗略的入门，可以不看文档既能编写，这也是swagger的魅力所在吧。



> 这里遇到个问题【securityDefinitions设置无效】，参考下面文章解决：
>
> [2.0.6升级到2.0.8securityDefinitions设置失效,请求头部不会渲染出来](https://gitee.com/xiaoym/knife4j/issues/I2AJLX)



### 方法2：自定义接口描述文件

1. 安装nodejs和express（略）

2. 使用express生成项目结构

   ```bash
   express
   ```

3. 下载swaggerUI代码

   ```bash
   cd public
   git clone https://github.com/swagger-api/swagger-ui.git
   ```

4. 设置swagger的访问入口

   ```bash
   vim app.js
   ```

   加入下面代码

   ```javascript
   const path = require('path')
   
   app.use('/swagger', express.static(path.join(__dirname, 'public', 'swagger-ui', 'dist')))
   ```

5. 安装项目依赖并启动项目

   ```bash
   npm install
   npm run start
   ```

6. 通过访问 http://localhost:3000/swagger/index.html 即可访问。

   ![image-20210715164508559](http://blog.cdn.ionluo.cn/blog/image-20210715164508559.png)

7. 自定义接口描述文件并使用

   - 编写接口描述文件swagger.json（放在 `public/swagger-ui/dist` 中）

     > 描述文件编写规则见：https://swagger.io/docs/specification/2-0/describing-request-body/
     >
     > 使用swagger edit在线编辑器编辑：https://editor.swagger.io/

   - 编辑`public/swagger-ui/dist/index.html`

     ```javascript
     const ui = SwaggerUIBundle({
         // 注释下面这行
         // url: "https://petstore.swagger.io/v2/swagger.json",
         // 增加下面这行
         url: './swagger.json',
         // 增加下面这行
         validatorUrl: false,
         dom_id: '#swagger-ui',
         deepLinking: true,
         presets: [
             SwaggerUIBundle.presets.apis,
             SwaggerUIStandalonePreset
         ],
         plugins: [
             SwaggerUIBundle.plugins.DownloadUrl
         ],
         layout: "StandaloneLayout"
     });
     ```

   - 重启项目（`npm run start`）, 刷新url，即可看到新的接口文档

     > 完成以上步骤以后，你所自定义的接口即可通过swaggerUI的方式展示在人面前，便于查看。
     >
     > 但是当具体点击某一个接口并进行【Try it】的时候，你会发现并没有成功，调出浏览器调试页面后，你会发现是跨域问题。
     >
     > 这是因为，swagger是纯粹的前端，它并没有后台的支持，所有界面上接口的请求都是直接由ajax发起，而此时你的服务端获取并不支持跨域，所以便出现了跨域的问题。
     >
     > 至于如何解决跨域问题，这里不累述，自行百度。



### 总结

比较上面的两个方法，感觉各有千秋。方法1导致代码注释比代码还多，但是方便后台人员改接口的时候不会忘记修改API文档。方法2的swagger.json随着项目的增大，api的增多感觉不好维护，而且也有可能后台人员改了接口但是忘记改API文档的情况。

总的来说还是有学习的价值的，方便了解API文档的编写规范，同时可以集成数据模拟，接口测试，文档编写的功能。但是接口测试发现自定义程度不高，更喜欢使用postman。这里团队开发的话推荐一个使用过的[Doclever](http://www.doclever.cn/controller/index/index.html)，具体使用请看官网吧。



## 参考链接

- [使用Swagger构建Node.js API文档与Mock Server](https://www.jianshu.com/p/50446a0513f8)

- [使用 Swagger 构建 Express API Server 的文档系统](http://maples7.com/2016/09/06/build-doc-system-of-express-api-server-with-swagger/)
- [者也后端文档 使用 swagger 搭建` 1.0.0 `](http://api.vikingship.xyz/public/swagger/index.html)