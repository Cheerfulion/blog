---
title: 【前端】canvas使用
date: 2021-06-19 00:06:38
tags:
	- canvas
---



## 简单用法

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
<!-- canvas三要素 -->
<!--
id: 作为唯一标识
width: 画布内容宽度的像素大小
height: 画布内容高度像素大小
这里的width和height与style的宽度和高度是有区别的, style是对元素的放大缩小。
-->
<canvas id="canvas1" width="600" height="600">您的浏览器不支持canvas，请升级浏览器！</canvas>
<!-- 在早期播放视频用flash，如果不支持的话其实也可以用canvas，虽然canvas是H5的标准，但是在11年就有了 -->
<script>
    // 关于这些API的完整使用方法请参考：https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D

    // 画布对象
    var canvas1 = document.getElementById('canvas1')
    // 上下文对象（画笔对象）
    // 2d: 代表二位平面图，还有一个参数是 webgl: 代表3d绘图协议
    // webgl通常用来做3d游戏，但是通常不使用原生API。
    // three.js可以用来开发小型的3d游戏，大型3d游戏可以用虚幻4引擎。国内的H5游戏用白鹭引擎。
    var ctx = canvas1.getContext('2d')
    // 查看对象的设置和方法
    console.log(ctx)

    // // 绘制矩形（x, y, widht, height）
    // ctx.rect(50,50,300,300)
    // ctx.fillStyle = 'aqua'  // 设置填充样式
    // ctx.fill()  // 填充
    // ctx.lineWidth = 10  // 设置描边样式
    // ctx.strokeStyle = 'salmon'  // 设置描边样式
    // ctx.stroke() // 描边

    // // 绘制路径
    // ctx.beginPath()  // 设置开始路径
    // ctx.moveTo(50,50)  // 设置绘制的起点
    // ctx.lineTo(50,300)  // 设置经过某个位置
    // ctx.lineTo(300,100)  // 设置经过某个位置
    // ctx.lineTo(300,250)  // 设置经过某个位置
    // ctx.closePath()  // 设置结束路径（如此可以闭合路径）
    // ctx.lineWidth = 20   // 设置线条的宽度
    // ctx.strokeStyle = 'aqua'  // 设置线条的颜色
    // ctx.lineCap = 'round' // 设置描边线条的结束端点样式（圆角）
    // // ctx.lineJoin = 'round'   // 设置两条线相交时，所创建的拐角类型
    // // ctx.miterLimit = 2   // 设置最大的衔接长度（lineJoin需要是默认值）
    // ctx.stroke()  // 描边
    // // ctx.fill()  // 填充

    // // 绘制圆(圆心在x轴上的坐标x, 圆心在y轴上的坐标y, 半径长度r, 起始角度start, 结束角度stop,是否是逆时针)
    // ctx.beginPath()
    // ctx.arc(300,300,100,0,Math.PI,false)
    // ctx.fillStyle = 'bisque'
    // ctx.fill()
    // ctx.stroke()

    // // 绘制文本（文字内容text, 起点x, 起点y）
    // ctx.font="30px 微软雅黑"
    // ctx.shadowBlur = 20  // 设置阴影
    // ctx.shadowColor = 'rgba(0,0,0,1)'
    // ctx.shadowOffsetX = 10
    // ctx.shadowOffsetY = 10
    // ctx.fillText('Hello First!', 100, 100)  // 绘制实心文本
    // ctx.strokeText('Hello Sencond!', 100, 200)  // 绘制空心文本

    // // 绘制图像 (图片对象， x, y, width, height)
    // // 裁剪绘制图像 (图片对象， 裁剪的x, 裁剪的y, 裁剪的width, 裁剪的height, x, y, width, height)
    // var img = new Image()
    // img.src = 'https://blog.cdn.ionluo.cn/wallpaper/1583472341555.jpg'
    // img.onload = function(){
    //     ctx.drawImage(img, 50, 50, 600, 600)
    // }

    // // 图像数据处理(通过此方法，可以用来做图像识别等功能)
    // var img = new Image()
    // img.src = './1.jpg'
    // img.onload = function(){
    //     // 这里我没有图片，假设是一张
    //     ctx.drawImage(img, 0, 0)
    //     // getImageData(x,y,width,height)
    //     const imgData1 = ctx.getImageData(0,0,5,5)
    //     console.log(imgData1)
    //     const imgData2=ctx.createImageData(100,100)
    //     for (let i=0;i<imgData2.data.length;i+=4) {
    //         imgData2.data[i]=255; // 对应r
    //         imgData2.data[i+1]=0; // 对应g
    //         imgData2.data[i+2]=0; // 对应b
    //         imgData2.data[i+3]=255; // 对应a
    //     }
    //     ctx.putImageData(imgData2,10,10);
    // }

    // // 移动
    // ctx.translate(50, 0)  // 坐标的移动
    // ctx.fillRect(50, 50, 50, 50)


    // // 旋转
    // ctx.rotate(Math.PI/4)  // 旋转45deg，以坐标原点进行旋转
    // ctx.fillRect(0, 0, 50, 50)

    // // 缩放
    // ctx.scale(2, 2)
    // ctx.fillRect(50, 0, 50, 50)
    
    
    // // 设置绘制新形状时应用的合成操作类型
    // // https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation
    // ctx.fillStyle = 'hotpink'
    // ctx.fillRect(100, 100, 200, 200)
    // // 源图像： 您打算放置到画布上的绘图     目标图像： 您已经放置在画布上的绘图
    // // ctx.globalCompositeOperation='source-over'  // 默认值, 在目标图像上显示源图像
    // // ctx.globalCompositeOperation='source-atop' // 在目标图像顶部显示源图像，源图像位于目标图像之外的部分不可见（即显示目标图像+相交部分的源图像）
    // // ctx.globalCompositeOperation='source-in' // 在目标图像中显示源图像，只有目标图像之内的源图像部分会显示，目标图像透明（即显示相交部分的源图像）
    // // ctx.globalCompositeOperation='source-out' // 在目标图像之外显示源图像，只有目标图像之外的源图像部分会显示，目标图像透明（即显示非相交部分的源图像）

    // // ctx.globalCompositeOperation='destination-over' // 在源图像上显示目标图像
    // // ctx.globalCompositeOperation='destination-atop' // 在源图像顶部显示目标图像，目标图像位于源图像之外的部分不可见（即显示源图像+相交部分的目标图像）
    // // ctx.globalCompositeOperation='destination-in' // 在源图像中显示目标图像，只有源图像之内的目标图像部分会显示，源图像透明（即显示相交部分的目标图像）
    // // ctx.globalCompositeOperation='destination-out' // 在源图像之外显示目标图像，只有源图像之外的目标图像部分会显示，源图像透明（即显示非相交部分的目标图像）


    // // ctx.globalCompositeOperation='lighter' // 显示源图像+目标图像-相交部分
    // // ctx.globalCompositeOperation='copy' // 显示源图像，忽略目标图像
    // // ctx.globalCompositeOperation='xor' // 使用异或操作对源图像与目标图像进行组合（效果等同于lighter）
    // ctx.fillStyle = 'deepskyblue'
    // ctx.fillRect(200,200,200,200)

    // // 画笔
    // ctx.save()  // 保存当前画笔的状态（多次save的话，需要恢复到上几次就下面restore几次）
    // ctx.restore()  // 恢复之前保留的画笔状态
