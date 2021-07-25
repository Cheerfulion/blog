---
title: 【nodejs】执行cmd命令获取服务器信息
tags:
  - nodejs
abbrlink: dfe2ecc1
date: 2021-07-19 12:01:14
---





## 正文

```javascript
// 参考文章：http://www.libs.org.cn/index.php?m=content&c=index&a=show&catid=78&id=195
// https://www.cnblogs.com/workstation-liuni

const os = require('os')
// 执行系统命令
var process = require('child_process')
// 编码转换（中文系统输出中文字符会出现乱码的情况，所以需要转换）
// 也可以使用模块 encoding， anguowang/p/6278842.html
const iconv = require('iconv-lite')

var dealTime = (seconds) => {
  // var seconds = seconds | 0
  // var day = (seconds / (3600 * 24)) | 0
  // var hours = ((seconds - day * 3600) / 3600) | 0
  // var minutes = ((seconds - day * 3600 * 24 - hours * 3600) / 60) | 0
  // var second = seconds % 60;
  // (day < 10) && (day = '0' + day);
  // (hours < 10) && (hours = '0' + hours);
  // (minutes < 10) && (minutes = '0' + minutes);
  // (second < 10) && (second = '0' + second)
  // return [day, hours, minutes, second].join(':')

  // 272445
  var day, hour, minute
  var result = []
  minute = Math.floor(seconds / 60)
  seconds = seconds % 60
  hour = Math.floor(minute / 60)
  minute = minute % 60
  day = Math.floor(hour / 24)
  hour = hour % 24

  day && result.push(day + '天')
  hour && result.push(hour + '小时')
  minute && result.push(minute + '分钟')
  seconds && result.push(seconds + '秒')
  return result.join('')
}

var dealMem = (mem) => {
  var G = 0
  var M = 0
  var KB = 0;
  (mem > (1 << 30)) && (G = (mem / (1 << 30)).toFixed(2));
  (mem > (1 << 20)) && (mem < (1 << 30)) && (M = (mem / (1 << 20)).toFixed(2));
  (mem > (1 << 10)) && (mem > (1 << 20)) && (KB = (mem / (1 << 10)).toFixed(2))
  return G > 0 ? G + 'G' : M > 0 ? M + 'M' : KB > 0 ? KB + 'KB' : mem + 'B'
}

// cpu架构
const arch = os.arch()
console.log('cpu架构：' + arch)

// 操作系统内核
const kernel = os.type()
console.log('操作系统内核：' + kernel)

// 操作系统平台
const pf = os.platform()
console.log('平台：' + pf)

// 系统开机时间
const uptime = os.uptime()
console.log('系统运行时间：' + dealTime(uptime))

// 主机名(中文主机名存在乱码情况)
const buf = iconv.decode(os.hostname(), 'GB18030')
const hn = iconv.encode(buf, 'UTF-8').toString().trim()
console.log('主机名：' + hn)

// 用户名
const username = os.userInfo().username
console.log('用户名：' + username)

// 主目录
const hdir = os.homedir()
console.log('主目录：' + hdir)

// 内存
const totalMem = os.totalmem()
const freeMem = os.freemem()
console.log('内存大小：' + dealMem(totalMem) + ' 空闲内存：' + dealMem(freeMem))

// cpu
const cpus = os.cpus()
console.log('*****cpu信息*******')
cpus.forEach((cpu, idx, arr) => {
  var times = cpu.times
  console.log(`cpu${idx}：`)
  console.log(`型号：${cpu.model}`)
  console.log(`频率：${cpu.speed}MHz`)
  console.log(`使用率：${((1 - times.idle / (times.idle + times.user + times.nice + times.sys + times.irq)) * 100).toFixed(2)}%`)
})

// 网卡
console.log('*****网卡信息*******')
const networksObj = os.networkInterfaces()
for (const nw in networksObj) {
  const objArr = networksObj[nw]
  console.log(`\r\n${nw}：`)
  objArr.forEach((obj, idx, arr) => {
    console.log(`地址：${obj.address}`)
    console.log(`掩码：${obj.netmask}`)
    console.log(`物理地址：${obj.mac}`)
    console.log(`协议族：${obj.family}`)
  })
}

// 执行系统命令
console.log('*****同步执行系统命令（Window）*******')
var cmd = 'net user %username%'
var rlt = process.execSync(cmd).toString()
// ？ 这里不清楚编码到底是什么
const buf2 = iconv.decode(rlt, 'GB18030')
rlt = iconv.encode(buf2, 'UTF-8').toString()
console.log(rlt)

// 执行系统命令
console.log('*****异步执行系统命令（Window）*******')
var cmd = 'net user %username%'
process.exec(cmd, function (error, stdout, stderr) {
  console.log('error', error)
  // ？ 这里不清楚编码到底是什么
  stdout = iconv.decode(stdout, 'GB18030')
  stdout = iconv.encode(stdout, 'UTF-8').toString()
  console.log('stdout', stdout)
  console.log('stderr', stderr)
})
```



## 参考文章

- http://www.libs.org.cn/index.php?m=content&c=index&a=show&catid=78&id=195
- https://www.cnblogs.com/workstation-liuni