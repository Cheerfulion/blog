---
title: 【nodejs】使用swagger生成api文档
date: 2021-07-15 16:32:27
tags:
	- nodejs
	- express
---







## 方法1：在代码中定义接口描述

```bash
npm install -S swagger-jsdoc@4.0.0 swagger-ui-express@4.1.4
```





## 方法2：自定义接口描述文件

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