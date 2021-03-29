---
title: 我的Grunt配置
description: '-'
tags:
  - WEB开发
  - Web前端
  - Grunt
abbrlink: edb97d18
date: 2021-02-12 13:31:15
---



> 这里存放我的grunt配置，这里打包之后的文件可以单独引用，也可以把需要合并的部分合并到一起引用

```javascript
/*jshint esversion: 6 */

// 下载包的压缩文件位置（现在下载包目录包太多了，这里就不放全部了，需要就单独指定）
const packageCn = {
    css: [
        'bower_components/bootstrap/dist/css/bootstrap.min.css',
        'bower_components/font-awesome/css/font-awesome.min.css',
        'bower_components/angular-busy/dist/angular-busy.min.css',
        'bower_components/ng-boat-dialog/dist/ng-boat-dialog.min.css'
    ],
    js: [
        'bower_components/underscore/underscore.min.js',
        'bower_components/jquery/dist/jquery.min.js',
        'bower_components/bootstrap/dist/js/bootstrap.min.js',
        'bower_components/angular/angular.min.js',
        'bower_components/ng-file-upload/ng-file-upload-all.min.js',
        'bower_components/ng-file-upload/angular-file-upload.min.js',
        'bower_components/angular-busy/dist/angular-busy.min.js',
        'bower_components/angular-ui-router/release/angular-ui-router.min.js',
        'bower_components/angular-bootstrap/ui-bootstrap-tpls.min.js',
        'bower_components/checklist-model/checklist-model.min.js',
        'bower_components/restangular/dist/restangular.min.js',
        'bower_components/ng-boat/dist/ng-boat.min.js',
        'bower_components/angular-socialshare/dist/angular-socialshare.min.js',
        'bower_components/angular-validator/dist/angular-validator.min.js'
    ]
};
// 项目代码文件位置（需要合并的css和js, 统一放到 web.cn.min.css 或 web.cn.min.js 中）
const compileCn = {
    css: [
        'less/web/public.less',
        'less/web/cn/cn.less',
        'less/web/cn/about-us.less',
        'less/web/cn/resource.less',
        'less/web/cn/community.less',
        'less/web/cn/project.less'
    ],
    js: [
        'js/web/cn/angular-validator-rules.js',
        'js/web/cn/services/web-services.js',
        'js/web/cn/directives/web-directives.js',
        'js/web/cn/filters/web-filters.js',
        'js/web/cn/app.js',
        'js/web/cn/controllers/public-es6.js',
        'js/web/cn/controllers/about-us.js',
        'js/web/cn/controllers/resource.js',
        'js/web/cn/controllers/community.js',
        'js/web/cn/controllers/project.js',

        'js/web/cn/controllers/account.js',
        'js/web/cn/controllers/gene-search.js',
        'js/web/cn/extra.js',
        'js/web/cn/public.js'
    ]
};

// 把项目css代码路径转成压缩后css的存放路径
compileCn.css = compileCn.css.map(item => {
    item = 'build/<%= meta.revision %>/css' + item.substr(4);
    return item.slice(0, -4) + 'min.css'
});

// 把项目js代码路径转成压缩后js的存放路径
compileCn.js = compileCn.js.map(item => {
    item = 'build/<%= meta.revision %>/js' + item.substr(2);
    return item.slice(0, -2) + 'min.js'
});


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
        dist: {
            options: {
                compress: true // 是否压缩css
            },
            files: [{
                expand: true,
                cwd: 'less',
                src: '**/*.less',
                dest: 'build/<%= meta.revision %>/css/',
                ext: '.min.css'
            }]
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
        dist: {
            files: [{
                expand: true,
                cwd: 'js',
                src: '**/*.js',
                dest: 'build/<%= meta.revision %>/babel',
                // ext: '.js'
            }]
        },
    };

    // js代码压缩混淆
    option.uglify = {
        options: {
            compress: {
                drop_console: false
            }
        },
        dist: {
            files: [{
                expand: true,
                cwd: 'build/<%= meta.revision %>/babel',
                src: '**/*.js',
                dest: 'build/<%= meta.revision %>/js',
                ext: '.min.js'
            }]
        }
    };

    // 文件合并
    option.concat = {
        js: {
            files: {
                'build/<%= meta.revision %>/js/web.cn.min.js': packageCn.js.concat(compileCn.js)
            }
        },
        css: {
            files: {
                'build/<%= meta.revision %>/css/web.cn.min.css': packageCn.css.concat(compileCn.css)
            }
        }
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
                    src: ['build/<%= meta.revision %>/css/**/*.css'],
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
    option.copy = {
        fonts: {
            expand: true,
            flatten: true,
            src: ['bower_components/font-awesome/fonts/**', 'bower_components/bootstrap/fonts/**'],
            dest: 'build/<%= meta.revision %>/fonts',
            filter: 'isFile'
        },
        select2: {
            src: 'bower_components/select2/select2.png',
            dest: 'build/<%= meta.revision %>/css/select2.png'
        },
        manage_tinymce: {
            files: [
                {
                    expand: true,
                    cwd: 'bower_components/tinymce/langs/',
                    src: ['**'],
                    dest: 'build/<%= meta.revision %>/js/langs/'
                },
                {
                    expand: true,
                    cwd: 'bower_components/tinymce/themes/',
                    src: ['**'],
                    dest: 'build/<%= meta.revision %>/js/themes/'
                },
                {
                    expand: true,
                    cwd: 'bower_components/tinymce/skins/',
                    src: ['**'],
                    dest: 'build/<%= meta.revision %>/js/skins/'
                },
                {
                    expand: true,
                    cwd: 'bower_components/tinymce/plugins/',
                    src: ['**'],
                    dest: 'build/<%= meta.revision %>/js/plugins/'
                },
                {
                    expand: true,
                    cwd: 'bower_components/tinymce/icons/',
                    src: ['**'],
                    dest: 'build/<%= meta.revision %>/js/icons/'
                }
            ]
        }
    };

    // 文件清理
    option.clean = {
        old: {
            src: ['build/<%= meta.revision %>/*']
        },
        babel_js: {
            src: ['build/<%= meta.revision %>/babel/*']
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
            'clean:old',
            'less',
            'babel_multi_files',
            'uglify',
            'concat',
            'replace',
            'clean:babel_js',
            'copy'
        ]
    );
};

```

