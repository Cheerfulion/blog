---
title: 【eslint】eslint的安装和配置（webstorm）
date: 2021-06-16 14:57:40
tags:
	- eslint
---





**step1**:  安装node环境（百度）。

 

**step2**: 全局安装eslint

```bash
npm install eslint -g
```

安装后可以看到全局安装的位置，这里我没有截图，可以自己复制到下面的步骤。

 

**step3**：创建一个`.eslintrc`文件，放在你项目的根目录下( https://github.com/eslint/eslint )

```JSON
{
    "extends": "eslint:recommended",
    "env":{
        "node":true,
        "es6":true
    },
    "rules": {
        "semi": ["error", "always"],
        "quotes": "off",
        "no-console":"off",
        "no-unused-vars":"off",
        "no-unreachable":"off",
        "no-redeclare":"warn"
    }
}
```

**可选配置** 让eslint忽略检测的文件 `.eslintignore` 配置规则与 `.gitignore` 一样

```.gitignore

# 如果 .eslintrc 开启了 env  nodejs 那么 默认 node_modules是自动忽略的
node_modules
/node_modules/**
*.sh
game-server/web-server
game-server/web-server/**
tools
tools/**
test
test/**
game-server/purchase-server/lib/seedrandom.js
game-server/app/staticData/data/temp/*.js
```

**step4：**打开webstorm, 选择File | Settings, 搜索ESLint，配置后点击OK即可

![image-20210616155853323](http://blog.cdn.ionluo.cn/blog/image-20210616155853323.png)