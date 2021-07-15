---
title: 【nodejs】Joi参数校验包使用
tags:
  - nodejs
abbrlink: 14e4b4a2
date: 2021-07-15 10:21:43
---



## 文档地址

Github文档：https://github.com/sideway/joi/tree/v8.0.5 (推荐，搜索方便)

官方文档：https://joi.dev/



## 举个栗子

这里列举一些我常用的验证规则，具体使用参考官方文档。

```javascript
const Joi = require('joi')  // "joi": "^14.3.1",

let schema = Joi.object().keys({ 
    // 3 - 30 个 数字、字符 
    // error 自定义错误信息，可以通过回调函数的err.message拿到
    username: Joi.string().alphanum().min(3).max(30).required().error(new Error('用户名应该为3-30个字符')), 

    // 3 - 30 位 字母数字组合密码 
    password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/), 

    // string || number 都可以通过 
    access_token: [Joi.string(), Joi.number()], 

    // 生日限制 
    birthyear: Joi.number().integer().min(1900).max(2018), 

    // email 限制 
    email: Joi.string().email(), 

    // URI限制 
    website: Joi.string().uri({ scheme: [ 'git', /git+https?/ ] }), 

    // ==== 允许为空/ 否认不允许为空 ==== 
    search: Joi.string().allow(''), 

    // 验证枚举值，如果不传，默认为all 
    type: Joi.string().valid('disabled', 'normal', 'all').default('all'), 

    // 开始时间 会自动格式化 
    startTime: Joi.date().min('1-1-1974').max(Date.now), 

    // 结束时间 必须大于开始时间，小于2100-1-1 
    endTime: Joi.when(Joi.ref('startTime'), { is: Joi.date().required(), then: Joi.date().max('1-1-2100') }), 
    
    // 页码 限制最小值 
    page: Joi.number().integer().min(1).default(1), pageSize: Joi.number().integer().default(8), 
    // deleteWhenLtTen: Joi.number().integer().max(10).strip(), 
   
    // 数组中包含某个字段 && 数字 
    arrayString: Joi.array().items( 
        // 数组中必须包含 name1 
        Joi.string().label('name1').required(), 
        // 数组中必须包含 数字 
        Joi.number().required(), 
        // 除掉【以上类型的以外字段】---数组中可以包含其他类型，如bool
        Joi.any().strip() 
    ), 

    // 数组对象, 如需其参考以上字段 
    arrayObject: Joi.array().items( 
        Joi.object().keys({ 
            age: Joi.number().integer().max(200), 
            sex: Joi.boolean() 
        }) 
    ) 
}).with('username', 'birthyear').without('password', 'access_token');

// with('isA', 'AVal') //意思是，isA 和 AVal 这两字段如果填写了isA，也必须要填写AVal
// without('isA', 'isB'); //意思是 isA 和 isB 只能填写其中一个    
// or('isA', 'isB') //意思是 isA 和 isB 这两字段至少填写其一

// 测试数据
const testData = { Password: "12345678"}
// 验证
// allowUnknown: allows object to contain unknown keys which are ignored. Defaults to false.
Joi.validate(testData, schema, { allowUnknown: true }, function(error, value) {
    if (error) { throw error; }
    console.log(value);
});
```





## 测试

```javascript
const Joi = require('joi')

async function testDemo () {
  const date = Joi.string().isoDate()
  const now = (new Date()).toISOString()
  // 下面得到的也是ISO时间, 也就是接口发送的数据格式，但是这里不能这样子写，因为会多引号而验证失败
  // console.log(JSON.stringify(new Date()))
  await date.validate(now)
}
testDemo()
```

