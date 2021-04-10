---
title: 配置babel和less支持
description: '-'
tags:
  - WEB开发
  - Web前端
  - Grunt
abbrlink: 3d49bc23
date: 2021-02-12 13:31:15
---



> 2021-01-14 更新，原先博客的`grunt-babel`配置不支持文件合并，只能使用通配符的形式。所以这里替换掉插件`grunt-babel` 为 `grunt-babel-multi-files``
>
> ``"grunt-babel-multi-files": "^0.2.0"`

## package.json

```json
{
  "name": "grunt_learn",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/core": "^7.11.6",
    "@babel/preset-env": "^7.11.5",
    "babel-plugin-angularjs-annotate": "^0.10.0",
    "bower": "^1.3.10",
    "grunt": "^1.3.0",
    // "grunt-babel": "^8.0.0",
    "grunt-babel-multi-files": "^0.2.0",
    "grunt-contrib-clean": "^2.0.0",
    "grunt-contrib-concat": "^1.0.1",
    "grunt-contrib-copy": "^1.0.0",
    // "grunt-contrib-cssmin": "^3.0.0",
    "grunt-contrib-imagemin": "^4.0.0",
    "grunt-contrib-less": "^2.0.0",
    "grunt-contrib-uglify": "^5.0.0",
    "grunt-git-revision": "0.0.1",
    "grunt-ng-annotate": "^4.0.0",
    "grunt-replace": "^1.0.1"
  },
  "dependencies": {}
}

```

> json 不支持注释，这里为了展示之前的一些插件，就使用js的注释



## Gruntfile.js

```javascript
/*jshint esversion: 6 */

const webCnMin = {
    css: [
        'bower_components/bootstrap/dist/css/bootstrap.min.css',
        'bower_components/font-awesome/css/font-awesome.min.css',
        'bower_components/angular-busy/dist/angular-busy.min.css',
    ],
    js: [
        'bower_components/underscore/underscore.min.js',
        'bower_components/jquery/dist/jquery.min.js',
        'bower_components/bootstrap/dist/js/bootstrap.min.js',
        'bower_components/angular/angular.min.js',
        'bower_components/angular-busy/dist/angular-busy.min.js',
        'bower_components/checklist-model/checklist-model.min.js',
        'bower_components/angular-validator/dist/angular-validator.min.js'
    ]
};

module.exports = function (grunt) {
    const option = {};

    // package.json文件信息
    option.pkg = grunt.file.readJSON('package.json');

    // 版本号
    option.revision = {
        options: {
            property: 'meta.revision',
            ref: 'FETCH_HEAD',
            short: true
        }
    };

    // 合并，编译，压缩css
    option.less = {
        // 编译大量文件时可以用在线编译器找出错位置（https://tool.oschina.net/less）
        options: {
            compress: true // 是否压缩css
        },
        web_cn: {
            files: {
                'build/<%= meta.revision %>/css/web.cn.min.css': webCnMin.css.concat([
                    'less/web/public.less',
                    'less/web/cn/cn.less'
                ])
            }
        }
    };

    // es6转es5
    option.babel_multi_files = {
        // babel在线编译　　https://babeljs.io/repl/
        options: {
            sourceMap: false, // 生成map文件
            compact: false,
            presets: ['@babel/preset-env'],
            plugins: [['angularjs-annotate', {'explicitOnly': true}]]
        },
        web_cn: {
            files: {
                'build/<%= meta.revision %>/js/web.cn.babel.js': [
                    'js/web/cn/angular-validator-rules.js',
                    'js/web/cn/services/web-services.js',
                    'js/web/cn/directives/web-directives.js',
                    'js/web/cn/filters/web-filters.js',
                    'js/web/cn/app.js',
                    'js/web/cn/controllers/public-es6.js'
                ]
            }
        }
    };

    // js代码压缩混淆
    option.uglify = {
        options: {
            compress: {
                drop_console: false
            }
        },
        web_cn: {
            files: {
                'build/<%= meta.revision %>/js/web.cn.uglify.js': 'build/<%= meta.revision %>/js/web.cn.babel.js'
            }
        },
    };

    // 文件合并
    option.concat = {
        web_cn: {
            files: {
                'build/<%= meta.revision %>/js/web.cn.min.js': webCnMin.js.concat([
                    'build/<%= meta.revision %>/js/web.cn.uglify.js'
                ])
            }
        },
    };

    // 替换（这里用于css图片加上版本号，确保更新版本后客户端刷新缓存）
    option.replace = {
        dist: {
            options: {
                patterns: [
                    {
                        match: /\.(png|jpg|gif|jpeg|tiff)/gi,
                        replacement: '.$1?v=<%= meta.revision %>'
                    }
                ]
            },
            files: [
                {
                    expand: true,
                    flatten: true,
                    src: ['build/<%= meta.revision %>/css/*.css'],
                    dest: 'build/<%= meta.revision %>/css/'
                }
            ]
        }
    };

    // 图片压缩
    option.imagemin = {
        all: {
            options: {
                progressive: true
            },
            files: [{
                expand: true,
                cwd: 'img/',
                src: ['**/*.{png,jpg,gif}'],
                dest: 'img/'
            }]
        }
    };

    // 文件复制
    option.copy = {     //复制css文件里所引用的文件到对应的目录下
        fonts: {    //copy fonts
            expand: true,
            flatten: true,
            src: ['bower_components/font-awesome/fonts/**', 'bower_components/bootstrap/fonts/**'],
            dest: 'build/<%= meta.revision %>/fonts',
            filter: 'isFile'
        }
    };

    // 文件清理
    option.clean = {
        // old: {
        //     src: ['build/<%= meta.revision %>/*']
        // },
        annotate_js: {
            src: ['build/<%= meta.revision %>/js/*.annotate.js']
        },
        babel_js: {
            src: [
                'build/<%= meta.revision %>/js/*.babel.js',
                'build/<%= meta.revision %>/js/*.babel.js.map'
            ]
        },
        uglify_js: {
            src: ['build/<%= meta.revision %>/js/*.uglify.js']
        }
    };

    grunt.initConfig(option);

    grunt.loadNpmTasks('grunt-replace');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-less');
    grunt.loadNpmTasks('grunt-babel-multi-files');
    // grunt.loadNpmTasks('grunt-babel');
    // grunt.loadNpmTasks('grunt-contrib-cssmin');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-copy');
    grunt.loadNpmTasks('grunt-contrib-clean');
    grunt.loadNpmTasks('grunt-git-revision');
    grunt.loadNpmTasks('grunt-ng-annotate');

    // grunt.loadNpmTasks('grunt-contrib-imagemin');
    // grunt.registerTask('img', ['imagemin']);

    grunt.registerTask(
        'build', [
            'revision',

            'less',
            'replace',

            'babel_multi_files',
            'uglify',
            'concat',

            'clean',
            'copy'
        ]
    );
};
```



