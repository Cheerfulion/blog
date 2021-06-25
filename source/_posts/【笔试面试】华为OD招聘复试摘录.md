---
title: 【笔试面试】华为OD招聘复试摘录
tags:
  - Web前端
  - 笔试面试
abbrlink: 6121effd
date: 2021-05-13 22:36:00
---



实际上面试问的问题挺多的，有BFC，微任务/宏任务，VUE生命周期，POSITION的取值，VAR变量提升，闭包，this取值有多少种情况，如何修改this指向，工作上最有成就感的事情等等，都是些比较基础的内容，这里不过多说明了。主要说下比较难的题目。



## 1. 构造一个二叉树，采用广度优先的方式遍历输出。

> 可以扩展到前中后序遍历，根据前中序还原二叉树，根据中后序还原二叉树，线索二叉树，平衡二叉树，哈夫曼树等等。

```javascript
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

        if (LR === 'left'){
            p.lchild = c;
            c.parent = p;
        } else if (LR === 'right'){
            p.rchild = c;
            c.parent = p;
        }
        return p;
    }
    // 删除子树
    this.deleteChild = function(p, LR){
        if (p && LR === 'left'){
            p.lchild = null;
        } else if (p && LR === 'right'){
            p.rchild = null;
        }
        return p;
    }
    // 清空二叉树
    this.clearBiTree = function(){
        this.root = null;
    }
    // 销毁二叉树
    this.DestroyBiTree = function(bitree){
        bitree = null;
    }
    // 根据定义构造二叉树(这里举例三种常用的二叉树)
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
    this.biTreeDepth = function(root){
        function getDepth(root){
            if (root === null) return 0;
            let left = getDepth(root.lchild);
            let right = getDepth(root.rchild);
            return (left > right) ? left + 1 : right + 1;
        }
        return getDepth(root || this.root);
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
    // 层次遍历（从上到下，从左到右）
    this.levelOrderTraverse = function(){
        const result = [];
        const queue = []; // 这里不使用栈(stack)， 而是队列(queue)
        queue.push(this.root);
        while(queue.length){
            // js数组模拟队列，但是这样子对于js数组来说需要不断开辟新内存，删除旧内存，比较费性能。因此这里简单的方式可以使用索引来访问。
            let node = queue.shift();
            result.push(node.value);
            node.lchild && queue.push(node.lchild);
            node.rchild && queue.push(node.rchild);
        }
        return result;
    }
    // 根据数据，使用canvas绘制二叉树
    this.drawPic = function(){
        // 思路：拿到二叉树的深度，将二叉树当成完全二叉树绘制
        // 这里可以使用isPointInPath来做一个可以拖拽的二叉树，但是除了交互，对于算法并没有什么意义，这里就不去实现了，纯属记录下
        const depth = this.biTreeDepth(this.root)
        // console.log('depth', depth)

        const depthHeight = 50;  // 二叉树每层的高度
        const elemeWidth = 60;   // 二叉树元素之间的间距
        const nodeRadius = 10;   // 元素结点的半径
        const width = Math.pow(2, depth-1) * elemeWidth
        const height = depth * depthHeight
        const canvas = document.createElement('canvas')
        canvas.width = width + 10  // 加上间距(padding)
        canvas.height = height + 10 // 加上间距(padding)
        const ctx = canvas.getContext('2d')

        // ctx.save()
        // 修改下坐标，让canvas留些间距
        ctx.translate(5,5)
        // 层次遍历，方便标序号
        const queue = []; // 这里不使用栈(stack)， 而是队列(queue)
        queue.push({
            x: width / 2,
            y: nodeRadius,
            element: this.root,
            value: 1
        });
        while(queue.length){
            // js数组模拟队列，但是这样子对于js数组来说需要不断开辟新内存，删除旧内存，比较费性能。因此这里简单的方式可以使用索引来访问。
            let node = queue.shift();

            // 连线
            if (node.element.lchild){
                queue.push({
                    x: node.x - elemeWidth/2,
                    y: node.y + depthHeight,
                    element: node.element.lchild,
                    value: node.value + 1
                })
                ctx.beginPath()
                ctx.moveTo(node.x, node.y)
                ctx.lineTo(node.x - elemeWidth/2, node.y + depthHeight)
                ctx.stroke()
                ctx.closePath()
            }
            if (node.element.rchild) {
                queue.push({
                    x: node.x + elemeWidth/2,
                    y: node.y + depthHeight,
                    element: node.element.rchild,
                    value: node.value + 2
                })  
                ctx.beginPath()
                ctx.moveTo(node.x, node.y)
                ctx.lineTo(node.x + elemeWidth/2, node.y + depthHeight)
                ctx.stroke()
                ctx.closePath()
            } 


            // 画圆
            ctx.beginPath()
            ctx.arc(node.x, node.y, nodeRadius, 0, 2*Math.PI)
            ctx.stroke()
            ctx.fillStyle = '#444'
            ctx.fill()
            ctx.closePath()

            // 填序号
            ctx.beginPath()
            ctx.font= nodeRadius + 2 + "px 微软雅黑"
            ctx.fillStyle = '#fff'
            ctx.textAlign = "center";
            ctx.textBaseline = "middle";
            // 现在坐标起点是3点钟开始，所以应该让第一个数字是3
            ctx.fillText(node.value, node.x, node.y + 1)
            ctx.closePath()
        }

        document.body.appendChild(canvas)
    }
}

var biTree = new InitBiTree();

biTree.createBiTree([1,2,3,null,null,6,7]);
console.log('biTree', biTree.root);
console.log('biTreeDepth', biTree.biTreeDepth());
// 关于一些非递归的写法，请看：https://www.jianshu.com/p/5e9ea25a1aae
console.log('preOrderTraverse', biTree.preOrderTraverse());
console.log('InOrderTraverse', biTree.InOrderTraverse());
console.log('PostOrderTraverse', biTree.PostOrderTraverse());
console.log('levelOrderTraverse', biTree.levelOrderTraverse());
// canvas绘制二叉树
biTree.drawPic()
```



