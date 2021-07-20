---
title: 【前端】IndexedDB从入门到精通
date: 2021-07-20 18:41:26
tags:	
	- 前端
---



## [IndexedDB兼容情况](https://caniuse.com/?search=indexDB)

![image-20210721000142815](http://blog.cdn.ionluo.cn/blog/image-20210721000142815.png)



## [IndexedDB 应用探索](https://segmentfault.com/a/1190000019130708)

说起 IndexedDB，大家应该会有一些疑问，比如：什么是 IndexedDB？适合什么业务场景？哪些公司哪些业务已经开始使用 indexedDB了？带着这些问题，阅读本文，相信能够给你答案。

### IndexedDB 诞生背景

在开始之前，我们先简单梳理一下浏览器存储的几种方式（详见👉[浏览器存储方式](https://segmentfault.com/a/1190000015804205)）

|              | 会话期 Cookie        | 持久性 Cookie            | sessionStorage       | localStorage             | indexedDB            | WebSQL |
| ------------ | -------------------- | ------------------------ | -------------------- | ------------------------ | -------------------- | ------ |
| 存储大小     | 4kb                  | 4kb                      | 2.5~10 MB            | 2.5~10 MB                | >250MB               | 已废弃 |
| 失效时间     | 浏览器关闭自动清除   | 设置过期时间，到期后清除 | 浏览器关闭后清除     | 永久保存（除非手动清除） | 手动更新或删除       | 已废弃 |
| 与服务端交互 | 有                   | 有                       | 无                   | 无                       | 无                   | 已废弃 |
| 访问策略     | 符合同源策略可以访问 | 符合同源策略可以访问     | 符合同源策略可以访问 | 即使同源也不可相互访问   | 符合同源策略可以访问 | 已废弃 |

WebSQL 是一种浏览器存储方案，属于传统的关系型数据库，需要写 sql 语句查询。WebSQL 出现过一段时间，虽然已被部分浏览器支持，但又被废弃，由 IndexedDB 取代。

> 废弃原因：
> This document was on the W3C Recommendation track but specification work has stopped. The specification reached an impasse: all interested implementors have used the same SQL backend (Sqlite), but we need multiple independent implementations to proceed along a standardisation path.
> 大意：
> 该文件是W3C推荐标准，但规范的制定工作已经停止。该规范陷入僵局：所有感兴趣的实现者都使用了相同的SQL后端（SQLite的），但我们需要多个独立的实现沿着规范化的路径进行。

cookie 和 webStorage 存储数据格式仅支持 String，存储时需要借助 `JSON.stringify()` 将 JSON 对象转化为字符串，读取时需要借助 `JSON.parse()` 将字符串转化为 JSON 对象。

一般来说，我们更推荐使用 webStroage，但其存储大小有限、数据存储仅支持 String 格式、不提供搜索功能，不能建立自定义的索引。因此，需要一种新的解决方案，这就是 `IndexedDB` 诞生的背景。

### 一、什么是 IndexedDB ？

> 通俗地说，IndexedDB 就是浏览器提供的本地数据库，它可以被网页脚本创建和操作。IndexedDB 允许储存大量数据，提供查找接口，还能建立索引。这些都是 LocalStorage 所不具备的。就数据库类型而言，IndexedDB 不属于关系型数据库（不支持 SQL 查询语句），更接近 NoSQL 数据库。

从 DB（Data Base) 可以看出它是一个数据库。常用的数据库有两种类型：

- 关系型数据库：数据存储在表中，使用 sql 语句操作数据库，如：`MySQL`、`Oracle`、`WebSQL`（已废弃）
- 非关系型数据库：数据集作为 JSON 对象存储，不需要写sql 语句，如：`Redis`、`MongoDB`、`IndexedDB`

IndexedDB 是非关系型数据库，不需要写 sql 语句进行数据库操作，数据格式可使用 JSON 对象。

IndexedDB 有很多优点：

- 存储空间大：没有存储上限，一般来说不小于 250M
- 存储格式多样：
  - 支持字符串存储
  - 支持二进制存储(ArrayBuffer 对象和 Blob 对象）
  - 支持 JSON 键值对存储，一个对象相当于关系型数据库中的数据表，我们称其为 `对象仓库 (object store)`
- 异步操作：性能更强。防止进行大量数据读写时，拖慢网页（localStorage 的操作是同步的）
- 同源限制：每一个数据库对应一个域名。只能访问自身域名下的数据库，不能跨域访问
- 支持事务：在一系列操作步骤之中，如果有一步失败，那么整个事务就都会取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况

### 二、适用场景

- cookie：短期登陆，例如：token 会过期，需要设置过期时间，过期后重新换取 token
- sessionStorage：敏感账号一次性登录
- localStorage：长期登录
- indexedDB：存储大量结构化数据数据

对于简单的数据，应该继续使用 localStorage；对于大量结构化数据，indexedDB 会更适合。当然如果你需要设置过期时间的短期存储，还是使用 cookie 存储吧。

### 三、开始使用

**基本步骤：**

- 打开（创建）数据库，并开始一个事务
- 创建一个 object store
- 构建一个请求来执行一些数据库操作，像增加或提取数据等
- 通过监听正确类型的 DOM 事件以等待操作完成
- 在操作结果上进行一些操作（可以在 request 对象中找到）

下面给出了三种使用方案，你可以用最简单的原生API 进行基本操作，也可以自己封装一个库，也可以用第三方库。

（1）基础调用

（2）自己封装

```javascript
<script>
  var myDB = {
    name: 'school', // 数据库名
    version: 1, // 数据库版本号
    db: null,
    ojstore: {
      name: 'teachers', // 存储空间表的名字
      keypath: 'id', // 主键
      indexKey: 'age' // 年龄索引
    }
  }

  var INDEXDB = {
    indexedDB: window.indexedDB || window.webkitindexedDB,
    IDBKeyRange: window.IDBKeyRange || window.webkitIDBKeyRange, // 键范围
    // 打开或创建数据库，建立对象存储空间 ObjectStore
    openDB: function (dbname, dbversion) {
      var that = this
      var version = dbversion || 1
      var request = that.indexedDB.open(dbname, version)

      request.onerror = function (e) {
        console.log(e.currentTarget.error.message)
      }

      request.onsuccess = function (e) {
        myDB.db = e.target.result
        console.log('成功建立并打开数据库:' + myDB.name + 'version' + dbversion)
      }

      request.onupgradeneeded = function (e) {
        var db = e.target.result
        var transaction = e.target.transaction
        var store

        if (!db.objectStoreNames.contains(myDB.ojstore.name)) {
          //没有该对象空间时创建该对象空间
          store = db.createObjectStore(myDB.ojstore.name, {
            keyPath: myDB.ojstore.keypath
          })
          console.log('成功建立对象存储空间：' + myDB.ojstore.name)
          that.storeIndex(store, myDB.ojstore.indexKey)
        }
      }
    },
    // 删除数据库
    deletedb: function (dbname) {
      var that = this
      that.indexedDB.deleteDatabase(dbname)
      console.log(dbname + '数据库已删除')
    },
    // 关闭数据库
    closeDB: function (db) {
      db.close()
      console.log('数据库已关闭')
    },
    // 添加数据，重复添加会报错
    addData: function (db, storename, data) {
      var store = db.transaction(storename, 'readwrite').objectStore(storename)
      var request

      for(var i = 0; i < data.length; i++) {
        request = store.add(data[i])

        request.onerror = function() {
          console.error('add添加数据库中已有该数据')
        }

        request.onsuccess = function() {
          console.log('add添加数据已存入数据库')
        }
      }
    },
    // 通过游标查询记录
    cursorGetData: function (db, storename, keyRange) {
      var keyRange = keyRange || ''
      var store = db.transaction(storename, 'readwrite').objectStore(storename)
      var request = store.openCursor(keyRange)

      request.onsuccess = function (e) {
        var cursor = e.target.result

        if (cursor) { // 必须要检查
          console.log(cursor)
          cursor.continue() // 遍历了存储对象中的所有内容
        } else{
          console.log('游标查询结束')
        }
      }
    },
    // 通过索引游标查询记录
    cursorGetDataByIndex: function (db, storename, keyRange) {
      var keyRange = keyRange || ''
      var store = db.transaction(storename, 'readwrite').objectStore(storename)
      var request = store.index('age').openCursor(keyRange)

      request.onsuccess = function (e) {
        console.log('游标开始查询')
        var cursor = e.target.result

        if (cursor) {//必须要检查
          console.log(cursor)
          cursor.continue()//遍历了存储对象中的所有内容
        } else {
          console.log('游标查询结束')
        }
      }
    },
    // 通过游标更新记录
    cursorUpdateData: function (db, storename) {
      var keyRange = keyRange || ''
      var store = db.transaction(storename,'readwrite').objectStore(storename)
      var request = store.openCursor()

      request.onsuccess = function (e) {
        console.log('游标开始查询')
        var cursor = e.target.result
        var value, updateRequest

        if (cursor) { // 必须要检查
          console.log(cursor)
          if (cursor.key === 1002) {
            console.log('游标开始更新')
            value = cursor.value
            value.age = 38
            updateRequest = cursor.update(value)

            updateRequest.onerror = function () {
              console.log('游标更新失败')
            }

            updateRequest.onsuccess = function () {
              console.log('游标更新成功')
            }
          } else {
            cursor.continue()
          }
        }
      }
    },
    // 通过游标删除记录
    cursorDeleteData: function (db, storename) {
      var keyRange = keyRange || ''
      var store = db.transaction(storename, 'readwrite').objectStore(storename)
      var request = store.openCursor()

      request.onsuccess = function (e) {
        var cursor = e.target.result
        var value, deleteRequest

        if (cursor) {
          if (cursor.key === 1003) {
            deleteRequest = cursor.delete() // 请求删除当前项
            deleteRequest.onerror = function () {
              console.log('游标删除该记录失败')
            }

            deleteRequest.onsuccess = function () {
              console.log('游标删除该记录成功')
            }
          } else {
            cursor.continue()
          }
        }
      }
    },
    // 创建索引
    storeIndex: function (store, indexKey) {
      var index = store.createIndex(indexKey, indexKey, {
        unique:false
      })
      console.log('创建索引' + indexKey + '成功')
    }
  }

  var teachers = [{ 
    id:1001, 
    name:'Byron', 
    age:21 
  }, {
    id:1002, 
    name:'Frank', 
    age:22
  }, {
    id:1003, 
    name:'Aaron', 
    age:23 
  }, {
    id:1004, 
    name:'Aaron', 
    age:24 
  }, {
    id:1005, 
    name:'Byron', 
    age:24 
  }, {
    id:1006, 
    name:'Frank', 
    age:30 
  }, {
    id:1007, 
    name:'Aaron', 
    age:26 
  }, {
    id:1008, 
    name:'Aaron', 
    age:27 
  }]

  INDEXDB.openDB(myDB.name, myDB.version)

  setTimeout(function() {
    // 添加数据
    INDEXDB.addData(myDB.db, myDB.ojstore.name, teachers)

    // 游标更新数据id1002更新其age为38
    INDEXDB.cursorUpdateData(myDB.db, myDB.ojstore.name)

    // 游标删除id为1003的数据
    // INDEXDB.cursorDeleteData(myDB.db, myDB.ojstore.name)

    // 关闭数据库
    // INDEXDB.closeDB(myDB.db)

    // 删除数据库
    // INDEXDB.deletedb(myDB.db)

    /*
     *游标键范围方法调用
     */
    var IDBKeyRange = window.IDBKeyRange || window.webkitIDBKeyRange    

    // 查找1004对象
    // var onlyKeyRange = IDBKeyRange.only(1004)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, onlyKeyRange)
    
    // 查找从1004对象开始
    // var lowerBoundKeyRange = IDBKeyRange.lowerBound(1004)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, lowerBoundKeyRange)
    
    // 查找从1004对象开始不包括1004
    // var lowerBoundKeyRangeTrue = IDBKeyRange.lowerBound(1004, true)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, lowerBoundKeyRangeTrue)
    
    // 查找到1004对象结束
    // var upperBoundKeyRange = IDBKeyRange.upperBound(1004)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, upperBoundKeyRange)
    
    // 查找到1004对象结束不包括1004
    // var upperBoundKeyRangeTrue = IDBKeyRange.upperBound(1004, true)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, upperBoundKeyRangeTrue)
    
    // 查找到1002到1004对象
    // var boundKeyRange = IDBKeyRange.bound(1002, 1004)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, boundKeyRange)
    
    // 查找到1002到1004对象不包括1002
    // var boundKeyRangeLowerTrue = IDBKeyRange.bound(1002, 1004, true)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, boundKeyRangeLowerTrue)
    
    // 查找到1002到1004对象包括1002不包括1004
    // var boundKeyRangeUpperTrue = IDBKeyRange.bound(1002, 1004, false, true)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, boundKeyRangeUpperTrue)
    
    // 查找到1002到1004对象不包括1002不包括1004
    // var boundKeyRangeLTUT = IDBKeyRange.bound(1002, 1004, true, true)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, boundKeyRangeLTUT)

    /*
     *存储键游标查询与索引键游标查询对比
     */
    
    // 存储键游标查询
    // var onlyKeyRange = IDBKeyRange.only(1004)
    // INDEXDB.cursorGetData(myDB.db, myDB.ojstore.name, onlyKeyRange)
    
    // 索引键游标查询
    // var onlyKeyRange = IDBKeyRange.only(30)
    // INDEXDB.cursorGetDataByIndex(myDB.db, myDB.ojstore.name, onlyKeyRange)
  }, 1000)
</script>
```

（3）第三方库1：Dexie.js

Dexie.js 是 indexedDB 封装 SDK，API 简洁强大、错误处理简单而强壮。

> 官方文档：[https://dexie.org/](https://link.segmentfault.com/?url=https%3A%2F%2Fdexie.org%2F)

```javascript
<!doctype html>
<html>
  <head>
    <!-- Include Dexie -->
    <script src="https://unpkg.com/dexie@latest/dist/dexie.js"></script>

    <script>
        // Define your database
        var db = new Dexie("student_database");
        db.version(1).stores({
            students: 'name'
        });

        //
        // Put some data into it
        //
        var data = {
          name: "Byron",
          shoeSize: 24
        }

        db.students.put(data).then (function (){
            //
            // Then when data is stored, read from it
            //
            return db.students.get('Nicolas');
        }).then(function (student) {
            //
            // Display the result
            //
            alert ("Nicolas has shoe size " + student.shoeSize);
        }).catch(function(error) {
           //
           // Finally don't forget to catch any error
           // that could have happened anywhere in the
           // code blocks above.
           //
           alert ("Ooops: " + error);
        });
    </script>
  </head>
</html>
```

（3）第三方库2：indb.js

HelloIndexedDB是对indexedDB复杂api的高级封装，采用Promise，通过高度抽象，让indexedDB的操作极其方便。对于初入门的开发者而言，可以像localStorage一样极其方便的使用indexedDB。

> 官方文档：https://www.tangshuang.net/5681.html



（4）第三方库3：indexeddb-promise

该库是我刚开始掌握indexeddb时候使用的，因为发现indexeddb不支持promise，于是用 indexeddb+promise 关键词在npm上搜索到的包。但是官方文档比较简陋，所以这里不去介绍了。



### 四、哪些公司在使用？

IndexedDB 目前已在美团的部分业务中使用，其他公司尚不清楚，但我相信数据量较大且需要缓存的业务一定可以用到 IndexedDB。

### 五、一些想法：

IndexedDB 是否可以结合 Vuex 使用？（localStorage 或许也能满足需求）

Vuex 将我们需要共享的数据放入一个公共的变量中，以便在路由跳转时重复使用，因为路由跳转是无刷新页面的，所以数据不会丢失。但是当我们刷新或跳转页面时，数据就会丢失。这时候就可以使用 IndexedDB 或 localStorage 将数据保存在其中既能够避免数据丢失，又能避免路由跳转时不必要的存储操作。

```javascript
created () {
    // 页面加载时，读取 IndexedDB
    var data = indexedDB.getData('myDB', 'myStore', 'myData')
    // var data = window.localStorage.getItem('myData')
    var storeData = Object.assign(this.$store.state, data)
    this.$store.replaceState(storeData)

    // 页面刷新时将 vuex 里的信息保存到 IndexedDB
    window.addEventListener('beforeunload', () => {
        indexedDB.updateData('myDB', this.$store.state)
        // window.localStorage.setItem(this.$store.state)
    })
}
```



## 参考文章

- [IndexedDB API 文档](https://wangdoc.com/javascript/bom/indexeddb.html)

- [IndexedDB 应用探索](https://segmentfault.com/a/1190000019130708)

- [HelloIndexedDB：最便捷的indexedDB操作工具](https://www.tangshuang.net/5668.html)

- [Chrome Dev 数据操作教程](https://www.cnblogs.com/junmoye/p/chrome_dev_dataStorage.html)