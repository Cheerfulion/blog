---
title: 循环生成盒子的padding和margin类名
description: 暂无描述！
tags:
  - WEB开发
  - Web前端
  - Css预处理
  - Less
abbrlink: 7a4a55e3
date: 2021-01-20 00:15:24
---



```less
// 自定义函数generateGap
.generateGap(@number) {
    .m@{number} {
        margin: ~'@{number}px';
    }
    .mt@{number} {
        margin-top: ~'@{number}px';
    }
    .mr@{number} {
        margin-right: ~'@{number}px';
    }
    .mb@{number} {
        margin-bottom: ~'@{number}px';
    }
    .ml@{number} {
        margin-left: ~'@{number}px';
    }

    .pd@{number} {
        padding: ~'@{number}px';
    }
    .pdt@{number} {
        padding-top: ~'@{number}px';
    }
    .pdr@{number} {
        padding-right: ~'@{number}px';
    }
    .pdb@{number} {
        padding-bottom: ~'@{number}px';
    }
    .pdl@{number} {
        padding-left: ~'@{number}px';
    }
}

// 循环 when, loop名称可以自定义
.loop(@count) when (@count > -1) {
    .generateGap(@count);
    .loop((@count - 5));
}
.loop(50);
```

