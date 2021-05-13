---
title: 【笔试面试】华为OD招聘复试摘录
date: 2021-05-13 22:36:00
tags:
  - Web前端 
  - 笔试面试
---



实际上面试问的问题挺多的，有BFC，微任务/宏任务，VUE生命周期，POSITION的取值，VAR变量提升，闭包，this取值有多少种情况，如何修改this指向，工作上最有成就感的事情等等，都是些比较基础的内容，这里不过多说明了。主要说下最后的两道题目。



1. 构造一个二叉树，采用广度优先的方式遍历输出？前中后序呢？



2. 

   ```javascript
   function isBIG (str){
                    return str <= 'Z'
                }
                
               function isSMALL (str){
                    return str > 'Z'
                }
                
               function sortedStr1(arr){
                   var small = [];
                   var big = [];
                   
                   arr.forEach(item => {
                       if (isBIG(item)){
                           big.push(item)
                       } else if(isSMALL(item)){
                           small.push(item)
                       }
                   })
                   
                   console.log(small.concat(big))
               }
               
               function sortedStr2(arr) {
                   arr.sort(function(a, b){
                       if (isBIG(a) && isSMALL(b)) return 1;
                       if (isSMALL(a) && isBIG(b)) return -1;;
                       return 0;
                   })
                   
                   console.log(arr)
               }
               
               function sortedStr3(arr) {
                   // 这里没有实现最小时间复杂度和空间复杂度，因为splice是删除，然后移动后面元素的操作
                   // 未完成
                   for(let i = arr.length - 1; i >= 0; i--){
                       if (isBIG(arr[i])){
                           arr.splice(i, 1);
                           arr.push(arr[i]);
                       }
                   }
                   console.log(arr)
               }
               
               
               sortedStr1(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
               sortedStr2(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
               sortedStr3(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
               
   ```

   