## 2.有一个由字母构成的字符数组, 要求让其按照大小写排序，小写在前，大写在后，不改变原始数组中大小写的相对顺序。

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

 



## 3.用一个for循环输出数组中出现次数最多的一个元素和它出现的次数。如果出现次数最多的元素不只一个，又该如何应对？

```javascript
function generateArray(len){
    var arr = [];
    for(let i = 0; i < len; i++){
        arr.push(parseInt(Math.random() * 10));
    }
    return arr;
}
// 解法一： max： 出现的最多次数  value: 出现次数最多的数组元素
// 如果有多个元素，则应该再用一个for循环找出放入数组中。
// 该方式的优点是：时间复杂度低（n）且数组允许是任意的数据类型。缺点是引入了新的空间
function solution1(){
    var arr = generateArray(10);
    console.log(arr)

    var map = new Map();  // 如果是简单数据类型的情况用object， 如果是对象类型推荐用weakmap
    // weakmap推荐大佬的文章了解详情：https://segmentfault.com/a/1190000015774465
    var max = 0, value, temp;

    for(let i in arr){
        if (!map.has(arr[i])){
            map.set(arr[i], 1);
            if(1 > max){
                max = 1;
                value = arr[i];
            }
        } else{
            temp = map.get(arr[i]);
            map.set(arr[i], ++temp);
            if(temp > max){
                max = temp;
                value = arr[i];
            }
        }
    }

    return [value, max]
}
console.log(solution1())

// 解法2：max： 出现的最多次数  value: 出现次数最多的数组元素
// 该方式的优点是：逻辑清晰，空间复杂度低。缺点是：不适用于对象类型，且用了join和RegExp，所以时间复杂度会稍高
function solution2(){
    var arr = generateArray(10);
    console.log(arr)

    var str = ',' + arr.join(',') + ',';
    var max = 0, value;
    for(let i in arr){
        let count = str.match(new RegExp(','+arr[i]+',', 'g'));
        if(!connt) continue;

        count = count.length;
        if (count > max){
            max = count;
            value = arr[i];
        }
    }
    return [value, max]
}
console.log(solution2())
```



## 4. JS中this的5种情况

参考：https://www.bbsmax.com/A/kjdwpp8wzN/

1. 给元素的某个事件行为绑定方法，事件触发，方法执行，此时方法中的this一般都是当前元素本身：

```javascript
// <div id="div"></div>

div.onclick = function() {
    console.log(this); 
};
div.addEventListener('click', function () {
    console.log(this);  
}, false);
```

这里边有个特殊情况就是DOM2级绑定事件

```javascript
div.attachEvent('onclick',function anonymous(){
    console.log(this); //=>window
});
```

2. 普通函数执行，它里边的this是谁，取决于方法执行前面是否有“.”，有的话，“.”前面是谁this就是谁，没有的话并且是在非严格模式下this就是window，严格模式下是undefined：

```javascript
function fn() {
    console.log(this);
}
let obj = {
    name: 'OBJ',
    fn: fn
};
fn(); //window
obj.fn(); //{name: 'OBJ',fn: fn}
```

3. 构造函数执行(也即是new执行)，函数中的this是当前类的实例：

```javascript
function F() {
    console.log(this);
}
let f = new F; // F {}
```

4. 箭头函数中没有this，所用到的this都是其上下文中的this(或者说是上级上下文)：

```javascript
let obj = {
    fn: () => {
        console.log(this);
    }
}
obj.fn(); //window
let obj = {
    fn: function () {
        setTimeout(_ => {
            console.log(this);
        }, 1000);
    }
};
obj.fn(); //obj
```

5. 基于call/apply/bind可以改变函数中this的指向：

```javascript
let obj = {
    fn: function(){
        console.log(this);
        }
}
obj.fn(); //obj
obj.fn.call(12); // Number {12}
```