</script>
</body>
</html>

```

## 详细用法

https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial



## 应用

### 1. 视频播放

> ps: 通过该方法播放视频也可以增加各式各样的字幕

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
    <canvas id="canvas" width="800" height="500"></canvas>
    <video width="800" autoplay src="https://blog.cdn.ionluo.cn/video/%E4%B8%80%E7%A6%85%E5%B0%8F%E5%92%8C%E5%B0%9A.mp4" controls></video>

    <script>
        var video = document.querySelector('video')
        var canvas = document.querySelector('canvas')

        var interId
        var ctx = canvas.getContext('2d')
        video.onplay = function () {
            interId = setInterval(() => {
                ctx.clearRect(0, 0, 800, 500)
                // 上下黑色
                ctx.fillStyle='#000'
                ctx.fillRect(0, 0, 800, 500)
                ctx.drawImage(video, 0, 25, 800, 450)
                // 字幕(可自行添加滚动逻辑)
                ctx.font = '10px Arail'
                ctx.fillStyle = '#fff'
                ctx.fillText('ionluo', 10, 15)
                // 可以通过ctx.getImageData(x, y, width, height)来进行canvas图像取色
                // 通过ctx.putImageData() 把图像对象修改后放到画布上
            }, 16)
            // 16ms符合普通屏幕的60HZ刷新率，即1/60*1000
        }
        video.onpause = function () {
            clearInterval(interId)
        }
    </script>
</body>
</html>
```

