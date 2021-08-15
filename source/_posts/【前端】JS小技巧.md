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



### 9_ 同步阻塞法实现sleep函数

```javascript
const sleep = delay => {
    const start = new Date().getTime();
    while (new Date().getTime() < start + delay) {
       continue;
    };
};
console.log(1);
sleep(3000);
console.log(2);
```



### 10_ 利用 new URL 解析 URL

```javascript
const parseURL = (url = '') => {
    // https://developer.mozilla.org/zh-CN/docs/Web/API/URL/URL
    const res = new URL(url);
    res.queryParams = key => {
        if (key) {
           return res.searchParams.get(key)
        };
        const params = {};
        const paramGroup = res.search.replace(/^\?/,'').split('&');
        paramGroup.forEach(param => {
            const [key, val] = param.split('=');
            params[key] = val;
        });
        return params;
    };
    return res;
};
parseURL('https://www.example.com/a/b?c=1&d=2');
```



### 11_ 一行代码实现星级评分

```javascript
const getRate = (rate = 0) => '★★★★★☆☆☆☆☆'.slice(5 - rate, 10 - rate);
getRate(3);
```



### 12_用位运算提升效率

```javascript
// ｜ 取整
let num1 = 1.7;
num1 = num1 | 0;

// >> 取半
let num2 = 6;
num2 = num2 >> 1; // 3

// << 加倍

let num3 = 6;
num3 = num3 << 1; / / 12

// ^ 交换值
let num4 = 10;
let num5 = 20;

num4 ^= num5;
num5 ^= num4;
num4 ^= num5; // num4 === 2, num5 === 1

// & 判断奇数
let num6 = 10;
let num7 = 11;

num6 & 1 === 1; // true
num7 & 1 === 1; // false

// ~ 判断是否存在
const data = '123456';
const key = '3';
const keyIsExist = !!~data.indexOf(key); // true

// 是否 2 的整数幂
const isPowerOf2 = num => (num & (num - 1)) === 0;
isPowerOf2(8) // true
isPowerOf2(7) // false
```



### 13_判断是否是千分符字符

```javascript
const numberIsThousand = str => /^-?\d{1,3}(,\d{3})*(\.\d+)?$/.test(str);
numberIsThousand('100,000,000,000') // true
numberIsThousand('100,000,000,00') // false
```





### 14_复制文本到剪切板

```javascript
const copyToClipboard = content => {
    const clipboardData = window.clipboardData;
    if (clipboardData) {
        clipboardData.clearData();
        clipboardData.setData('Text', content);
        return true;
    } else if (document.execCommand) {
        const el = document.createElement('textarea');
        el.value = content;
        el.setAttribute('readonly', '');
        el.style.position = 'absolute';
        el.style.left = '-9999px';
        document.body.appendChild(el);
        el.select();
        document.execCommand('copy');
        document.body.removeChild(el);
        return true;
    }
    return false;
};copyToClipboard('hello world');
```



### 15_一行代码生成指定长度的数组

```javascript
const List = len => [...new Array(len).keys()];
const list = List(10); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```



### 16_判断数据类型

```javascript
const type = data => {
      let toString = Object.prototype.toString;
      const dataType =
        data instanceof Element
          ? 'element' // 为了统一DOM节点类型输出
          : toString
              .call(data)
              .replace(/\[object\s(.+)\]/, '$1')
              .toLowerCase()
      return dataType
};

type({}); // object
```



### 17_正则判断字符重复次数不超过两次

```javascript
const strIsRepeatThan2 = (str = '') => /^(?!.*(.).*\1{2})[\da-zA-Z].+$/.test(str);
strIsRepeatThan2('123456'); // true
strIsRepeatThan2('1234566'); // true
strIsRepeatThan2('12345666'); // false
```



### 18_正则匹配可以只有 0 但开头不能是 0 的数字

```javascript
const getCorrectNumber = (str = '') => /^(\d|[1-9]\d*)$/.test(str);
getCorrectNumber('0'); // true
getCorrectNumber('011'); // false
getCorrectNumber('101'); // true
```





## 推荐阅读

- [一文帮你搞定90%的JS手写题，真香！](https://mp.weixin.qq.com/s/lgeKcq2Q5Biot6VhXn20rQ)
- [这些Web API真的有用吗？别问，问就是有用](https://mp.weixin.qq.com/s/f_eR5cqf1QiKzFlz424E5Q)