## 说明

### less编译后修改后缀

> https://stackoverflow.com/questions/15344584/grunt-0-4-less-task-how-to-not-concatenate-destination-files
>
> `grunt-contrib-less` 生成单独的文件, 不合并
>
> ```javascript
> option.less = {
>     // 编译大量文件时可以用在线编译器找出错位置（https://tool.oschina.net/less）
>     options: {
>         compress: true // 是否压缩css
>     },
>     web_cn: {
>         files: [{
>             expand: true,
>             cwd: 'less',
>             src: '×*/*.less',
>             dest: 'build/<%= meta.revision %>/css/',
>             ext: '.css'
>         }],
>     },
> };
> ```



### 解决angular的依赖注入问题

> 因为angular有依赖注入这个特点, `uglify`插件压缩代码是不安全的，所以要借助`grunt`的`grunt-ng-annotate`包实现依赖注入。具体可以查看 [angular 压缩最佳实践](https://segmentfault.com/q/1010000004520699)
>
> 这里引用下[**Mr_Betty**](https://segmentfault.com/u/guox) 的回答：
>
> 1. uglify后变量名被替换确实会影响module对依赖的识别(通过字面量), angular的解决办法是在`[]`内将依赖名以字符串的形式作为数组元素声明一遍，如上述的`$scope`,最后一个元素接着控制器代码, 这样即便控制器参数中的依赖变量被替换,$inject 服务仍能通过前面的字符串声明取得(字符串不变)。
>
>    ```javascript
>    angular.module('xxx').controller('xxCtrl', ['$scope', function ($scope) {
>        
>    }]);
>    ```
>
> 2. 也可以显示通过$inject服务注入:
>
>    ```javascript
>    angular.module('app').controller('ActivityCtrl', ActivityCtrl);
>    
>    ActivityCtrl.$inject = [
>        '$state',
>        '$sce'
>    ];
>    
>    function ActivityCtrl ($state, $sce) {}
>    ```
>
>    原理都是一个, 即`angular`中的`$inject`服务注入依赖





### babel转换新的API

babel默认是只会去转义js语法的，不会去转换新的API，比如像Promise、Generator、Symbol这种全局API对象，babel是不会去编译的。所以上面的配置使用的`@babel/preset-env`预设并不能满足项目需求。

由于我们这里使用的是Grunt而不是模块化的打包工具，所以网上的很多配置都是不适用的，查看[babel官方文档](https://babeljs.io/docs/en/babel-polyfill)可以看到，直接在所有js之前引入@babel/polyfill 发布的文件dist/polyfill.js 就行了。

```html
<script src="https://cdn.bootcss.com/babel-polyfill/7.0.0-rc.4/polyfill.min.js"></script>
<script src="/js/web.min.js"></script>
```



推荐阅读：[Babel 配置用法解析](https://www.cnblogs.com/bai1218/p/12392180.html)



