---
title: 【前端】如何中断Promise的链式调用
tags:
  - 前端
abbrlink: e0d4478a
date: 2021-04-12 10:54:17
---

Promise的then用起来很方便，通过链式调用可以让代码看起来更清晰。

```javascript
let p = new Promise((resolve, reject) => {
  resolve('step1');
});

p.then(data => {
  console.log(data);
  return 'step2';
}).then(data => {
  console.log(data);
  return 'step3';
}).then(data => {
  console.log(data);
  return 'step4';
}).catch(reason => {
  console.log(reason);
}).finally(() => {
  console.log('finished.');
});
```
对应的结果是：
```
"step1"
"step2"
"step3"
"finished."  /*return 'step4'后面没有继续处理了，所以不会打印*/
```

可是，如果我们在处理step2的时候，因为条件满足了，后面的步骤不需要执行，这时候就需要去中断后续的调用链。

方法一：通过抛出一个异常来终止
```javascript
let needBreak = true;
let p = new Promise((resolve, reject) => {
  resolve('step1');
});

p.then(data => {
  console.log(data);
  return 'step2';
}).then(data => {
  console.log(data);
  if (needBreak) {
    throw "we need break";
  }
  return 'step3';
}).then(data => {
  console.log(data);
  return 'step4';
}).catch(reason => {
  console.log('got error:', reason);
}).finally(() => {
  console.log('finished.');
});
```
这时候的输出就成了这样：

```
step1
step2
got error: we need break
finished.
```

方法二：通过reject来中断
```javascript
new Promise((resolve, reject) => {
    resolve('')
}).then(res => {
    console.log(1)
}).then(res => {
    console.log(2)
}).then(res => {
    console.log(3)
    return Promise.reject('333')
}).then(res => {
    console.log(4)
}).then(res => {
    console.log(5)
}).catch(e => console.log(e))
.finally(() => {
  console.log('finished.');
});
```
输出结果：
```
step1
step2
break without exception.
finished.
```