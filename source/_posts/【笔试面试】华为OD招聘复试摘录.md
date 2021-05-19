---
title: 【笔试面试】华为OD招聘复试摘录
tags:
  - Web前端
  - 笔试面试
abbrlink: 6121effd
date: 2021-05-13 22:36:00
---



实际上面试问的问题挺多的，有BFC，微任务/宏任务，VUE生命周期，POSITION的取值，VAR变量提升，闭包，this取值有多少种情况，如何修改this指向，工作上最有成就感的事情等等，都是些比较基础的内容，这里不过多说明了。主要说下最后的两道题目。



1. 构造一个二叉树，采用广度优先的方式遍历输出。

   > 可以扩展到前中后序遍历，根据前中序还原二叉树，根据中后序还原二叉树，线索二叉树，平衡二叉树，哈夫曼树等等。
   
   ```java
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title></title>
   	</head>
   	<body>
            <script>
                // 构造一棵二叉树（Binary Tree）
                // 二叉树(Binary Tree)是一种树形结构，它的特点是每个节点最多只有两个分支节点。
                // 二叉树的存储结构有两种：顺序存储和链式存储。由于顺序存储对于一个很深的且子树都只有一个结点的极端情况会造成大量空间浪费，因此不推荐， 这里介绍链式存储的方式。（满二叉树和完全二叉树可以使用顺序存储）。
                
                // 构造一棵空二叉树
                function InitBiTree(){
                    // 根节点
                    this.root = null;
                    
                    // 创建结点
                    this.createNode = function (value) {
                        return {
                            value: value,
                            lchild: null,  // 指向左节点
                            rchild: null, // 指向右节点
                            parent: null // 指向父节点
                        }
                    }
                    // 插入子树
                    this.insertChild = function(p, LR, c){
                        // p: 需要插入的结点位置，如果不传则是根节点
                        // LR: 插入左边还是右边
                        // c: 子树
                        if (!p && !LR && c){
                            this.root = c;
                            return this.root;
                        }
                        if (!p || !LR || !c) return;
                
                        if (LR === 'left'){
                            p.lchild = c;
                        } else if (LR === 'right'){
                            p.rchild = c;
                        }
                        c.parent = p;
                    }
                    // 删除子树
                    this.deleteChild = function(p, LR){
                        if (!p || !LR) return;
                        
                        if (LR === 'left'){
                            p.lchild = null;
                        } else if (LR === 'right'){
                            p.rchild = null;
                        }
                    }
                    // 清空二叉树
                    this.clearBiTree = function(){
                        this.root = null;
                    }
                    // 销毁二叉树
                    this.DestroyBiTree = function(bitree){
                        bitree = null;
                    }
                    // 根据定义构造二叉树(这里举例两种常用的二叉树)
                    this.createBiTree = function(arr=[], definition='normal'){
                        // 创建普通的二叉树
                        function createNormalTree(arr){
                            for(let i in arr){
                                i = +i;  // 字符串转数字
                                if (arr[i] === null) continue;
                                
                                let node = this.createNode(arr[i]);
                                arr[i] = node;
                                if (i === 0) continue;
                                
                                if (i % 2 === 1) {
                                    // 父节点的左子树
                                    arr[(i-1)/2].lchild = node;
                                    node.parent = arr[(i-1)/2];
                                } else {
                                    // 父节点的右子树
                                    arr[i/2-1].rchild = node;
                                    node.parent = arr[i/2-1];
                                }
                            }
                            this.root = arr[0];
                            return this.root;
                        }
                        // 创建哈夫曼树
                        function createHuffmanTree(){
                            // 未完成
                        }
                        // 创建哈夫曼树
                        function createAVLTree(){
                            // 未完成
                        }
                        // 根据顺序存储（数组）来构造二叉树
                        switch (definition){
                            case 'normal':
                                // 数组的元素是从上往下，从左往右记录的，不存在的为null
                                // 表示一个深度为k的二叉树，需要数组长度2^k - 1
                                createNormalTree.call(this, arr);
                                break;
                            case 'huffman': 
                                // 哈夫曼树(最优二叉树)
                                createHuffmanTree(arr);
                                break;
                            case 'AVL':
                                // 平衡二叉树
                                createAVLTree(arr);
                                break;
                        }
                        return this.root;
                    }
                    // 二叉树是否为空
                    this.biTreeEmpty = function(){
                        return this.root === null;
                    }
                    // 返回二叉树的深度
                    this.biTreeDepth = function(){
                        if (this.root === null) return 0;
                        
                        let depth = 1;
                        function getDepth(node, temp){
                            if(!node.lchild && !node.rchild){
                                depth = temp > depth ? temp : depth;
                                return;
                            }
                            if (node.lchild){
                                getDepth(node.lchild, temp+1);
                            }
                             
                             if (node.rchild){
                                getDepth(node.rchild, temp+1);
                            }
                        }
                        getDepth(this.root, 1);
                        return depth;
                    }
                    // 先序遍历
                    this.preOrderTraverse = function(){
                        const result = [];
                        function preOrder(root){
                            if (root){
                                result.push(root.value)
                                preOrder(root.lchild)
                                preOrder(root.rchild)
                            }
                        }
                        preOrder(this.root);
                        return result;
                    }
                    // 中序遍历
                    this.InOrderTraverse = function(){
                        const result = [];
                        function inOrder(root){
                            if (root){
                                inOrder(root.lchild)
                                result.push(root.value)
                                inOrder(root.rchild)
                            }
                        }
                        inOrder(this.root);
                        return result;
                    }
                    // 后序遍历
                    this.PostOrderTraverse = function(){
                        const result = [];
                        function postOrder(root){
                            if (root){
                                postOrder(root.lchild)
                                postOrder(root.rchild)
                                result.push(root.value)
                            }
                        }
                        postOrder(this.root);
                        return result;
                    }
                    // 层次遍历
                    this.levelOrderTraverse = function(){
                        // 要上班了，先不写了。初步想到可以使用队列
                    }
                    // 根据数据，使用canvas绘制二叉树
                    this.drawPic = function(){
                        
                    }
                }
                
                var biTree = new InitBiTree();
                
                biTree.createBiTree([1,2,3,null,null,6,7])
                console.log('biTree', biTree.root)
                console.log('biTreeDepth', biTree.biTreeDepth())
                // 关于一些非递归的写法，请看：https://www.jianshu.com/p/5e9ea25a1aae
                console.log('preOrderTraverse', biTree.preOrderTraverse())
                console.log('InOrderTraverse', biTree.InOrderTraverse())
                console.log('PostOrderTraverse', biTree.PostOrderTraverse())
                
            </script>
   	</body>
   </html>
   ```
   
   
   
