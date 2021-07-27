---
title: 【前端】JS小技巧
tags:
  - 前端
abbrlink: 229e087d
date: 2021-06-25 11:27:26
---





## JS小技巧



### 1_获取URL参数并转换成对象

```javascript
// 获取URL参数并转换成对象
function getQueryParams(){
    const result = {};
    const querystring = window.location.search;
    // "?name=ionluo&age=25"
    const reg = /[?&][^?&]+=[^?&]+/g;
    const found = querystring.match(reg);
    if(found){
        found.forEach(item => {
            let temp = item.substring(1).split('=');
            let key = decodeURIComponent(temp[0]);
            let value = decodeURIComponent(temp[1]);
            result[key] = value;
        })
    }
    return result;
}

// 根据对象转换成URL查询参数
function setQueryParams(prefixUrl, paraObj) {
    var link = "?";
    Object.keys(paraObj).forEach(function (key) {
        link += encodeURIComponent(key) + '=' + encodeURIComponent(paraObj[key]) + '&';
    });
    link = link.slice(0, -1);
    return prefixUrl + link;
};

export {
	getQueryParams,
    setQueryParams
}
```



### 2_封装storage的存取

```javascript
 // 封装storage的存取(加入命名空间和过期时间控制)
// {
//     created_on: '',  // 创建时间
//     updated_on: '',  // 更新时间（默认等于创建时间）
//     expires: '',   // 过期时间（毫秒）
//     data: '',    // 存储的数据
// }
const obj = {
    namespace: 'manage',
    init(namespace){
        // 初始化命名空间
        this.namespace = namespace || this.namespace;
        // 删除过期变量
        let storage = window.localStorage.getItem(this.namespace);
        storage = storage || '{}';
        storage = JSON.parse(storage);
        for(let k in storage){
            if (storage[k].expires && new Date().getTime() - new Date(storage[k].updated_on).getTime() > storage[k].expires){
                delete storage[k];
            }
        }
        window.localStorage.setItem(this.namespace, JSON.stringify(storage));
    },
    setItem(key, value, expires) {
        let storage = window.localStorage.getItem(this.namespace);
        storage = storage || '{}';
        storage = JSON.parse(storage);
        let date = new Date();
        storage[key] = {
            created_on: date,  // 创建时间
            updated_on: date,  // 更新时间（默认等于创建时间）
            expires: expires,   // 过期时间（毫秒, 空表示永不过期）
            data: value,    // 存储的数据
        };

        // 对于存储大小超出了限制的情况处理
        // https://segmentfault.com/q/1010000014736335/a-1020000014736515
        try {
            window.localStorage.setItem(this.namespace, JSON.stringify(storage));
        } catch (e) {
            // 删除过期数据
            // this.init();
            // 删除最久远的10条数据
            // this.removeOldestTen();
            window.localStorage.setItem(this.namespace, '{}');
            this.setItem(key, value, expires);
        }
    },
    getItem(key) {
        this.init();
        let storage = window.localStorage.getItem(this.namespace);
        storage = storage || '{}';
        storage = JSON.parse(storage);
        return storage[key] && storage[key].data;
    },
    removeItem(key) {
        let storage = window.localStorage.getItem(this.namespace);
        storage = storage || '{}';
        storage = JSON.parse(storage);
        if (storage[key]){
            delete storage[key]
        }
        window.localStorage.setItem(this.namespace, JSON.stringify(storage));
    }
}
export obj;

// obj.init('custom');
// obj.setItem('key1', 'value1', 5 * 1000);
// setTimeout(function(){
//     console.log(obj.getItem('key1'));
// }, 6 * 1000)
```



### 3_实现洗牌函数

```javascript
// 获取一个在[min, max]的整数 
function getRandomNumber(min, max) {
     // [0, 1) -> [0, max-min+1) -> [min, max+1) -> [min, max]
     return Math.floor(Math.random() * (max - min + 1) + min);
 }

// 实现洗牌函数
function shuffle(arr) {
    // 浅拷贝
    let _arr = arr.slice();

    for(let i = 0; i < _arr.length; i++) {
        const j = getRandomNumber(0, i);
        [_arr[i], _arr[j]] =  [_arr[j], _arr[i]];
    }
    return _arr;
}

export shuffle;
```



### 4_格式化UTC时间

