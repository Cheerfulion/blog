---
title: 我的带过期时间的localStorage
description: '-'
tags:
  - WEB开发
  - Web前端
  - Angular
abbrlink: dcb32251
date: 2021-03-18 20:33:11
---



```javascript
webServices.factory('storage', ['web', function (web) {
    // 使用该factory，只能记录对象类型数据，因为要加入过期时间功能
    return {
        // 检验浏览器是否支持localStorage
        validBrowser: function () {
            if (!window.localStorage) {
                web.error('错误', '您的浏览器版本过低，请升级重试！');
                return false;
            }
            return true;
        },
        // 辅助函数，用于保留对象中指定的字段
        holdField: function(object){
            object = angular.copy(object);
            var fields = Array.prototype.slice.call(arguments, 1);
            var keys = Object.keys(object);

            for (var i = 0; i < keys.length; i++) {
                if (fields.indexOf(keys[i]) === -1) {
                    delete object[keys[i]]
                }
            }
            return object;
        },
        // 辅助函数，用于删除对象中指定的字段
        deleteField: function (object) {
            object = angular.copy(object);

            for (var j = 1; j < arguments.length; j++) {
                delete object[arguments[j]];
            }
            return object;
        },
        getItem: function (slug) {
            var isOK = this.validBrowser();
            if(!isOK) return;

            var data =  window.localStorage.getItem(slug);
            if(!data) return data;

            data = JSON.parse(data);

            var now = new Date().getTime(),
                last = new Date(data.created_on).getTime() + data.expires;

            return now > last ? window.localStorage.removeItem(slug) : data;
        },
        setItem: function (slug, data, expires) {
            // data：普通对象　　expires：数据过期时间（默认是一天, 24 * 60 * 60 * 1000）
            // 函数作用是给保存的数据加上创建时间和过期时间
            var isOK = this.validBrowser();
            if(!isOK) return;

            data.created_on = new Date();
            data.expires = expires || 24 * 60 * 60 * 1000;

            window.localStorage.setItem(slug, JSON.stringify(data))
        }
    };
}]);
```

