---
title: 【笔试面试】华为OD招聘机考题
tags:
  - Web前端 
  - 笔试面试
abbrlink: a6fbe4e7
date: 2021-05-06 22:27:23
---

> 这里我又发现了一个有意思的功能，原来浏览器也可以做到系统屏幕分享和摄像头录制视频。之后研究下是如何实现的。

## 1. 题目1

> 给定一个正整数数组，检查数组中是否存在满足规则数字组合：A=B+2C，每个元素最多只可用一次，最多只有一组满足规则的数字（其实这里有多组也无妨）。若有满足的数字组合，依次输出A、B、C三个数字，之间用空格隔开；若无满足条件的组合，输出0。
>
> 例如：输入 2 3 5 7 输出 7 3 2

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<script>
    // 思路： 枚举， 另外把一些题干条件的情况排除掉，如A != B != C
    // 这里虽然可以推测出一些其他条件，但是想不出更好的方式，该方式比较粗暴，有其他方式欢迎联系我
    function output(arr) {
        if (arr.length < 3) return 0;

        for (let i = 0; i < arr.length; i++) {
            for (let j = 0; j < arr.length; j++) {
                if (i !== j) {
                    for (let k = 0; k < arr.length; k++) {
                        if (k !== i && k !== j) {
                            if (arr[i] === arr[j] + 2 * arr[k]) {
                                return arr[i] + ' ' + arr[j] + ' ' + arr[k];
                            }
                        }
                    }
                }
            }
        }
        return 0;
    }
    
    function randArr(min, max, len){
        const arr = [];
        for(let i = 0; i < len; i++){
            arr.push(parseInt(Math.random() * (max - min) + min));
        }
        console.log(arr);
        return arr;
        
    }
    console.log(output(randArr(0, 100, 100)))
</script>
</body>
</html>
```





## 2. 题目2

> 只进行一次变换，返回变换后能得到的最小字符串（按照字典序进行比较）

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<script>
// 只进行一次变换，返回变换后能得到的最小字符串（按照字典序进行比较）
function output(str) {
    var str_old = str.split('');
    var str_new = str.split('').sort();
    var old_pos = 0, new_pos = 0, temp;
    for(let i = 0; i < str_old.length; i++){
        if (str_old[i] !== str_new[i]){
            old_pos = i;
            new_pos = str_old.indexOf(str_new[i]);
            break;
        }
    }
    if(old_pos !== new_pos){
        temp = str_old[old_pos];
        str_old[old_pos] = str_old[new_pos];
        str_old[new_pos] = temp;
    }
    return str_old.join('');
}
console.log(output('cvsaza'))
</script>
</body>
</html>
```





## 3. 题目3

![Snipaste_2021-05-06_22-09-41](http://blog.cdn.ionluo.cn/blog/Snipaste_2021-05-06_22-09-41.png)

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<script>
    function output(input){
        var input_arr = input.split('');
        var temp;
        var arr = [1,2,3,4,5,6];
        for(let i = 0; i < input_arr.length; i++){
            temp = [];
            switch (input[i]) {
                case 'L':
                    temp[0] = arr[4];
                    temp[1] = arr[5];
                    temp[2] = arr[2];
                    temp[3] = arr[3];
                    temp[4] = arr[1];
                    temp[5] = arr[0];
                    arr = temp;
                    break;
                case 'R':
                    temp[0] = arr[5];
                    temp[1] = arr[4];
                    temp[2] = arr[2];
                    temp[3] = arr[3];
                    temp[4] = arr[0];
                    temp[5] = arr[1];
                    arr = temp;
                    break;
                case 'F':
                    temp[0] = arr[0];
                    temp[1] = arr[1];
                    temp[2] = arr[4];
                    temp[3] = arr[5];
                    temp[4] = arr[3];
                    temp[5] = arr[2];
                    arr = temp;
                    break;
                case 'B':
                    temp[0] = arr[0];
                    temp[1] = arr[1];
                    temp[2] = arr[5];
                    temp[3] = arr[4];
                    temp[4] = arr[2];
                    temp[5] = arr[3];
                    arr = temp;
                    break;
                case 'A':
                    temp[0] = arr[3];
                    temp[1] = arr[2];
                    temp[2] = arr[0];
                    temp[3] = arr[1];
                    temp[4] = arr[4];
                    temp[5] = arr[5];
                    arr = temp;
                    break;
                case 'C':
                    temp[0] = arr[2];
                    temp[1] = arr[3];
                    temp[2] = arr[1];
                    temp[3] = arr[0];
                    temp[4] = arr[4];
                    temp[5] = arr[5];
                    arr = temp;
                    break;
            }
        }
        return arr.join('')
    }
</script>
</body>
</html>
```