![image-20210618222321634](http://blog.cdn.ionluo.cn/blog/image-20210618222321634.png)

### 2. 时钟

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
    <canvas id="canvas" width="800" height="800"></canvas>

    <script>
        var canvas = document.getElementById('canvas')
        var ctx = canvas.getContext('2d')


        function renderCloak() {
            ctx.clearRect(0,0,800,800)
            // ctx.save()
            // // 将坐标移动到时钟的圆心,坐标向上旋转90度
            // ctx.translate(400, 300)
            // ctx.rotate(-2*Math.PI/4)  // 方便时针的旋转


            // 绘制表盘
            ctx.save()
            ctx.beginPath()
            ctx.translate(400, 300)
            ctx.arc(0, 0, 200, 0, 2*Math.PI)
            ctx.strokeStyle = 'darkgrey'
            ctx.lineWidth = 10
            ctx.stroke()
            ctx.fillStyle = '#01b3b3'
            ctx.fill()
            ctx.closePath()
            ctx.restore()


            // 绘制六十刻度
            for(let i = 0; i < 60; i++){
                ctx.save()
                ctx.beginPath()
                ctx.translate(400, 300)
                ctx.rotate(2*Math.PI/60*i)
                ctx.moveTo(180, 0)
                ctx.lineTo(190, 0)
                ctx.lineWidth= 2
                ctx.strokeStyle = 'orangered'
                ctx.stroke()
                ctx.closePath()
                ctx.restore()
            }


            // 绘制十二刻度
            for(let i = 0; i < 12; i++){
                ctx.save()
                ctx.beginPath()
                ctx.translate(400, 300)
                ctx.rotate(2*Math.PI/12*i)
                ctx.moveTo(180, 0)
                ctx.lineTo(200, 0)
                ctx.lineWidth= 10
                ctx.strokeStyle = 'darkgrey'
                ctx.stroke()
                ctx.closePath()
                ctx.restore()
            }


            // 绘制十二刻度数字(数字倾斜版本，不太好看)
            // for(let i = 1; i <= 12; i++){
            //     ctx.save()
            //     ctx.beginPath()
            //     ctx.translate(400, 300)
            //     ctx.rotate(-2*Math.PI/4)  // 坐标轴应该从12点开始
            //     ctx.rotate(2*Math.PI/12*i)
            //     ctx.font="30px 微软雅黑"
            //     ctx.fillStyle = '#fff'
            //     ctx.fillText(i, i<10?150:140, 12)
            //     ctx.closePath()
            //     ctx.restore()
            // }

            // 绘制十二刻度数字
            for(let i = 0; i < 12; i++){
                ctx.save()
                ctx.beginPath()
                ctx.translate(400, 300)
                ctx.font="30px 微软雅黑"
                ctx.fillStyle = '#fff'
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                // 现在坐标起点是3点钟开始，所以应该让第一个数字是3
                ctx.fillText((i+3)%12===0?12:(i+3)%12, Math.cos(2*Math.PI/12*i)*160, Math.sin(2*Math.PI/12*i)*160)
                ctx.closePath()
                ctx.restore()
            }


            const time = new Date()
            let hour = time.getHours()
            hour = hour%12
            let min = time.getMinutes()
            let sec = time.getSeconds()
            // console.log(hour, min, sec)


            // 绘制时针
            ctx.save()
            ctx.beginPath()
            ctx.translate(400, 300)
            ctx.rotate(-2*Math.PI/4)  // 坐标轴应该从12点开始
            ctx.rotate(2*Math.PI/12*hour+2*Math.PI/60/12*min) // 这里的秒造成的角度偏差极小，忽略
            ctx.moveTo(-15, 0)
            ctx.lineTo(140, 0)
            ctx.lineWidth = 8
            ctx.strokeStyle = 'darkslategray'
            ctx.stroke()
            ctx.closePath()
            ctx.restore()


            // 绘制分针
            ctx.save()
            ctx.beginPath()
            ctx.translate(400, 300)
            ctx.rotate(-2*Math.PI/4)  // 坐标轴应该从12点开始
            ctx.rotate(2*Math.PI/60*min+2*Math.PI/3600*sec)
            ctx.moveTo(-20, 0)
            ctx.lineTo(150, 0)
            ctx.lineWidth = 4
            ctx.strokeStyle = 'darkblue'
            ctx.stroke()
            ctx.closePath()
            ctx.restore()


            // 绘制秒针
            ctx.save()
            ctx.beginPath()
            ctx.translate(400, 300)
            ctx.rotate(-2*Math.PI/4)  // 坐标轴应该从12点开始
            ctx.rotate(2*Math.PI/60*sec)
            ctx.moveTo(-30, 0)
            ctx.lineTo(170, 0)
            ctx.lineWidth = 2
            ctx.strokeStyle = 'red'
            ctx.stroke()
            ctx.closePath()
            ctx.restore()


            // 绘制原点
            ctx.save()
            ctx.beginPath()
            ctx.translate(400, 300)
            ctx.arc(0,0,9,0,2*Math.PI)
            ctx.fillStyle = '#222'
            ctx.fill()
            ctx.closePath()
            ctx.restore()
        }
        setInterval(renderCloak, 1000)
    </script>
</body>
</html>
```

![image-20210619000256108](http://blog.cdn.ionluo.cn/blog/image-20210619000256108.png)

### 3. 刮刮卡

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
        <style>
            #ggk {
                width: 400px;
                height: 100px;
                position: relative;
                cursor: pointer;
            }
            
            #ggk .xxhg {
                position: absolute;
                left: 0;
                top: 0;
                right: 0;
                bottom: 0;
                text-align: center;
                font-size: 30px;
                line-height: 100px;
                color: hotpink;
                user-select: none;
            }
            
            #ggk #canvas {
                position: absolute;
                left: 0;
                top: 0;
                right: 0;
                bottom: 0;
            }
        </style>
	</head>
	<body>
        <div id="ggk">
            <div class="xxhg">谢谢惠顾！</div>
            <canvas id="canvas" width="400" height="100"></canvas>
        </div>
		
        
        <script>
            var ggkDom = document.getElementById('ggk')
            var canvas = document.getElementById('canvas')
            var ctx = canvas.getContext('2d')
            
            ctx.fillStyle = 'darkgray'
            ctx.fillRect(0, 0, 400, 100)
            
            ctx.font = '20px 微软雅黑'
            ctx.fillStyle = '#fff'
            ctx.fillText('刮刮卡', 180, 40)
            
            var isDraw = false
            canvas.onmousedown = function(){
                isDraw = true
            }
            
            canvas.onmousemove = function(e){
                if (isDraw){
                    e = e || window.event;
                    var x = e.clientX - ggkDom.offsetLeft
                    var y = e.clientY - ggkDom.offsetTop
                    ctx.globalCompositeOperation='destination-out'
                    ctx.arc(x, y, 20, 0, 2*Math.PI)
                    ctx.fill()
                }
            }
            
            canvas.onmouseup = function(){
                isDraw = false
            }
            
            // // 设置绘制新形状时应用的合成操作类型
            // // https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation
            // ctx.fillStyle = 'hotpink'
            // ctx.fillRect(100, 100, 200, 200)
            // // 源图像： 您打算放置到画布上的绘图     目标图像： 您已经放置在画布上的绘图
            // // ctx.globalCompositeOperation='source-over'  // 默认值, 在目标图像上显示源图像
            // // ctx.globalCompositeOperation='source-atop' // 在目标图像顶部显示源图像，源图像位于目标图像之外的部分不可见（即显示目标图像+相交部分的源图像）
            // // ctx.globalCompositeOperation='source-in' // 在目标图像中显示源图像，只有目标图像之内的源图像部分会显示，目标图像透明（即显示相交部分的源图像）
            // // ctx.globalCompositeOperation='source-out' // 在目标图像之外显示源图像，只有目标图像之外的源图像部分会显示，目标图像透明（即显示非相交部分的源图像）
            
            // // ctx.globalCompositeOperation='destination-over' // 在源图像上显示目标图像
            // // ctx.globalCompositeOperation='destination-atop' // 在源图像顶部显示目标图像，目标图像位于源图像之外的部分不可见（即显示源图像+相交部分的目标图像）
            // // ctx.globalCompositeOperation='destination-in' // 在源图像中显示目标图像，只有源图像之内的目标图像部分会显示，源图像透明（即显示相交部分的目标图像）
            // // ctx.globalCompositeOperation='destination-out' // 在源图像之外显示目标图像，只有源图像之外的目标图像部分会显示，源图像透明（即显示非相交部分的目标图像）
            
            
            // // ctx.globalCompositeOperation='lighter' // 显示源图像+目标图像-相交部分
            // // ctx.globalCompositeOperation='copy' // 显示源图像，忽略目标图像
            // // ctx.globalCompositeOperation='xor' // 使用异或操作对源图像与目标图像进行组合（效果等同于lighter）
            // ctx.fillStyle = 'deepskyblue'
            // ctx.fillRect(200,200,200,200)
        </script>
	</body>
</html>
```

![image-20210619110221246](http://blog.cdn.ionluo.cn/blog/image-20210619110221246.png)

### 4. 画板

> 以前的关于这个的文章：https://blog.csdn.net/ion_L/article/details/83020855
>
> 这里对原来的一些问题做下优化

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
        <style>
            * {
                margin: 0;
                padding: 0;
                box-sizing: border-box;
            }
            html, body {
                width: 100vw;
                height: 100vh;
            }
            
            
        </style>
	</head>
	<body>
        <!-- 
        个人觉得比较好的一些在线画板：
        https://sumo.app/paint/en
        https://www.suxieban.com/#
        画布功能：撤销，保存，清空
        画笔功能：能够拖动鼠标在页面内绘图，能够设置画笔的粗细，能够设置画笔的颜色
        橡皮擦功能
        能够在任意位置绘制折线，并且可以定制长度
        能够在任意位置绘制圆形，并且可以定制大小
        能够在任意位置绘制矩形，并且可以定制大小
        能够在任意位置插入文字，并且可以定制大小
        能够在任意位置插入图片，并且可以定制大小
        -->
        <div class="menu">
            <div class="btn">画笔</div>
            <div class="btn">橡皮擦</div>
            <div class="btn">矩形</div>
            <div class="btn">圆形</div>
        </div>
        <canvas id="canvas"></canvas>
		
        
        <script>
            var canvas = document.getElementById('canvas')
            var ctx = canvas.getContext('2d')
            
        </script>
	</body>
</html>
```



### 5.随机验证码

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
        <script>
            function generatorCapture(w, h){
                // w: 验证码图片的宽度
                // h: 验证码图片的高度
                // 返回正确的验证码字符串
                
                // 随机数的生成函数
                function rn(min, max){
                    return parseInt(Math.random()*(max-min)+min)
                }
                
                // 随机颜色的生成函数
                function rc(min, max){
                    var r = rn(min, max)
                    var g = rn(min, max)
                    var b = rn(min, max)
                    return `rgb(${r}, ${g}, ${b})`
                }
                
                // var canvas = document.getElementById('canvas')
                var canvas = document.createElement('canvas')
                canvas.width = w
                canvas.height = h
                var ctx = canvas.getContext('2d')
                // ctx.fillStyle = rc(0, 256)  // 这里尽量采用浅色，字体用深色
                ctx.fillStyle = rc(180, 230)
                ctx.fillRect(0, 0, w, h)
                
                // 随机字符串
                var pool = 'ABCDEFGHIGKLMNOPQRSTUVWSYZ1234567890'
                // 验证码
                var result = ''
                
                for(let i = 0; i < 4; i++){
                    // 取出随机的字母或者数字
                    var c = pool[rn(0,pool.length)]
                    result += c
                    // 随机字体大小
                    var fs = rn(18,40)
                    // 随机旋转角度
                    var deg = rn(-30,30)
                    
                    ctx.font = fs + 'px Simhei'
                    ctx.textBaseline = 'top'
                    ctx.fillStyle = rc(80, 150)
                    ctx.save()
                    ctx.translate(w/4*i+15, 15)
                    ctx.rotate(deg*Math.PI/180)
                    ctx.fillText(c, -10, -10)
                    ctx.restore()
                }
                
                
                // 随机生成干扰线
                for(let i = 0; i < 5; i++){
                    ctx.beginPath()
                    ctx.moveTo(rn(0, w), rn(0, h))
                    ctx.lineTo(rn(0, w), rn(0, h))
                    ctx.closePath()
                    ctx.strokeStyle = rc(180, 230)
                    ctx.stroke()
                }
                
                
                // 随机生成干扰圆点
                for(let i = 0; i < 40; i++){
                    ctx.beginPath()
                    ctx.arc(rn(0, w), rn(0, h), 1, 0, 2*Math.PI)
                    ctx.closePath()
                    ctx.fillStyle = rc(150, 230)
                    ctx.fill()
                }
                
                // document.body.appendChild(canvas)
                
                return {
                    imgUrl: canvas.toDataURL(),
                    imgValue: result
                }                                                                                                                                                                                 
            }
            
            var capture = generatorCapture(120, 40);
            var img = document.createElement('img')
            img.src = capture.imgUrl
            document.body.appendChild(img)
        </script>
	</body>
</html>
```

![image-20210619160836742](http://blog.cdn.ionluo.cn/blog/image-20210619160836742.png)