```javascript
Date.prototype.format = function (fmt) {
    var o = {
        "M+": this.getMonth() + 1,                 //月份
        "d+": this.getDate(),                    //日
        "H+": this.getHours(),                   //小时
        "m+": this.getMinutes(),                 //分
        "s+": this.getSeconds(),                 //秒
        "q+": Math.floor((this.getMonth() + 3) / 3), //季度
        "S": this.getMilliseconds()             //毫秒
    };
    if (/(y+)/.test(fmt)){
        fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    }
    for (var k in o){
        if (new RegExp("(" + k + ")").test(fmt)){
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
        }
    }
    return fmt;
};

// 用法
new Date().format("yyyy-MM-dd HH:mm:ss");
// 中间可以插入其他字符，不替换，如
new Date().format("yyyy-MM-ddTHH:mm:ss");
```



### 5_移动端拖拽

```javascript
var block = document.querySelector("#element");
    var oW, oH;
    // 绑定touchstart事件
    block.addEventListener("touchstart", function (e) {
        console.log(e);
        var touches = e.touches[0];
        oW = touches.clientX - block.offsetLeft;
        oH = touches.clientY - block.offsetTop;
    }, false);

    block.addEventListener("touchmove", function (e) {
        var touches = e.touches[0];
        var oLeft = touches.clientX - oW;
        var oTop = touches.clientY - oH;
        if (oLeft < 0) {
            oLeft = 0;
        } else if (oLeft > document.documentElement.clientWidth - block.offsetWidth) {
            oLeft = (document.documentElement.clientWidth - block.offsetWidth);
        }
        block.style.left = oLeft + "px";
        block.style.top = oTop + "px";
        e.preventDefault();
    }, false);
```

参考：https://www.cnblogs.com/lvmingyin/p/5372678.html

touchstart→touchmove→touchend或者touchstart→touchend→click。



### 6_PC端拖拽

```javascript
var running_mouse = document.getElementById('running-mouse');

function loadStatus(element, storage_name) {
    var storage = window.localStorage.getItem(storage_name);
    if (storage) {
        storage = JSON.parse(storage);
        if (new Date().getTime() - new Date(storage.created_on).getTime() > 24 * 60 * 60 * 1000) {
            window.localStorage.removeItem(storage_name);
        } else {
            element.style.left = storage.left;
            element.style.top = storage.top;
            element.style.display = storage.close ? 'none' : 'block';
        }
    }
}

loadStatus(running_mouse, 'running-mouse');

function dragElement(element, url, storage_name) {
    element.addEventListener('mousedown', function (event) {
        var originMouseX, originMouseY, moveX, moveY;

        originMouseX = event.clientX;
        originMouseY = event.clientY;
        document.addEventListener('mousemove', mouseMove, false);
        document.addEventListener('mouseup', mouseUp, false);

        function mouseMove(event) {
            moveX = event.clientX - originMouseX;
            moveY = event.clientY - originMouseY;
            originMouseX = event.clientX;
            originMouseY = event.clientY;
            element.style.left = +moveX + element.offsetLeft + 'px';
            element.style.top = +moveY + element.offsetTop + 'px';
            event.preventDefault();
        }

        function mouseUp(event) {
            document.removeEventListener('mousemove', mouseMove, false);
            document.removeEventListener('mouseup', mouseUp, false);
            var is_close = false;
            if (url && !moveX && !moveY) {
                if (event.target.tagName === "SPAN") {
                    element.style.display = 'none';
                    is_close = true;
                } else {
                    var a = document.createElement('a');
                    a.href = url;
                    a.style.display = 'none';
                    a.click();
                }
            }

            window.localStorage.setItem(storage_name, JSON.stringify({
                left: element.style.left,
                top: element.style.top,
                close: is_close,
                created_on: new Date()
            }));
        }
    }, false)
}

dragElement(running_mouse, '/cn/zh-cn/public-search.html', 'running-mouse');

```





### 7_ 移动到元素上出现滚动条，移出滚动条消失

```html
<!-- 移动到元素上出现滚动条，移出滚动条消失 -->
<div onmouseover="this.style.overflowY='auto'"    			     onmouseout="this.style.overflowY='hidden'"></div>
```



### 8_ 全局异常处理

https://juejin.cn/post/6844903751271055374



## 推荐阅读

- [一文帮你搞定90%的JS手写题，真香！](https://mp.weixin.qq.com/s/lgeKcq2Q5Biot6VhXn20rQ)
- [这些Web API真的有用吗？别问，问就是有用](https://mp.weixin.qq.com/s/f_eR5cqf1QiKzFlz424E5Q)