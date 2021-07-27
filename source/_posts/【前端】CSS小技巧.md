---
title: 【前端】CSS小技巧
tags:
  - 前端
abbrlink: 229e087e
date: 2021-06-25 13:27:26
---





## CSS小技巧



### 1_文字垂直居中

```html
<div class='father'>
    <div class="son">这是要居中的文字</div>
</div>
```



```css
/* 通用方法，单行多行文字均适用 */ 
.father {
    display: table;
    width: 100%;
}
.son {
    display: table-cell;
    height: 100px;
    background: blue;
    color: #fff;
    vertical-align: middle;
    text-align: center;
}
```

```css
/* 单行文字垂直居中 line-height方法 */ 
.son {
    height: 100px;
    line-height: 100px;
    background: blue;
    color: #fff;
    text-align: center;
}
```


```css
/* 单行文字垂直居中 伪元素方法(适用于line-height不确定的时候，如高度是百分比) */ 
.father {
    height: 200px;
}

.son {
    height: 50%;
    background: blue;
    color: #fff;
    text-align: center;
}

.son::before {
    display: inline-block;
    content: "";
    height: 100%;
    vertical-align: middle;
}
```





### 2_Sticky Footer

参考：[Sticky Footer，完美的绝对底部](https://juejin.cn/post/6844903474136612877)

说明：这里仅记录2种比较认可的css实现方式，它们都有一定的缺陷，看具体的业务需求使用。JS实现较为简单，不做说明。

```html
<div class="wrapper">
    <!-- 页面主体内容区域 -->
    <div class="content">
    	lorem
    </div>
    <!-- 需要做到 Sticky Footer 效果的页脚 -->
    <div class="footer"></div>
</div>
```



**实现方案一：absolute**

> 这个方案需指定 html、body 100% 的高度，且 content 的 `padding-bottom` 需要与 footer 的 `height` 一致。也就是说，对于footer高度不确定的情况，该方式就比较头疼了。margin负值，calc也都是和该原理差不多。好处就是兼容好！

```css
html, body {
    height: 100%;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.wrapper {
    position: relative;
    min-height: 100%;
    padding-bottom: 150px;
    box-sizing: border-box;
}
.footer {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 150px;
    background-color: black;
}
```

**实现方案二：Flex布局**

> 允许页脚的高度是可变的，但是有兼容性问题，见：https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex

```css
html {
    height: 100%;
}
body {
    min-height: 100%;
    display: flex;
    flex-direction: column;
}
.content {
    flex: 1;
}
```



> PS: 原文中table的方式有一个缺陷就是容易触发浏览器的重绘回流，这是table布局的固有缺陷，因此不适合用在顶层DOM结构中，会造成性能影响。



### 3_[flex布局](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cad4dc9d88834a8e831c03de2b9e624c~tplv-k3u1fbpfcp-zoom-1.image)最后一行元素左对齐

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			#app {
				width: 800px;
				max-width: 100%;
				border: 5px solid #000;
			}
			
			ul.list {
				list-style: none;
				padding: 0;
				margin: 0;
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				flex-wrap: wrap;
			}
			
			ul.list .item {
				width: 250px;
				height: 250px;
			}
			
			ul.list .item img {
				max-width: 100%;
			}
			
			/* 如果3列以上可以用这个类来填充差的元素 */
			ul.list .item.placeholder {
				width: 250px;
				height: 0;
			}
			
			ul.list::after {
				content: '';
				width: 250px;
				/* height: 0; */
			}
		</style>
	</head>
	<body>
		<div id="app">
			<ul class="list">
				<li class="item" v-for="item in result" :key="item.id">
					<img :src="item.url" alt="">
				</li>
				<!-- <li class="item placeholder"></li> -->
				<!-- <li class="item placeholder"></li> -->
			</ul>
		</div>
		
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.1/vue.js"></script>
		<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.js"></script>
		<script>
			const app = new Vue({
				el: '#app',
				data: {
					result: []
				},
				created(){
					axios.get('https://jsonplaceholder.typicode.com/photos?_limit=6').then(({ data }) => {
						this.result = data
					})
				}
			})
		</script>
	</body>
</html>
```



### 4_使用伪元素扩大点击区域

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			.close {
				position: relative;
				float: right;
			}
			.close::after {
				content: '';
				position: absolute;
				left: -10px;
				right: -10px;
				top: -10px;
				bottom: -10px;
				/* background-color: orange; */
			}
		</style>
	</head>
	<body>
		<div class="wapper">
			<div class="header">
				<span class="close" onclick="alert('你好!')">
					X
				</span>
			</div>
			<div class="content">
				Lorem ipsum dolor sit amet, consectetur adipisicing elit. Debitis nihil ducimus delectus autem magni quam earum dolorum voluptatum tempora doloremque et excepturi quaerat beatae odio inventore. Tempora facere dolorum consequuntur.
			</div>
		</div>
	</body>
</html>
```



### 6_禁止元素行为(类似disabled的效果)

```css
/* 加上类is-disabled */
.is-disabled * {
    color: #6c757d;
    pointer-events: none;
}
```



## 推荐阅读

- [请收下这72个炫酷的CSS技巧](https://juejin.cn/post/6844904031513477128#heading-18)

- [【译】22个必备的CSS小技巧](https://juejin.cn/post/6844903736289001485)