2. 有一个由字母构成的字符数组, 要求让其按照大小写排序，小写在前，大写在后，不改变原始数组中大小写的相对顺序。

   如：`['D', 'a', 'F', 'B', 'c', 'A', 'z']`，输出为`['a', 'c', 'z', 'D', 'F', 'B', 'A']`

   ```javascript
   String.prototype.isBIG = function (){
        return this <= 'Z'
   }
   
   String.prototype.isSMALL = function (){
        return this > 'Z'
   }
   
   // 简单好理解，但空间复杂度高
   function sortedStr1(arr){
       var small = [];
       var big = [];
       
       arr.forEach(item => {
           if (item.isBIG()){
               big.push(item)
           } else if(item.isSMALL()){
               small.push(item)
           }
       })
       
       console.log('sortedStr1', small.concat(big))
   }
   
   // 简单好理解，但时间复杂度高, splice
   function sortedStr2(arr) {
       let len = arr.length;
       for(let i = 0; i < len; i++){
           if (arr[i].isBIG()){
               arr.push(arr[i]);
               arr[i] = null;
           }
       }
       for(let i = len - 1; i >= 0; i--){
           if (arr[i] === null){
               arr.splice(i, 1);
           }
       }
       // let len = arr.length;
       // let i = j = 0;
       // while(j < len){
       //     if(arr[i].isBIG()){
       //         arr.push(arr[i])
       //         arr.splice(i, 1);
       //         i--;
       //     }
       //     i++;
       //     j++;
       // }
       console.log('sortedStr2', arr)
   }
   
   // 借助语言的排序方法
   function sortedStr3(arr) {
       arr.sort(function(a, b){
           if (a.isBIG() && b.isSMALL()) return 1;
           if (a.isSMALL() && b.isBIG()) return -1;;
           return 0;
       })
       
       console.log('sortedStr3', arr)
   }
   
   // 和sortedStr2的原理类似，但是没有借助语言的提供的函数，
   // 不需要把所有后面的元素后移而是在需要的地方进行移动，
   // 是我想到的时间复杂性和空间复杂性最好的方式，有其他方法欢迎提供给我
   function sortedStr4(arr) {
       let b_c = 0; // 当前位置大写字母个数
       let temp;  // 交换使用的临时空间
       let j;  // 临时空间，存储当前需要交换的次数
       for(let i in arr){
           if (arr[i].isSMALL()){
               let j = b_c;
               while(j > 0) {
                   temp = arr[i - j];
                   arr[i - j] = arr[i];
                   arr[i] = temp;
                   j--;
               }
           } else if(arr[i].isBIG()){
               b_c++;  
           }
       }
       
       console.log('sortedStr4', arr)
   }
   
   
   sortedStr1(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
   sortedStr2(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
   sortedStr3(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
   sortedStr4(['D', 'a', 'F', 'B', 'c', 'A', 'z']);
   ```

    