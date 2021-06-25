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
            let key = temp[0];
            let value = temp[1];
            result[key] = value;
        })
    }
    return result;
}
console.log(getQueryParams())

export {
	getQueryParams
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





## 推荐阅读

- [一文帮你搞定90%的JS手写题，真香！](https://mp.weixin.qq.com/s/lgeKcq2Q5Biot6VhXn20rQ)
- [这些Web API真的有用吗？别问，问就是有用](https://mp.weixin.qq.com/s/f_eR5cqf1QiKzFlz424E5Q)