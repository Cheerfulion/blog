---
title: 【前端】canvas使用
tags:
  - canvas
abbrlink: 78bd14cd
date: 2021-06-19 00:06:38
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



## 注意事项

- [挖坑指南: canvas元素的宽高属性和css中的宽高(大型翻车现场..)](https://blog.csdn.net/weixin_39015132/article/details/86295280)



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



### 6. canvas生成自定义图片

> 通常用于自动生成用户头像，如钉钉注册后默认的头像

```javascript

export default function getImgUrl (img) {
  const canvas = document.createElement('canvas')
  const ctx = canvas.getContext('2d')

  img = img || {}
  canvas.width = img.width || 50
  canvas.height = img.height || 50

  ctx.rect(0, 0, canvas.width, canvas.height)
  ctx.fillStyle = img.backgroundColor || '#fff'
  ctx.fill()

  if (img.text) {
    ctx.font = img.textFont || '50px 微软雅黑'
    ctx.textAlign = 'center'
    ctx.textBaseline = 'middle'
    ctx.fillStyle = img.textColor || '#444'

    var textWidth = ctx.measureText(img.text).width
    var scaled = 1
    if (textWidth > canvas.width) {
      scaled = canvas.width / textWidth * 0.8 // 不要占满
      ctx.scale(scaled, scaled)
    }
    // ctx.fillText(img.text, 0, 0, canvas.width)
    ctx.fillText(img.text, canvas.width / 2 / scaled, canvas.height / 2 / scaled)
  }
  //   document.body.appendChild(canvas)
  return canvas.toDataURL()
}

```





### 7. 粒子碰撞





### 8. canvas特效或游戏开发

> 下面的代码来自TechbrooD.com， 领先的沉浸式互联网内容门户，一站式学习、创作和展示。

#### [HTML5 Canvas五彩纸屑粒子动画特效](https://wow.techbrood.com/fiddle/36443)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            * {
                margin: 0;
                padding: 0;
                box-sizing: border-box;
            }
            body {
                width: 100vw;
                min-height: 100vh;
            }
            #canvas {
                display: block;
                cursor: pointer;
            }
        </style>
	</head>
	<body>
        <canvas id="mycanvas"></canvas>
        
        <script>
            const getRandom = (min, max) => {
              return Math.random() * (max - min) + min
            }
            
            const getRandomInt = (min, max) => {
              return Math.floor(Math.random() * (max - min)) + min
            }
            
            const getRandomColor = () => {
              const colors = [
                'rgba(231, 76, 60, 1)', // red
                'rgba(241, 196, 15, 1)', // yellow
                'rgba(46, 204, 113, 1)', // green
                'rgba(52, 152, 219, 1)', // blue
                'rgba(155, 89, 182, 1)' // purple
              ]
            
              return colors[getRandomInt(0, colors.length)]
            }
            
            // Particle
            
            class Particle {
              
              constructor (system, x, y) {
                this.system = system
                this.universe = this.system.world.universe
                this.x = x
                this.y = y
                this.color = getRandomColor()
                this.life = 1
                this.aging = getRandom(0.99, 0.999) // 0.99, 0.999 || 0.999, 0.9999
                
                this.r = getRandomInt(8, 16)
                this.speed = getRandom(3, 8)
                this.velocity = [
                  getRandom(-this.speed, this.speed),
                  getRandom(-this.speed, this.speed)
                ]
              }
              
              update (dt) {
                this.life *= this.aging
                
                if (
                  this.r < 0.1
                  || this.life === 0
                  || this.x + this.r < 0
                  || this.x - this.r > this.universe.width
                  || this.y + this.r < 0
                  || this.y - this.r > this.universe.height
                ) {
                  this.system.removeObject(this)
                }
                
                this.r *= this.life
                this.x += this.velocity[0]
                this.y += this.velocity[1]
              }
              
              render (ctx) {
                // Main circle
                
                ctx.fillStyle = this.color
                ctx.beginPath()
                ctx.arc(this.x, this.y, this.r, 0, 2 * Math.PI, false);
                ctx.fill()
                ctx.closePath()
            
                const r = this.color.match(/([0-9]+)/g)[0]
                const g = this.color.match(/([0-9]+)/g)[1]
                const b = this.color.match(/([0-9]+)/g)[2]
            
                // Gradient
                
                const spread = 1.5
                const gradient = ctx.createRadialGradient(
                  this.x, this.y, this.r,
                  this.x, this.y, this.r * spread
                );
                gradient.addColorStop(0, `rgba(${r}, ${g}, ${b}, 0.3)`);
                gradient.addColorStop(1, `rgba(${r}, ${g}, ${b}, 0)`);
            
                ctx.globalCompositeOperation = 'lighter'
                ctx.fillStyle = gradient
                ctx.beginPath()
                ctx.arc(this.x, this.y, this.r * spread, 0, 2 * Math.PI, false)
                ctx.fill()
                ctx.closePath()
                ctx.globalCompositeOperation = 'source-over'
                
                // Aberration
                
                const offset = this.r * 0.5
                const color = `rgba(${g}, ${b}, ${r}, 0.3)`
                
                ctx.globalCompositeOperation = 'lighter'
                ctx.fillStyle = color
                ctx.beginPath()
                ctx.arc(this.x + offset, this.y + offset, this.r, 0, 2 * Math.PI, false)
                ctx.fill()
                ctx.closePath()
                ctx.globalCompositeOperation = 'source-over'
              }
              
            }
            
            // Crown
            
            class Crown {
              
              constructor (system, x, y) {
                this.system = system
                this.x = x
                this.y = y
                this.r = getRandomInt(15, 20) // 5, 20
                this.mod = 1.1
                this.life = 1
                this.aging = getRandom(0.83, 0.88)
                this.speed = getRandom(4, 5)
                
                this.color = {
                  r: getRandomInt(236, 242),
                  g: getRandomInt(196, 241),
                  b: getRandomInt(195, 242)
                }
            
                this.angle1 = Math.PI * getRandom(0, 2)
                this.angle2 = this.angle1 + Math.PI * getRandom(0.1, 0.5)
              }
              
              update (dt) {
                this.life *= this.aging
                
                if (this.life <= 0.0001) this.system.removeObject(this)
                
                this.r += Math.abs(1 - this.life) * this.speed
                
                this.x1 = this.x + this.r * Math.cos(this.angle1)
                this.y1 = this.y + this.r * Math.sin(this.angle1)
                
                this.angle3 = this.angle1 + ((this.angle2 - this.angle1) / 2)
                this.x2 = this.x + this.r * this.mod * Math.cos(this.angle3)
                this.y2 = this.y + this.r * this.mod * Math.sin(this.angle3)
              }
            
              render (ctx) {
                const gradient = ctx.createRadialGradient(
                  this.x, this.y, this.r * 0.9,
                  this.x, this.y, this.r
                );
                gradient.addColorStop(0, `rgba(${this.color.r}, ${this.color.g}, ${this.color.b}, ${this.life})`);
                gradient.addColorStop(1, `rgba(${this.color.r}, ${this.color.g}, ${this.color.b}, ${this.life * 0.5})`);
                
                ctx.fillStyle = gradient
                ctx.beginPath()
                ctx.arc(this.x, this.y, this.r, this.angle1, this.angle2, false)
                ctx.quadraticCurveTo(this.x2, this.y2, this.x1, this.y1)
                ctx.fill()
                ctx.closePath()
              }
              
            }
            
            // Explosion
            
            class Explosion {
              
              constructor (world, x, y) {
                this.world = world
                this.x = x
                this.y = y
                this.objects = []
            
                let particles = getRandomInt(30, 80) // 10, 30 amount of particles
                let crowns = particles * getRandom(0.3, 0.5)
            
                while (crowns-- > 0) this.addCrown()
                while (particles-- > 0) this.addParticle()
              }
              
              update (dt) {
                this.objects.forEach((obj) => {
                  if (obj) obj.update(dt)
                })
                
                if (this.objects.length <= 0) {
                  this.world.clearExplosion(this)
                }
              }
              
              render (ctx) {
                this.objects.forEach((obj) => {
                  if (obj) obj.render(ctx)
                })
              }
              
              addCrown () {
                this.objects.push(new Crown(this, this.x, this.y))
              }
              
              addParticle () {
                this.objects.push(new Particle(this, this.x, this.y))
              }
              
              removeObject (obj) {
                const index = this.objects.indexOf(obj)
                
                if (index !== -1) {
                  this.objects.splice(index, 1)
                }
              }
              
            }
            
            // World
            
            class ConfettiWorld {
              
              init () {        
                this.objects = []
                window.addEventListener('click', this.explode.bind(this))
                
                // Initial explosion
                let counter = 3
                while (counter-- > 0) {
                  this.explode({
                    clientX: this.universe.width / 2,
                    clientY: this.universe.height / 2
                  }) 
                }
              }
              
              update (dt) {
                this.objects.forEach((obj) => {
                  if (obj) obj.update(dt)
                })
            
                const amount = this.objects.reduce((sum, explosion) => {
                  return sum += explosion.objects.length
                }, 0)
              }
              
              render (ctx) {
                this.objects.forEach((obj) => {
                  if (obj) obj.render(ctx)
                })
              }
              
              explode (event) {
                const x = event.clientX
                const y = event.clientY
            
                this.objects.push(new Explosion(this, x, y))
              }
              
              clearExplosion (explosion) {
                const index = this.objects.indexOf(explosion)
                
                if (index !== -1) {
                  this.objects.splice(index, 1)
                }
              }
              
            }
            
            // Time
            
            class Time {
              
              constructor () {
                this.now = 0 // current tick time
                this.prev = 0 // prev tick time
                this.elapsed = 0 // elapsed time from last tick
                this.delta = 0 // time from last update
                this.fps = 60 // desired fps
                this.step = 1 / 60 // step duration
              }
              
              update (time) {
                this.now = time
                this.elapsed = (this.now - this.prev) / 1000
                this.prev = this.now
                this.delta += this.elapsed
              }
              
              raf (func) {
                window.requestAnimationFrame(func)
              }
              
              hasFrames () {
                return this.delta >= this.step
              }
              
              processFrame () {
                this.delta -= this.step
              }
              
            }
            
            // Canvas
            
            class Universe {
              
              constructor (element) {
                this.el = element
                this.ctx = this.el.getContext('2d')
                this.pixelRatio = window.devicePixelRatio
                this.time = new Time
                
                this.worlds = {}
                this.world = null // current state
            
                this.updateSize()
                window.addEventListener('resize', this.updateSize.bind(this))
                
                this.addWorld('confetti', ConfettiWorld)
                this.setWorld('confetti')
                
                this.start()
              }
              
              start () {
                this.time.raf(this.tick.bind(this))
              }
              
              tick (time) {
                this.time.update(time)
            
                if (this.time.hasFrames()) {
                  this.update()
                  this.time.processFrame()
                }
                
                this.render()
                this.time.raf(this.tick.bind(this))
              }
              
              update () {
                this.world.update(this.time.step)
              }
              
              render () {
                const gradient = this.ctx.createLinearGradient(0, 0, this.width, this.height)
                gradient.addColorStop(0, '#34495e')
                gradient.addColorStop(1, '#2c3e50')
                
                this.ctx.clearRect(0, 0, this.width, this.height)
                // this.ctx.fillStyle = '#2c3e50'
                this.ctx.fillStyle = gradient
                this.ctx.fillRect(0, 0, this.width, this.height)
                this.world.render(this.ctx)
              }
              
              // Helpers
              
              updateSize () {
                this.width = window.innerWidth
                this.height = window.innerHeight
                this.el.width = this.width * this.pixelRatio
                this.el.height = this.height * this.pixelRatio
                this.el.style.width = window.innerWidth + 'px'
                this.el.style.height = window.innerHeight + 'px'
                this.ctx.scale(this.pixelRatio, this.pixelRatio)
              }
              
              addWorld (worldName, World) {
                this.worlds[worldName] = new World()
                this.worlds[worldName].universe = this
                this.worlds[worldName].init()
              }
              
              setWorld (worldName) {
                this.world = this.worlds[worldName]
              }
              
            }
            
            // Main
            
            console.clear()
            const element = document.querySelector('#mycanvas')
            window.Canvas = new Universe(element)
        </script>
	</body>
</html>
```

#### [HTML5 Canvas可交互的粒子碰撞实验](https://wow.techbrood.com/fiddle/37427)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            html,
            body {
                margin: 0;
                overflow: hidden;
                cursor: pointer;
            }
            .note {
                position: absolute;
                color: rgba(249, 241, 204, .5);
                left: 0;
                right: 0;
                bottom: 20px;
                margin: auto;
                text-align: center;
                user-select: none;
                font-family: Arial;
            }
        </style>
	</head>
	<body>
        <canvas id="canvas"></canvas>
        <span class="note">Click to add particles</span>
        <script src="https://wow.techbrood.com//uploads/161101/dat.gui.min.js"></script>
        
        
        <script>
            // 性能不太好，会卡
            
            function getRandomInt(min, max, except) {
                var i = Math.floor(Math.random() * (max - min + 1)) + min;
                if (typeof except == "undefined") return i;
                else if (typeof except == "number" && i == except) return getRandomInt(min, max, except);
                else if (typeof except == "object" && (i >= except[0] && i <= except[1])) return getRandomInt(min, max, except);
                else return i;
            }
            
            function isEven(n) {
                return n == parseFloat(n) ? !(n % 2) : void 0;
            }
            
            function degToRad(deg) {
                return deg * (Math.PI / 180);
            }
            
            var colors = [];
            colors[0] = [];
            colors[0]["background"] = "black"; // "#272822";
            colors[0][1] = "#e6db74";
            colors[0][2] = "#f92672";
            colors[0][3] = "#53d2ef";
            colors[0][4] = "#fd8b20";
            colors[0][5] = "#a6e22e";
            
            function Person(id) {
                this.id = id;
                this.width = getRandomInt(5, 12);
                this.height = this.width;
                this.x = getRandomInt(0, (canvas.width - this.width));
                this.y = getRandomInt(0, (canvas.height - this.height));
                this.color = "white";
                this.direction = getRandomInt(0, 360);
                this.isHit = false;
                this.nextActionIndex = 0;
                this.job = 'unemployed';
                this.shadowBlur = getRandomInt(10, 50);
            }
            
            Person.prototype.checkIfPositionIsEmpty = function() {
                var initialPosition = [];
                initialPosition = this.readMatrix(this.x, this.y, this.width, this.height, this.id);
                if (initialPosition.length == 0 || typeof initialPosition[0] == 'undefined') {
                    this.writeMatrix(this.x, this.y, this.width, this.height, this.id);
                    return;
                } else {
                    this.x = getRandomInt(0, (canvas.width - this.width));
                    this.y = getRandomInt(0, (canvas.height - this.height));
                    this.checkIfPositionIsEmpty();
                }
            }
            
            Person.prototype.nextAction = function() {
                switch (this.nextActionIndex) {
                    case 0:
                        return this.walk();
                    case 1:
                        return this.stop();
                    default:
                        this.walk();
                }
            }
            
            Person.prototype.stop = function() {
                this.update();
            }
            
            Person.prototype.walk = function() {
            
                var next_x = this.x + Math.cos(degToRad(this.direction)) * params.speed,
                    next_y = this.y + Math.sin(degToRad(this.direction)) * params.speed;
            
                // Limites du canvas
                if (next_x >= (canvas.width - this.width) && (this.direction < 90 || this.direction > 270)) {
                    next_x = canvas.width - this.width;
                    this.direction = getRandomInt(90, 270, this.direction);
                }
                if (next_x <= 0 && (this.direction > 90 && this.direction < 270)) {
                    next_x = 0;
                    var exept = [90, 270];
                    this.direction = getRandomInt(0, 360, exept);
                }
                if (next_y >= (canvas.height - this.height) && (this.direction > 0 && this.direction < 180)) {
                    next_y = canvas.height - this.height;
                    this.direction = getRandomInt(180, 360, this.direction);
                }
                if (next_y <= 0 && (this.direction > 180 && this.direction < 360)) {
                    next_y = 0;
                    this.direction = getRandomInt(0, 180, this.direction);
                }
            
                var nextMatrixPosition = [];
                nextMatrixPosition = this.readMatrix(next_x, next_y, this.width, this.height, this.id);
                if (nextMatrixPosition.length == 0 || typeof nextMatrixPosition[0] == 'undefined') {
                    this.eraseMatrix(this.x, this.y, this.width, this.height);
            
                    this.x = next_x;
                    this.y = next_y;
            
                    // Enregistre la position dans la matrice
                    this.writeMatrix(this.x, this.y, this.width, this.height, this.id);
            
                    this.update();
                }
            
                // La place dans la matrice est déjà prise -> collision
                else {
                    var e = 0;
                    while (e <= nextMatrixPosition.length) {
                        if (typeof nextMatrixPosition[e] != 'undefined') this.hit(nextMatrixPosition[e]);
                        e++;
                    }
            
                    this.takeOppositeDirection();
            
                    this.update();
                }
            }
            
            
            Person.prototype.eraseMatrix = function(x, y, width, height) {
                var x = Math.floor(x),
                    y = Math.floor(y);
                var i = x;
                while (i < (x + width)) {
                    var u = y;
                    while (u < (y + height)) {
                        if (matrix[i][u] >= 0) {
                            delete matrix[i][u];
                        }
                        u++;
                    }
                    i++;
                }
            }
            
            
            Person.prototype.writeMatrix = function(x, y, width, height, id) {
                var x = Math.floor(x),
                    y = Math.floor(y);
                var i = x;
                while (i < (x + width)) {
                    var u = y;
                    while (u < (y + height)) {
                        matrix[i][u] = id;
                        u++;
                    }
                    i++;
                }
            }
            
            
            Person.prototype.readMatrix = function(x, y, width, height, id) {
                var results = [];
                var x = Math.floor(x),
                    y = Math.floor(y);
                var i = x;
                while (i < (x + width)) {
                    var u = y;
                    while (u < (y + height)) {
                        //console.log(i, u);
                        if (typeof matrix[i][u] != 'undefined' && matrix[i][u] != id) {
                            if (results.length > 0) {
                                for (var o in results) {
                                    if (matrix[i][u] != results[o]) results[results.length] = matrix[i][u];
                                }
                            } else results[results.length] = matrix[i][u];
                        }
                        u++;
                    }
                    i++;
                }
                return results;
            }
            
            
            Person.prototype.takeOppositeDirection = function() {
                if ((this.direction >= 0 && this.direction < 90) || (this.direction > 270 && this.direction <= 360)) {
                    this.direction = getRandomInt(90, 270);
                    return;
                }
            
                if (this.direction > 90 && this.direction < 270) {
                    var exept = [90, 270];
                    this.direction = getRandomInt(0, 360, exept);
                    return;
                }
            
                if (this.direction > 0 && this.direction < 180) {
                    this.direction = getRandomInt(180, 360);
                    return;
                }
            
                if (this.direction > 180) {
                    this.direction = getRandomInt(0, 180);
                }
            }
            
            
            Person.prototype.hit = function(objectId) {
                bursts[bursts.length] = new Burst(bursts.length, this.x, this.y);
                this.color = this.color == "white" ? colors[params.theme][getRandomInt(1, colors[params.theme].length - 1)] : "white";
                persons[objectId].color = persons[objectId].color == "white" ? colors[params.theme][getRandomInt(1, colors[params.theme].length - 1)] : "white";
                persons[objectId].direction = getRandomInt(0, 360, persons[objectId].direction);
            }
            
            
            Person.prototype.update = function() {
                context.beginPath();
            
                context.fillStyle = this.color;
                context.arc(this.x + (this.width / 2), this.y + (this.height / 2), this.width / 2, 0, 2 * Math.PI, false);
                context.shadowColor = this.color;
                context.shadowBlur = this.shadowBlur;
                context.shadowOffsetX = 0;
                context.shadowOffsetY = 0;
                context.fill();
            }
            
            
            
            var canvas = document.getElementById("canvas"),
                context = canvas.getContext("2d"),
                canvasWidth = document.documentElement.clientWidth,
                canvasHeight = document.documentElement.clientHeight;
            
            var persons = [],
                bursts = [],
                numberOfPerson = canvasWidth * canvasHeight <= 320000 ? 25 : 60,
                matrix = createMatrixArray(),
                worldIsPaused = false,
                carpentersCount = numberOfPerson / 10,
                soldiersCount = numberOfPerson / 10,
                priestsCount = numberOfPerson / 10
            params = {
                speed: 2,
                theme: 0
            };
            
            var gui;
            gui = new dat.GUI();
            gui.add(params, 'speed').min(0).max(5).step(1).name('Speed');
            gui.close();
            
            canvas.setAttribute("width", canvasWidth);
            canvas.setAttribute("height", canvasHeight);
            
            function createMatrixArray() {
                var matrix = [],
                    i = 0;
                while (i <= canvasWidth) {
                    matrix[i] = [];
                    i++;
                }
                return matrix;
            }
            
            function start() {
                instantiatePopulation();
                animate();
            
            }
            
            start();
            
            function instantiatePopulation() {
                var i = 0;
                while (i < numberOfPerson) {
                    persons[i] = new Person(i);
                    persons[i].checkIfPositionIsEmpty();
                    i++;
                }
            }
            
            function animate() {
                if (worldIsPaused) return;
            
                //context.clearRect(0, 0, canvas.width, canvas.height);
            
                context.fillStyle = colors[params.theme]["background"];
                context.fillRect(0, 0, canvas.width, canvas.height);
            
                persons_order = persons.slice(0);
            
                persons_order.sort(function(a, b) {
                    return a.y - b.y;
                });
            
                for (var i in persons_order) {
                    var u = persons_order[i].id;
                    persons[u].nextAction();
                }
            
                for (var i in bursts) {
                    bursts[i].draw();
                }
            
                requestAnimationFrame(animate);
            }
            
            function kill(objectId) {
                persons[objectId].eraseMatrix(
                    persons[objectId].x,
                    persons[objectId].y,
                    persons[objectId].width,
                    persons[objectId].height
                );
                persons[objectId] = null;
                delete persons[objectId];
            }
            
            canvas.onclick = function(e) {
                var i = persons.length;
                persons[i] = new Person(i);
                persons[i].x = e.layerX;
                persons[i].y = e.layerY;
            };
            
            
            
            function Burst(id, x, y) {
                this.id = id;
                this.x = x;
                this.y = y;
                this.r = 1;
                this.a = getRandomInt(2, 7) / 10;
            }
            
            Burst.prototype.draw = function() {
            
                context.beginPath();
                context.arc(this.x, this.y, this.r, 0, 2 * Math.PI, false);
                context.closePath();
                context.strokeStyle = "rgba(100,100,100," + this.a + ")";
                context.shadowBlur = 0;
                context.stroke();
                this.a -= .01;
                if (this.a <= 0) this.die();
                else this.r++;
            }
            
            Burst.prototype.die = function() {
                bursts[this.id] = null;
                delete(bursts[this.id]);
            }
        </script>
	</body>
</html>
```

#### [HTML5 Canvas可在线定制的9类粒子爆发动画](https://wow.techbrood.com/fiddle/34161)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            body {
                background-color: #000;
                margin: 0;
                min-height: 100vh;
                overflow: hidden;
                font-family: Helvetica, Sans-serif;
                font-size: 85%;
            }
            .info {
                position: absolute;
                bottom: 0;
                left: 0;
                z-index: 100;
                background-color: rgba(200, 200, 200, .8);
                font-size: 80%;
            }
            form {
                position: absolute;
                top: 0;
                left: 0;
                z-index: 100;
                background-color: rgba(200, 200, 200, .8);
                padding: 8px;
                font-size: 80%;
            }
            form input[type=text] {
                width: 30px;
                border: 1px solid #000;
                text-align: center;
            }
            form button {
                margin: 4px auto;
                border: 1px solid #000;
                display: block;
            }
        </style>
	</head>
	<body>
        <form id="settings" onsubmit="return false">
            Particles
            <br/>
            <input type="text" id="inBubblesMin" value="150" /> To
            <input type="text" id="inBubblesMax" value="150" />
            <br/> Size
        
            <br/>
            <input type="text" id="inSizeMin" value="2" /> To
            <input type="text" id="inSizeMax" value="2" />
            <br/> Speed
        
            <br/>
            <input type="text" id="inSpeedMin" value="1" /> To
            <input type="text" id="inSpeedMax" value="1" />
            <br/> Curve
        
            <br/>
            <input type="text" id="inCurveMin" value=".5" /> To
            <input type="text" id="inCurveMax" value=".5" />
            <br/>
            <input type="checkbox" name="waveX" checked="checked" />Wave X
            <br/>
            <input type="checkbox" name="waveY" />Wave Y
            <br/>
            <button id="btnClear">Clear</button>
            <button id="btnSet">Burst!</button>
            <br/> Presets:
            <button id="preset1">Time Dilation</button>
            <button id="preset2">Warp Speed</button>
            <button id="preset3">Kaleider</button>
            <button id="preset4">Star Dust</button>
            <button id="preset5">Kaleidonut</button>
            <button id="preset6">Loading...</button>
            <button id="preset7">Flower Out</button>
            <button id="preset8">Flower Out 2</button>
            <button id="preset0">Default</button>
        </form>
        
        <script>
            // Create Canvas.
            var canvas = document.createElement('canvas');
            var ctx = canvas.getContext('2d');
            // Store all particles.
            var particles = [];
            // A lighter colorset (I can't decide).
            var colors = [
                'rgba(243,169,192,.95)',
                'rgba(255,229,154,.95)',
                'rgba(97,181,97,.95)',
                'rgba(58,144,176,.95)',
                'rgba(255,255,255,.95)'
            ];
            // The default colorset.
            var colors = [
                'rgba(69,204,255,.95)',
                'rgba(73,232,62,.95)',
                'rgba(255,212,50,.95)',
                'rgba(232,75,48,.95)',
                'rgba(178,67,255,.95)'
            ];
            
            
            
            
            
            // Configure settings options
            var config = {};
            var settings = document.getElementById('settings');
            initSettings();
            
            // Init.
            resize();
            // Fire the Burst! button.
            settings.btnSet.onclick();
            // Attach canvas.
            document.body.appendChild(canvas);
            // Begin drawing.
            window.requestAnimationFrame(draw);
            // Sync canvas to window.
            window.onresize = resize;
            
            
            function burst() {
                var n = rand(config.num[0], config.num[1]);
                var origin = [
                    canvas.width / 2,
                    canvas.height / 2
                ]
                for (var i = 0; i < n; i++) {
                    var angle = (360 / n) * (i + 1);
                    particles.push(new particle({
                        angle: angle,
                        pos: [origin[0], origin[1]],
                        size: rand(config.size[0], config.size[1]),
                        speed: rand(config.speed[0], config.speed[1]),
                        color: colors[i % 5],
                        //color:randColor(92,255),
                        waveX: config.waveX,
                        waveY: config.waveY,
                        curve: rand(config.curve[0], config.curve[1]),
                        index: i
                    }));
                }
            }
            
            // Drawing.
            function draw() {
                for (var i = 0; i < particles.length; i++) {
                    var p = particles[i];
                    p.move();
                    p.draw(ctx);
                }
                fade();
                window.requestAnimationFrame(draw);
            }
            
            
            function particle(options) {
                this.angle = 0;
                this.curve = 0;
                this.pos = [0, 0];
                this.size = 100;
                this.speed = 1;
                this.color = 'rgba(255,64,64,.95)';
                this.waveX = false;
                this.waveY = false;
                this.index = 0;
            
                // Override defaults.
                for (var i in options) {
                    this[i] = options[i];
                }
            
                this.move = function() {
                    this.angle += this.curve;
                    var radians = this.angle * Math.PI / 180;
                    this.pos[0] += Math.cos(radians) * this.speed,
                        this.pos[1] += Math.sin(radians) * this.speed;
            
                    if (this.waveX) {
                        this.pos[0] += Math.cos(this.index);
                    }
                    if (this.waveY) {
                        this.pos[1] += Math.sin(this.index);
                    }
                }
                this.draw = function(ctx) {
                    ctx.strokeStyle = this.color;
                    ctx.beginPath();
                    ctx.arc(this.pos[0], this.pos[1], this.size, 0, 2 * Math.PI);
                    ctx.stroke();
                }
            }
            
            function preset(options) {
                for (var i in options) {
                    if (settings[i].type == 'checkbox') {
                        settings[i].checked = options[i];
                        continue;
                    }
                    settings[i].value = options[i];
                }
                clear();
                settings.btnSet.onclick();
            }
            
            function fade() {
                ctx.beginPath();
                ctx.fillStyle = 'rgba(0, 0, 0, .03)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                ctx.fill();
            }
            
            function clear() {
                ctx.beginPath();
                ctx.fillStyle = 'rgba(0, 0, 0, 1)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                ctx.fill();
                particles.length = 0;
            }
            
            function resize() {
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;
            }
            
            function initSettings() {
                settings.btnSet.onclick = function() {
                    config.num = [
                        parseFloat(settings.inBubblesMin.value),
                        parseFloat(settings.inBubblesMax.value)
                    ];
                    config.size = [
                        parseFloat(settings.inSizeMin.value),
                        parseFloat(settings.inSizeMax.value)
                    ];
                    config.speed = [
                        parseFloat(settings.inSpeedMin.value),
                        parseFloat(settings.inSpeedMax.value)
                    ];
                    config.curve = [
                        parseFloat(settings.inCurveMin.value),
                        parseFloat(settings.inCurveMax.value)
                    ];
            
                    config.waveX = settings.waveX.checked;
                    config.waveY = settings.waveY.checked;
                    burst();
                }
            
                // Configure clear button
                settings.btnClear.onclick = clear;
            
                // Presets.
                settings.preset0.onclick = function() {
                    preset({
                        inBubblesMin: 150,
                        inBubblesMax: 150,
                        inSizeMin: 2,
                        inSizeMax: 2,
                        inSpeedMin: 1,
                        inSpeedMax: 1,
                        inCurveMin: .5,
                        inCurveMax: .5,
                        waveX: true,
                        waveY: false
                    });
                }
                settings.preset1.onclick = function() {
                    preset({
                        inBubblesMin: 300,
                        inBubblesMax: 300,
                        inSizeMin: 2,
                        inSizeMax: 2,
                        inSpeedMin: 100,
                        inSpeedMax: 100,
                        inCurveMin: 180,
                        inCurveMax: 180,
                        waveX: true,
                        waveY: false
                    });
                }
                settings.preset2.onclick = function() {
                    preset({
                        inBubblesMin: 500,
                        inBubblesMax: 500,
                        inSizeMin: 2,
                        inSizeMax: 3,
                        inSpeedMin: 1,
                        inSpeedMax: 50,
                        inCurveMin: 0,
                        inCurveMax: 0,
                        waveX: true,
                        waveY: false
                    });
                }
                settings.preset3.onclick = function() {
                    preset({
                        inBubblesMin: 80,
                        inBubblesMax: 80,
                        inSizeMin: 500,
                        inSizeMax: 500,
                        inSpeedMin: 1,
                        inSpeedMax: 10,
                        inCurveMin: -.5,
                        inCurveMax: .5,
                        waveX: false,
                        waveY: false
                    });
                }
                settings.preset4.onclick = function() {
                    preset({
                        inBubblesMin: 100,
                        inBubblesMax: 100,
                        inSizeMin: .5,
                        inSizeMax: .5,
                        inSpeedMin: 120,
                        inSpeedMax: 120,
                        inCurveMin: 5,
                        inCurveMax: 20,
                        waveX: true,
                        waveY: false
                    });
                }
                settings.preset5.onclick = function() {
                    preset({
                        inBubblesMin: 90,
                        inBubblesMax: 90,
                        inSizeMin: 200,
                        inSizeMax: 200,
                        inSpeedMin: 2,
                        inSpeedMax: 2,
                        inCurveMin: .5,
                        inCurveMax: .5,
                        waveX: false,
                        waveY: false
                    });
                }
                settings.preset6.onclick = function() {
                    preset({
                        inBubblesMin: 5,
                        inBubblesMax: 5,
                        inSizeMin: 16,
                        inSizeMax: 16,
                        inSpeedMin: 1.8,
                        inSpeedMax: 1.8,
                        inCurveMin: 1.8,
                        inCurveMax: 1.8,
                        waveX: false,
                        waveY: false
                    });
                }
                settings.preset7.onclick = function() {
                    preset({
                        inBubblesMin: 300,
                        inBubblesMax: 300,
                        inSizeMin: 2,
                        inSizeMax: 2,
                        inSpeedMin: 2,
                        inSpeedMax: 2,
                        inCurveMin: 1,
                        inCurveMax: 1,
                        waveX: true,
                        waveY: true
                    });
                }
                settings.preset8.onclick = function() {
                    preset({
                        inBubblesMin: 300,
                        inBubblesMax: 300,
                        inSizeMin: .5,
                        inSizeMax: .5,
                        inSpeedMin: 6,
                        inSpeedMax: 6,
                        inCurveMin: 1.5,
                        inCurveMax: 1.5,
                        waveX: true,
                        waveY: true
                    });
                }
            }
            
            
            
            /* Common functions */
            
            function rand(min, max) {
                return Math.random() * (max - min) + min;
            }
            
            function randColor(min, max) {
                var r = Math.floor(rand(min, max)),
                    g = Math.floor(rand(min, max)),
                    b = Math.floor(rand(min, max));
                return 'rgba(' + r + ',' + g + ',' + b + ',1)';
            }
        </script>
	</body>
</html>
```

#### [HTML5 Canvas炫彩粒子特效生成器](https://wow.techbrood.com/fiddle/37104)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            body {
                margin: 0;
                height: 100vh;
                background-color: #1d1f20;
            }
            ::-webkit-scrollbar {
                display: none;
            }
            #container {
                display: flex;
                justify-content: center;
                align-items: center;
                width: 100%;
                height: 100%;
            }
            canvas {
                max-width: 100%;
                max-height: 100%;
            }
        </style>
	</head>
	<body>
        <div id="container">
            <canvas id="noise"></canvas>
        </div>
        
        <script src='https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.4/lodash.min.js'></script>
        <script src='https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.3/FileSaver.min.js'></script>
        <script src='https://cdnjs.cloudflare.com/ajax/libs/dat-gui/0.6.5/dat.gui.min.js'></script>
        
        <script>
            // from https://gist.github.com/mjackson/5311256
        
            /**
             * Converts an RGB color value to HSL. Conversion formula
             * adapted from https://en.wikipedia.org/wiki/HSL_color_space.
             * Assumes r, g, and b are contained in the set [0, 255] and
             * returns h, s, and l in the set [0, 1].
             *
             * @param   Number  r       The red color value
             * @param   Number  g       The green color value
             * @param   Number  b       The blue color value
             * @return  Array           The HSL representation
             */
            function rgb2hsl(r, g, b) {
                r /= 255, g /= 255, b /= 255;
        
                var max = Math.max(r, g, b),
                    min = Math.min(r, g, b);
                var h, s, l = (max + min) / 2;
        
                if (max == min) {
                    h = s = 0; // achromatic
                } else {
                    var d = max - min;
                    s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
        
                    switch (max) {
                        case r:
                            h = (g - b) / d + (g < b ? 6 : 0);
                            break;
                        case g:
                            h = (b - r) / d + 2;
                            break;
                        case b:
                            h = (r - g) / d + 4;
                            break;
                    }
        
                    h /= 6;
                }
        
                return [h, s, l];
            }
        
            /**
             * Converts an HSL color value to RGB. Conversion formula
             * adapted from https://en.wikipedia.org/wiki/HSL_color_space.
             * Assumes h, s, and l are contained in the set [0, 1] and
             * returns r, g, and b in the set [0, 255].
             *
             * @param   Number  h       The hue
             * @param   Number  s       The saturation
             * @param   Number  l       The lightness
             * @return  Array           The RGB representation
             */
            function hsl2rgb(h, s, l) {
                var r, g, b;
        
                if (s == 0) {
                    r = g = b = l; // achromatic
                } else {
                    function hue2rgb(p, q, t) {
                        if (t < 0) t += 1;
                        if (t > 1) t -= 1;
                        if (t < 1 / 6) return p + (q - p) * 6 * t;
                        if (t < 1 / 2) return q;
                        if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6;
                        return p;
                    }
        
                    var q = l < 0.5 ? l * (1 + s) : l + s - l * s;
                    var p = 2 * l - q;
        
                    r = hue2rgb(p, q, h + 1 / 3);
                    g = hue2rgb(p, q, h);
                    b = hue2rgb(p, q, h - 1 / 3);
                }
        
                return [r * 255, g * 255, b * 255];
            }
        </script>
        
        <script>
            const tau = 2 * Math.PI
            const { random } = _
            const { abs, max, sin, cos } = Math
            
            // a collection of Vector methods
            class Vector {
              constructor (x = 0, y = 0) {
                this.x = x
                this.y = y
              }
              
              static fromPolar (r, a) {
                let x = r * cos(a),
                    y = r * sin(a)
                return new Vector(x, y)
              }
              
              clone () {
                return new Vector(this.x, this.y)
              }
              
              add (v) {
                this.x += v.x
                this.y += v.y
                return this
              }
              
              mul (x) {
                this.x *= x
                this.y *= x
                return this
              }
              
              dist (v) {
                let dx, dy
                return Math.sqrt(
                  (dx = this.x - v.x) * dx,
                  (dy = this.y - v.y) * dy)
              }
              
              get r () {
                return Math.sqrt(
                  this.x * this.x +
                  this.y * this.y)
              }
              
              set r (r) {
                let n = this.norm()
                this.x = r * n.x
                this.y = r * n.y
                return r
              }
              
              get a () {
                return Math.atan2(this.y, this.x)
              }
              
              set a (a) {
                let r = this.r
                this.x = r * cos(a)
                this.y = r * sin(a)
                return a
              }
              
              norm () {
                let r = this.r
                return new Vector(
                  this.x / r, this.y / r)
              }
            }
            
            // canvas' built-in drawing methods have a lot of
            // overhead. this buffer class is for optimized
            // drawing of pixels with alpha. it works by directly 
            // manipulating pixel data and flushing to the canvas
            // by calling putImageData
            class CanvasBuffer {
              constructor (canvas) {
                this.ctx = canvas.getContext("2d")
                this.width = canvas.width
                this.height = canvas.height
                this.center = new Vector(this.width / 2, this.height / 2)
                this.ncenter = this.center.clone().mul(-1)
                this.clear()
              }
              
              clear () {
                this.ctx.fillStyle = "#000"
                this.ctx.fillRect(0, 0, this.width, this.height)
                this.buf = this.ctx.getImageData(0, 0, this.width, this.height)
              }
              
              // draws floating point coordinate pixel
              // with bilinear interpolation
              drawPixel (x, y, c) {
                let fx = ~~x,
                    fy = ~~y,
                    dx = x - fx,
                    dy = y - fy
                
                this.drawCell(fx, fy, c, (1 - dx) * (1 - dy))
                this.drawCell(fx + 1, fy, c, dx * (1 - dy))
                this.drawCell(fx, fy + 1, c, (1 - dx) * dy)
                this.drawCell(fx + 1, fy + 1, c, dx * dy)
              }
              
              // set color of cell with alpha, for drawPixel
              drawCell (x, y, c, a) {
                if (x < 0 || x >= this.width) return
                if (y < 0 || y >= this.height) return
                
                let data = this.buf.data,
                    i = 4 * (x + y * this.width),
                    r = data[i + 0],
                    g = data[i + 1],
                    b = data[i + 2]
                
                a *= c[3]
                r += c[0] * a
                g += c[1] * a
                b += c[2] * a
                
                // overflow color math so red + red = white
                let ro = max(0, r - 255),
                    go = max(0, g - 255),
                    bo = max(0, b - 255),
                    to = (ro + bo + go) / 6
                
                data[i + 0] = r + to
                data[i + 1] = g + to
                data[i + 2] = b + to
              }
              
              flush () {
                this.ctx.putImageData(this.buf, 0, 0)
              }
            }
            
            // a class for generating composite noise textures
            // and reading values of the texture at a position
            class Noise {
              constructor (settings) {
                this.width = settings.width
                this.height = settings.height
                this.type = settings.type
                this.strength = settings.strength
                
                this.canvas = Noise.compositeNoise(
                  this.width, this.height, settings.startOctave, settings.endOctave)
                let ctx = this.canvas.getContext('2d')
                this.data = ctx.getImageData(0, 0, this.width, this.height).data
              }
              
              // create w by h noise
              static noise (w, h) {
                let cv = document.createElement('canvas'),
                    ctx = cv.getContext('2d')
                
                cv.width = w
                cv.height = h
                
                let img = ctx.getImageData(0, 0, w, h),
                    data = img.data
                
                for (let i = 0, l = data.length; i < l; i += 4) {
                  data[i + 0] = random(0, 255)
                  data[i + 1] = random(0, 255)
                  data[i + 2] = random(0, 255)
                  data[i + 3] = 255
                }
                
                ctx.putImageData(img, 0, 0)
                return cv;
              }
              
              // create composite noise with multiple octaves
              static compositeNoise (w, h, soct, eoct) {
                let cv = document.createElement('canvas'),
                    ctx = cv.getContext('2d')
                
                cv.width = w
                cv.height = h
                
                ctx.fillStyle = '#000'
                ctx.fillRect(0, 0, w, h)
                
                ctx.globalCompositeOperation = 'lighter'
                ctx.globalAlpha = 1 / (eoct - soct)
                
                for (let i = soct; i < eoct; i++) {
                  let noise = Noise.noise(w >> i, h >> i)
                  ctx.drawImage(noise, 0, 0, w, h)
                }
                
                return cv
              }
              
              // returns noise value from -1.0 to 1.0
              getNoise (x, y, ch = 0) {
                // bitwise ~~ to floor
                let i = (~~x + ~~y * this.width) * 4
                return this.data[i + ch] / 127.5 - 1
              }
            }
            
            class Particle {
              constructor (system, x = 0, y = 0) {
                this.system = system
                this.pos = new Vector(x, y)
                this.vel = new Vector()
                this.col = [255, 255, 255, 0.1]
              }
              
              update () {
                let { x, y } = this.pos,
                    noise = this.system.noise,
                    dx = noise.getNoise(x, y, 0),
                    dy = noise.getNoise(x, y, noise.type == 2 ? 0 : 1),
                    d = new Vector(dx, dy).mul(noise.strength)
                
                this.col[3] *= 0.99
                
                this.vel.mul(1 - this.system.friction)
                this.vel.add(d)
                
                if ((noise.type == 0 && this.vel.r > this.system.speed)
                    || noise.type == 1)
                  this.vel.r = this.system.speed
              }
            }
            
            class ParticleSystem {
              constructor (settings, noise, buffer) {
                this.noise = noise
                this.buffer = buffer
                this.particles = []
                
                this.num = settings.numParticles
                this.subFrames = settings.subFrames
                
                this.speed = settings.speed
                this.friction = settings.friction
                
                for (let i = 0; i < this.num; i++)
                  this.particles.push(new Particle(this))
                
                let r = (this.buffer.height / 2) * settings.radius
                
                switch (settings.shape) {
                  case 'circle': this.circle(r); break;
                  case 'triangle': this.polygon(r, 3); break;
                  case 'triangle2': this.polygon(r, 3, tau / 2); break;
                  case 'square': this.polygon(r, 4, tau / 8); break;
                  case 'square2': this.polygon(r, 4); break;
                  case 'hexagon': this.polygon(r, 6); break;
                  case 'hexagon2': this.polygon(r, 6, tau / 12); break;
                  case 'star': this.star(r); break;
                  case 'random': this.random(r); break;
                }
                
                this.setVels(settings.velocity, this.speed)
                
                if (settings.colorType == 'solid')
                  this.colorify(settings.color.concat(settings.alpha))
                
                if (settings.colorType == 'rainbow')
                  this.rainbowify(settings.alpha)
              }
              
              circle (radius, thickness = 0) {
                for (let p of this.particles) {
                  // get random radius and angle
                  let r1 = radius + thickness * random(-0.5, 0.5),
                      a1 = random(0, tau),
                      r2 = random(0, 1, true),
                      a2 = random(0, tau)
                  
                  let pos = Vector.fromPolar(r1, a1),
                      vel = Vector.fromPolar(-3, a1)
                  
                  pos.add(this.buffer.center)
                  
                  p.pos = pos
                  p.vel = vel
                }
              }
              
              polygon (radius, sides, angle = 0) {
                // calculate lines of polygon
                let lines = []
                for (let i = 0; i < sides; i++) {
                  let a = i * tau / sides - tau / 4 + angle,
                      x1 = radius * cos(a),
                      y1 = radius * sin(a),
                      x2 = radius * cos(a + tau / sides),
                      y2 = radius * sin(a + tau / sides)
                  lines.push({ x1, y1, x2, y2 })
                }
                
                let yd = (abs(lines[0].y1) - abs(lines[sides >> 1].y1)) / 2
                if (lines[0].y1 > lines[sides >> 1].y1) yd *= -1
                
                for (let p of this.particles) {
                  // choose random line
                  let { x1, x2, y1, y2 } = lines[random(0, sides - 1)]
                  // lerp between points
                  let t = random(0, 1, true),
                      x = x1 + t * (x2 - x1),
                      y = y1 + t * (y2 - y1)
                  
                  let pos = new Vector(x, y)
                  
                  // center on canvas
                  pos.add(this.buffer.center)
                  pos.y += yd
                  
                  p.pos = pos
                }
              }
              
              star (radius) {
                // calculate lines of star
                let lines = []
                for (let i = 0; i < 5; i++) {
                  let a = i * tau / 5 - tau / 4,
                      x1 = radius * cos(a),
                      y1 = radius * sin(a),
                      x2 = radius * cos(a + tau * 2 / 5),
                      y2 = radius * sin(a + tau * 2 / 5)
                  lines.push({ x1, y1, x2, y2 })
                }
                
                // centering factor
                let yd = (abs(lines[0].y1) - abs(lines[2].y1)) / 2
                
                for (let p of this.particles) {
                  // choose random line 
                  let { x1, x2, y1, y2 } = lines[random(0, 4)]
                  
                  // magic number is tip to valley on star
                  let t = random(0, 0.381966011250105)
                  if (random(0, 1)) t = 1 - t
                  
                  // lerp between points
                  let x = x1 + t * (x2 - x1),
                      y = y1 + t * (y2 - y1)
                  
                  let pos = new Vector(x, y)
                  
                  // center on canvas
                  pos.add(this.buffer.center)
                  pos.y += yd
                  
                  p.pos = pos
                }
              }
              
              fn (fn, domain) {
                for (let p of this.particles) {
                  let x = random(...domain, 1),
                      y = fn(x),
                      t = (x + domain[0]) / (domain[1] - domain[0]),
                      rgb = hsl2rgb(t, 1, 0.5)
                  
                  let pos = new Vector(x, y)
                  
                  pos.add(this.buffer.center)
                  rgb.push(p.col[3])
                  
                  p.col = rgb
                  p.pos = pos
                }
              }
              
              random () {
                for (let p of this.particles) {
                  let x = random(0, this.buffer.width, true),
                      y = random(0, this.buffer.height, true)
                  p.pos = new Vector(x, y)
                }
              }
              
              // 0 = frozen, 1 = outward, 2 = inward
              setVels (dir, speed) {
                for (let p of this.particles) {
                  if (dir == 0) {
                    p.vel.mul(0)
                  } else {
                    let vel = p.pos.clone().add(this.buffer.ncenter)
                    vel.r = (dir == 2 ? -1 : 1) * speed
                    p.vel = vel
                  }
                }
              }
              
              update () {
                let n = this.subFrames
                
                // break up into n steps to make smooth lines
                for (let p of this.particles) {
                  p.update(this.noise)
                  let step = p.vel.clone().mul(1 / n)
                  for (let i = 0; i < n; i++) {
                    this.buffer.drawPixel(p.pos.x, p.pos.y, p.col)
                    p.pos.add(step)
                  }
                }
                
                this.buffer.flush()
              }
              
              rainbowify (alpha) {
                for (let p of this.particles) {
                  let c = p.pos.clone().add(this.buffer.ncenter),
                      a = (c.a + tau / 4) / tau,
                      rgb = hsl2rgb(a, 1, 0.5)
                  
                  rgb.push(alpha)
                  p.col = rgb
                }
              }
              
              colorify (col) {
                for (let p of this.particles)
                  p.col = col.slice()
              }
            }
            
            class ParticleNoise {
              constructor (canvas) {
                this.canvas = canvas
                this.renderID = 0
              }
              
              generate (settings) {
                let {
                  width, height, shape,
                  startOctave, endOctave,
                  numParticles, numFrames
                } = settings
                
                this.frame = 0
                this.numFrames = numFrames
                this.shape = shape
                
                this.canvas.width = width
                this.canvas.height = height
                
                this.buffer = new CanvasBuffer(this.canvas)
                this.noise = new Noise(settings)
                this.particles = new ParticleSystem(settings, this.noise, this.buffer)
                
                this.stop()
                this.render()
              }
              
              render () {
                this.frame++
                this.particles.update()
                
                // update progress bar at 30fps
                if (this.frame == this.numFrames || this.frame % 2 == 0)
                  settings.progress = 100 * this.frame / this.numFrames
                
                if (this.frame < this.numFrames)
                  this.renderID = window.requestAnimationFrame(this.render.bind(this))
                else {
                  this.renderID = 0
                  if (settings.autogenerate)
                    this.generate(settings)
                }
              }
              
              save () {
                let rnd = new Array(5).fill(0).map(() => Math.floor(36 * Math.random()).toString(36)).join('')
                let name = `noise-${this.shape}-${rnd}.png`
                this.canvas.toBlob(blob => {
                  saveAs(blob, name)
                })
              }
              
              stop () {
                if (this.renderID) {
                  window.cancelAnimationFrame(this.renderID)
                  this.renderID = 0
                  settings.progress = 0
                }
              }
              
              invert () {
                this.stop()
                let ctx = this.canvas.getContext('2d')
                let img = ctx.getImageData(0, 0, this.canvas.width, this.canvas.height)
                for (let i = 0; i < img.data.length; i += 4) {
                  img.data[i + 0] = 255 - img.data[i + 0]
                  img.data[i + 1] = 255 - img.data[i + 1]
                  img.data[i + 2] = 255 - img.data[i + 2]
                }
                ctx.putImageData(img, 0, 0)
              }
            }
            
            let PN = new ParticleNoise(document.getElementById('noise'))
            let dpr = 2//window.devicePixelRatio
            
            let settings = {
              width: window.innerWidth * dpr,
              height: window.innerHeight * dpr,
              type: 0,
              strength: 1,
              startOctave: 5,
              endOctave: 8,
              numParticles: 10000,
              numFrames: 300,
              subFrames: 4,
              progress: 0,
              shape: 'triangle',
              radius: 0.7,
              colorType: 'rainbow',
              color: [127, 0, 255],
              alpha: 0.1,
              velocity: 0,
              speed: 3,
              friction: 0.05,
              autogenerate: true,
              save: () => PN.save(),
              stop: () => PN.stop(),
              invert: () => PN.invert(),
              generate: () => PN.generate(settings)
            }
            
            settings.generate()
            
            let gui = new dat.GUI()
            
            let rndr = gui.addFolder('rendering')
            rndr.add(settings, 'width')
            rndr.add(settings, 'height')
            rndr.add(settings, 'numParticles', 10, 3e4, 1)
            rndr.add(settings, 'numFrames', 1, 600, 1)
            rndr.add(settings, 'subFrames', 1, 10, 1)
            rndr.open()
            
            let col = gui.addFolder('particles')
            col.add(settings, 'shape', ['circle', 'triangle', 'triangle2', 'square', 'square2', 'hexagon', 'hexagon2', 'star', 'random'])
            col.add(settings, 'radius', 0.1, 1)
            col.add(settings, 'velocity', {'none': 0, 'outward': 1, 'inward': 2}).name('initial velocity')
            col.add(settings, 'speed', 0.1, 10)
            col.add(settings, 'friction', 0, 1)
            col.add(settings, 'colorType', ['solid', 'rainbow'])
            col.addColor(settings, 'color')
            col.add(settings, 'alpha', 0, 1)
            col.open()
            
            let nz = gui.addFolder('noise')
            nz.add(settings, 'type', {'ink': 0, 'loop': 1, 'streak': 2})
            nz.add(settings, 'strength', 0.1, 10, 0.1)
            nz.add(settings, 'startOctave', 0, 10, 1).onChange(update)
            nz.add(settings, 'endOctave', 0, 11, 1).onChange(update)
            nz.open()
            
            gui.add(settings, 'invert')
            gui.add(settings, 'save')
            gui.add(settings, 'stop')
            gui.add(settings, 'autogenerate')
            gui.add(settings, 'progress', 0, 100, 1).listen()
            gui.add(settings, 'generate')
            
            function update() {
              for (let c of nz.__controllers) {
                switch (c.property) {
                  case "endOctave":
                    let min = settings.startOctave + 1;
                    if (c.getValue() < min)
                      c.setValue(min)
                    break;
                }
              }
            }
        </script>
	</body>
</html>

```

#### [HTML5 Canvas图像粒子化渐显动画特效](https://wow.techbrood.com/fiddle/33013)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            :root {
                --width: 0;
                --height: 0;
            }
            
            body {
                margin: 0;
                overflow: hidden;
                background: #000;
            }
            
            body  canvas {
                position: absolute;
                left: calc(50% - (var(--width))/2);
                top: calc(50% - (var(--height))/2);
                width: var(--width);
                height: var(--height);
                border-radius: 8px;
            }
        </style>
	</head>
	<body>
        <canvas id="c"></canvas>
        
        <script>
            // 注意这边不是用的刮刮卡方式实现
            var c = document.getElementById('c'),
                c_ = document.createElement('canvas'),
                ctx = c.getContext('2d'),
                ctx_ = c_.getContext('2d'),
                dots = [],
                delta = 10,
                dot_size = 1,
                count = 200,
                img,
                src = "https://wow.techbrood.com/uploads/161001/normal1.jpg";
            
            function Dot(x, y, dx, dy, color) {
                this.x = x;
                this.y = y;
                this.dx = dx;
                this.dy = dy;
                this.color = color;
            }
            
            function drawDot(dot) {
                ctx.fillStyle = dot.color;
                ctx.beginPath();
                ctx.arc(dot.x, dot.y, dot_size, 0, 2 * Math.PI);
                ctx.fill();
            }
            
            function getColor(x, y) {
                var pixel = ctx_.getImageData(x, y, 1, 1),
                    data = pixel.data,
                    color = 'rgba(' + data[0] + ',' + data[1] + ',' + data[2] + ',' + (data[3] / 255) +
                    ')';
            
                return color;
            }
            
            function initDot() {
                var x = c.width / 2,
                    y = c.height / 2,
                    dx = -delta / 2 + delta * Math.random(),
                    dy = -delta / 2 + delta * Math.random(),
                    color = getColor(x, y);
                dots.push(new Dot(x, y, dx, dy, color));
            }
            
            img = new Image();
            img.crossOrigin = 'anonymous';
            img.src = src;
            img.onload = function() {
                c.width = c_.width = img.width;
                c.height = c_.height = img.height;
                c.style.setProperty('--width', c.width + 'px');
                c.style.setProperty('--height', c.height + 'px');
                ctx_.drawImage(img, 0, 0);
            
                for (var i = 0; i < count; i++) {
                    initDot();
                }
                draw();
            }
            
            function update() {
                for (var i = 0; i < dots.length; i++) {
                    var dot = dots[i],
                        xDx = dot.x + dot.dx,
                        yDy = dot.y + dot.dy;
            
                    if (xDx - dot_size < 0 || xDx + dot_size > c.width) {
                        dot.dx = -dot.dx;
                    }
            
                    if (yDy - dot_size < 0 || yDy + dot_size > c.height) {
                        dot.dy = -dot.dy;
                    }
            
                    dot.x += dot.dx;
                    dot.y += dot.dy;
                    dot.color = getColor(dot.x, dot.y);
            
                    drawDot(dot);
                }
            }
            
            window.requestAnimFrame = (function() {
                return window.requestAnimationFrame ||
                    window.webkitRequestAnimationFrame ||
                    window.mozRequestAnimationFrame ||
                    window.oRequestAnimationFrame ||
                    window.msRequestAnimationFrame ||
                    function(callback) {
                        window.setTimeout(callback, 1000 / 60);
                    };
            })();
            
            function draw() {
                update();
                window.requestAnimFrame(draw);
            }
        </script>
	</body>
</html>

```

#### [HTML5 Canvas粉尘态粒子引力效应动画特效](https://wow.techbrood.com/fiddle/33452)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }
            html {
                overflow: hidden;
            }
            canvas {
                cursor: none;
            }
        </style>
	</head>
	<body>
        <canvas id=canvas></canvas>
        
        <!--move mouse or click -->
        
        <script>
            window.onload = function() {
            
                var canvas = document.getElementById("canvas");
                var ctx = canvas.getContext("2d");
            
                var pi = Math.PI;
            
                var centerX, centerY;
                var part_num = 2000;
            
                var mousedown = false;
                var X, Y;
                /*===========================================================================*/
            
                /*===========================================================================*/
                var P = [];
                var part = function(x, y, vx, vy, r, red, green, blue, alpha, col) {
                    this.x = x;
                    this.y = y;
                    this.vx = vx;
                    this.vy = vy;
                    this.r = r;
                    this.red = red;
                    this.green = green;
                    this.blue = blue;
                    this.alpha = alpha;
                    this.col = col;
                };
            
                function rand(min, max) {
                    return Math.random() * (max - min) + min;
                }
            
                function dist(dx, dy) {
                    return Math.sqrt(dx * dx + dy * dy);
                }
            
                function size() {
                    canvas.width = window.innerWidth;
                    canvas.height = window.innerHeight;
            
                    centerX = canvas.width / 2;
                    centerY = canvas.height / 2;
                }
                size();
                X = centerX;
                Y = centerY;
            
                function init() {
                    var x, y, vx, vy, r, red, green, blue, alpha, col;
                    for (var i = 0; i < part_num; i++) {
                        x = rand(0, canvas.width);
                        y = rand(0, canvas.height);
                        vx = rand(-1, 1);
                        vy = rand(-1, 1);
                        r = rand(1, 3);
                        red = Math.round(rand(150, 200));
                        green = Math.round(rand(100, 255));
                        blue = Math.round(rand(180, 255));
                        alpha = 1;
                        col = "rgba(" + red + "," + green + "," + blue + "," + alpha + ")";
            
                        P.push(new part(x, y, vx, vy, r, red, green, blue, alpha, col));
                    }
                }
            
                function bg() {
                    ctx.fillStyle = "rgba(25,25,30,1)";
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    //ctx.clearRect(0, 0, canvas.width, canvas.height);
                }
            
                function bounce(b) {
            
                    if (b.x < b.r) {
                        b.x = b.r;
                        b.vx *= -1;
                    }
                    if (b.x > canvas.width - b.r) {
                        b.x = canvas.width - b.r;
                        b.vx *= -1;
                    }
            
                    if (b.y - b.r < 0) {
                        b.y = b.r;
                        b.vy *= -1;
                    }
                    if (b.y > canvas.height - b.r) {
                        b.y = canvas.height - b.r;
                        b.vy *= -1;
                    }
                }
            
                function attract(p) {
            
                    var dx = (p.x - X),
                        dy = (p.y - Y),
                        dist = Math.sqrt(dx * dx + dy * dy),
                        angle = Math.atan2(dy, dx);
            
                    if (dist > 10 && dist < 300) {
                        if (!mousedown) {
                            p.vx -= (20 / (p.r * dist)) * Math.cos(angle);
                            p.vy -= (20 / (p.r * dist)) * Math.sin(angle);
                        } else if (mousedown) {
                            p.vx += (250 / (p.r * dist)) * Math.cos(angle);
                            p.vy += (250 / (p.r * dist)) * Math.sin(angle);
                        }
                    }
            
                }
            
                function draw() {
                    var p;
                    for (var i = 0; i < P.length; i++) {
                        p = P[i];
            
                        if (mouseover) attract(p);
                        bounce(p);
            
                        p.x += p.vx;
                        p.y += p.vy;
            
                        p.vx *= .975;
                        p.vy *= .975;
            
                        ctx.fillStyle = p.col;
                        ctx.fillRect(p.x, p.y, p.r, p.r);
                        //ctx.beginPath();
                        //ctx.fillStyle = p.col;
                        //ctx.arc(p.x, p.y, p.r, 0, 2 * pi);
                        //ctx.fill();
            
            
                    }
                    ctx.strokeStyle = (!mousedown) ? "rgba(255,255,255,1)" : "rgba(255,0,0,1)";
            
                    ctx.beginPath();
                    ctx.moveTo(X, Y - 10);
                    ctx.lineTo(X, Y + 10);
                    ctx.moveTo(X - 10, Y);
                    ctx.lineTo(X + 10, Y);
                    ctx.stroke();
            
                }
            
                function loop() {
                    bg();
                    draw();
            
                    window.requestAnimationFrame(loop);
                }
            
                window.onresize = size;
            
                window.onmousemove = function(e) {
                    X = e.clientX;
                    Y = e.clientY;
                }
            
                window.onmousedown = function() {
                    mousedown = true;
                }
            
                window.onmouseup = function() {
                    mousedown = false;
                }
            
                var mouseover = false;
            
                window.onmouseover = function() {
                    mouseover = true;
                }
            
                window.onmouseout = function() {
                    mouseover = false;
                }
            
                init();
                loop();
            }
        </script>
	</body>
</html>

```

#### [nanogl逼真的3D冰凌雪花模型和漫天飞雪特效](https://wow.techbrood.com/fiddle/43270)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <style>
            *,
            *:after,
            *:before {
                box-sizing: border-box;
                margin: 0;
                padding: 0;
            }
            html,
            body {
                margin: 0;
                padding: 0;
                width: 100%;
                height: 100%;
                overflow: hidden;
            }
            body {
                overflow: hidden;
                background: radial-gradient(circle at center, #ff1a1a, #420000);
            }
            canvas {
                display: block;
                width: 100%;
                height: 100%;
            }
        </style>
	</head>
	<body>
        <script src="https://wow.techbrood.com/uploads/1907/nanogl.js"></script>
        <script src="https://unpkg.com/gl-matrix@2.8.1/dist/gl-matrix.js"></script>
        
        <script type="shader/vertex" id="snowstorm-vert">
        uniform mat4 u_projection;
        uniform mat4 u_view;
        uniform mat4 u_model;
        uniform float u_time;
        attribute vec3 position;
        attribute float offset;
        attribute float size;
        varying float v_alpha;
        
        void main() {
          // snowfall logic adapted from:
          // https://oos.moxiecode.com/js_webgl/snowfall/
          float time = offset + u_time * 0.00005;
          float modTime = mod(time, 1.0);
          time = modTime * modTime;
          vec3 pos = position;
          pos.x += cos(time * 5.0 + position.z) * 5.0;
          pos.y *= time;
          pos.z += sin(time * 5.0 + position.x) * 5.0;
          gl_Position = u_projection * u_view * u_model * vec4(pos, 1.0);
          gl_PointSize = min(150.0, size * (150.0 / length(gl_Position.xyz)));
          v_alpha = gl_PointSize / 50.0;
        }
        </script>
        
        <script type="shader/vertex" id="snowstorm-frag">
        varying float v_alpha;
        
        void main() {
          // render a gl.POINT as a circle
          // https://www.desultoryquest.com/blog/drawing-anti-aliased-circular-points-using-opengl-slash-webgl/
          vec2 xy = 2.0 * gl_PointCoord - 1.0;
          float radius = dot(xy, xy);
          if(radius > 1.0) discard;
          // render the point with with a slight blur / soft edge
          // change the gl_FragColor to use v_alpha to see the difference
          float alpha = v_alpha * mix(1.0, 0.0, length(radius)) * 0.5;
          gl_FragColor = vec4(1, 1, 1, alpha);
        }
        </script>
        
        <script type="shader/vertex" id="snowflake-vert">
        uniform mat4 u_model;
        uniform mat4 u_view;
        uniform mat4 u_projection;
        uniform vec2 u_resolution;
        uniform vec2 u_mouse;
        uniform float u_time;
        attribute float index;
        attribute vec3 position;
        attribute vec4 color;
        varying vec4 v_color;
        
        void main() {
        
          v_color = color;
        
          vec3 pos = position;
          pos.z += sin(index + u_time * 0.001) * 0.1;
        
          // mat4 explode;
          // float intensity = 0.5 * sin(u_time * 0.015) + 0.5 * sin(u_time * 0.005);
          // float rescale = 1.0 + 0.5 * intensity;
          // float angle = u_time * 0.002;
          // explode[0] = vec4(rescale * sin(angle), cos(angle), 0.0, 0.0);
          // explode[1] = vec4(-cos(angle), rescale * sin(angle), 0.0, 0.0);
          // explode[2] = vec4(0.0, 0.0, rescale, 0.0);
          // explode[3] = vec4(0.0, 0.0, 0.0, rescale);
          // mat4 matrix = u_projection * u_view * u_model * explode;
          // gl_Position = matrix * vec4(pos, 1.0);
          gl_Position = u_projection * u_view * u_model * vec4(pos, 1.0);
        }
        </script>
        
        <script type="shader/vertex" id="snowflake-frag">
        varying vec4 v_color;
        
        void main() {
          gl_FragColor = v_color;
        }
        </script>
        
        <script>
        window.complex = {"positions":[[0.06211642175912857,-0.08497891575098038,0],[-0.032064758241176605,-0.08468912541866302,0],[-0.014363478869199753,-0.05202816054224968,0],[0.06211642175912857,-0.08497891575098038,0],[0.09370777755975723,-0.03757401555776596,0],[0.135721817612648,-0.05977260321378708,0],[0.06211642175912857,-0.08497891575098038,0],[-0.014363478869199753,-0.05202816054224968,0],[0.07669702172279358,-0.031432997435331345,0],[0.06211642175912857,-0.08497891575098038,0],[0.05650078505277634,-0.22343795001506805,0],[-0.032064758241176605,-0.08468912541866302,0],[0.06211642175912857,-0.08497891575098038,0],[0.07669702172279358,-0.031432997435331345,0],[0.09370777755975723,-0.03757401555776596,0],[0.06211642175912857,-0.08497891575098038,0],[0.135721817612648,-0.05977260321378708,0],[0.22990381717681885,-0.18727415800094604,0],[0.06211642175912857,-0.08497891575098038,0],[0.06211642175912857,-0.2965243458747864,0],[0.05650078505277634,-0.22343795001506805,0],[0.22990381717681885,-0.18727415800094604,0],[0.3781304955482483,-0.1609034389257431,0],[0.33321332931518555,-0.2474776655435562,0],[0.22990381717681885,-0.18727415800094604,0],[0.33321332931518555,-0.2474776655435562,0],[0.324548602104187,-0.3197607398033142,0],[0.22990381717681885,-0.18727415800094604,0],[0.324548602104187,-0.3197607398033142,0],[0.26264989376068115,-0.4466346502304077,0],[0.22990381717681885,-0.18727415800094604,0],[0.135721817612648,-0.05977260321378708,0],[0.27264758944511414,-0.09990300983190536,0],[0.22990381717681885,-0.18727415800094604,0],[0.27264758944511414,-0.09990300983190536,0],[0.3781304955482483,-0.1609034389257431,0],[0.26264989376068115,-0.4466346502304077,0],[0.06211642175912857,-0.4183802902698517,0],[0.06211642175912857,-0.2965243458747864,0],[0.26264989376068115,-0.4466346502304077,0],[0.29307764768600464,-0.5920358896255493,0],[0.06211642175912857,-0.4183802902698517,0],[0.26264989376068115,-0.4466346502304077,0],[0.36653897166252136,-0.5463942289352417,0],[0.29307764768600464,-0.5920358896255493,0],[0.26264989376068115,-0.4466346502304077,0],[0.324548602104187,-0.3197607398033142,0],[0.36653897166252136,-0.5463942289352417,0],[-0.19449107348918915,-0.17379900813102722,0],[-0.18010908365249634,-0.11171084642410278,0],[-0.032064758241176605,-0.08468912541866302,0],[-0.19449107348918915,-0.17379900813102722,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.18010908365249634,-0.11171084642410278,0],[-0.19449107348918915,-0.17379900813102722,0],[-0.24288570880889893,-0.19698207080364227,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.032064758241176605,-0.08468912541866302,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.014363478869199753,-0.05202816054224968,0],[-0.032064758241176605,-0.08468912541866302,0],[-0.18523892760276794,-0.07798518240451813,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.032064758241176605,-0.08468912541866302,0],[-0.18010908365249634,-0.11171084642410278,0],[-0.18523892760276794,-0.07798518240451813,0],[-0.032064758241176605,-0.08468912541866302,0],[0.05650078505277634,-0.22343795001506805,0],[-0.032064758241176605,-0.28442567586898804,0],[-0.032064758241176605,-0.28442567586898804,0],[-0.032499440014362335,-0.40671631693840027,0],[-0.2617219388484955,-0.44648975133895874,0],[-0.032064758241176605,-0.28442567586898804,0],[0.05650078505277634,-0.22343795001506805,0],[0.06211642175912857,-0.2965243458747864,0],[-0.032064758241176605,-0.28442567586898804,0],[-0.00177862960845232,-0.37985581159591675,0],[-0.032499440014362335,-0.40671631693840027,0],[-0.032064758241176605,-0.28442567586898804,0],[0.06211642175912857,-0.2965243458747864,0],[-0.00177862960845232,-0.37985581159591675,0],[-0.2617219388484955,-0.44648975133895874,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.2901249825954437,-0.26447218656539917,0],[-0.2617219388484955,-0.44648975133895874,0],[-0.29403331875801086,-0.5921807885169983,0],[-0.37726524472236633,-0.5336626768112183,0],[-0.2617219388484955,-0.44648975133895874,0],[-0.032499440014362335,-0.40671631693840027,0],[-0.29403331875801086,-0.5921807885169983,0],[-0.2617219388484955,-0.44648975133895874,0],[-0.37726524472236633,-0.5336626768112183,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.2617219388484955,-0.44648975133895874,0],[-0.2901249825954437,-0.26447218656539917,0],[-0.24288570880889893,-0.19698207080364227,0],[-0.2937435507774353,0.11338113248348236,0],[-0.23484407365322113,0.19676770269870758,0],[-0.18295370042324066,0.055295493453741074,0],[-0.2937435507774353,0.11338113248348236,0],[-0.39937135577201843,0.1743815541267395,0],[-0.3286181092262268,0.23733896017074585,0],[-0.2937435507774353,0.11338113248348236,0],[-0.5240001082420349,0.038920897990465164,0],[-0.39937135577201843,0.1743815541267395,0],[-0.2937435507774353,0.11338113248348236,0],[-0.3286181092262268,0.23733896017074585,0],[-0.23484407365322113,0.19676770269870758,0],[-0.2937435507774353,0.11338113248348236,0],[-0.18295370042324066,0.055295493453741074,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.2937435507774353,0.11338113248348236,0],[-0.5324565768241882,0.008043109439313412,0],[-0.5240001082420349,0.038920897990465164,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.10748667269945145,0.04725825786590576,0],[-0.05208911374211311,0.0162054356187582,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.18523892760276794,-0.07798518240451813,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.05208911374211311,0.0162054356187582,0],[-0.014363478869199753,-0.05202816054224968,0],[-0.09791913628578186,0.00036372101749293506,0],[-0.18295370042324066,0.055295493453741074,0],[-0.10748667269945145,0.04725825786590576,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.4773484468460083,-0.046849433332681656,0],[-0.5324565768241882,0.008043109439313412,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.18523892760276794,-0.07798518240451813,0],[-0.18010908365249634,-0.11171084642410278,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.334538996219635,-0.12396079301834106,0],[-0.3776938021183014,-0.10782800614833832,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.18010908365249634,-0.11171084642410278,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.334538996219635,-0.12396079301834106,0],[-0.2783123254776001,-0.10396004468202591,0],[-0.3776938021183014,-0.10782800614833832,0],[-0.4773484468460083,-0.046849433332681656,0],[-0.032064758241176605,0.30072924494743347,0],[0.06211642175912857,0.31275543570518494,0],[0.050713200122117996,0.09692870080471039,0],[-0.032064758241176605,0.30072924494743347,0],[-0.08419256657361984,0.35216090083122253,0],[-0.01154017448425293,0.3469943702220917,0],[-0.032064758241176605,0.30072924494743347,0],[-0.01154017448425293,0.3469943702220917,0],[0.06211642175912857,0.31275543570518494,0],[-0.032064758241176605,0.30072924494743347,0],[0.050713200122117996,0.09692870080471039,0],[-0.032064758241176605,0.08556146174669266,0],[-0.032064758241176605,0.30072924494743347,0],[-0.2617219388484955,0.46301063895225525,0],[-0.08419256657361984,0.35216090083122253,0],[-0.032064758241176605,0.08556146174669266,0],[-0.10748667269945145,0.04725825786590576,0],[-0.23484407365322113,0.19676770269870758,0],[-0.032064758241176605,0.08556146174669266,0],[0.02093544788658619,0.0523127056658268,0],[-0.010218738578259945,0.017899414524435997,0],[-0.032064758241176605,0.08556146174669266,0],[-0.05208911374211311,0.0162054356187582,0],[-0.10748667269945145,0.04725825786590576,0],[-0.032064758241176605,0.08556146174669266,0],[-0.010218738578259945,0.017899414524435997,0],[-0.05208911374211311,0.0162054356187582,0],[-0.032064758241176605,0.08556146174669266,0],[0.050713200122117996,0.09692870080471039,0],[0.02093544788658619,0.0523127056658268,0],[-0.23484407365322113,0.19676770269870758,0],[-0.10748667269945145,0.04725825786590576,0],[-0.18295370042324066,0.055295493453741074,0],[-0.23484407365322113,0.19676770269870758,0],[-0.3286181092262268,0.23733896017074585,0],[-0.2617219388484955,0.46301063895225525,0],[0.2025873064994812,0.1706530898809433,0],[0.20874238014221191,0.08320517092943192,0],[0.14933504164218903,0.09614250808954239,0],[0.2025873064994812,0.1706530898809433,0],[0.14933504164218903,0.09614250808954239,0],[0.06211642175912857,0.08599614351987839,0],[0.2025873064994812,0.1706530898809433,0],[0.2585534155368805,0.14723767340183258,0],[0.20874238014221191,0.08320517092943192,0],[0.2025873064994812,0.1706530898809433,0],[0.22512230277061462,0.18698734045028687,0],[0.2585534155368805,0.14723767340183258,0],[0.06211642175912857,0.08599614351987839,0],[0.02093544788658619,0.0523127056658268,0],[0.050713200122117996,0.09692870080471039,0],[0.06211642175912857,0.08599614351987839,0],[0.14933504164218903,0.09614250808954239,0],[0.09962562471628189,0.03915441781282425,0],[0.06211642175912857,0.08599614351987839,0],[0.09962562471628189,0.03915441781282425,0],[0.02093544788658619,0.0523127056658268,0],[0.06211642175912857,0.08599614351987839,0],[0.050713200122117996,0.09692870080471039,0],[0.06211642175912857,0.31275543570518494,0],[0.06211642175912857,0.31275543570518494,0],[0.06019790098071098,0.3891279697418213,0],[0.2611284852027893,0.4627208709716797,0],[0.06211642175912857,0.31275543570518494,0],[-0.01154017448425293,0.3469943702220917,0],[0.06019790098071098,0.3891279697418213,0],[0.2611284852027893,0.4627208709716797,0],[0.33075010776519775,0.24769797921180725,0],[0.27630525827407837,0.3000730574131012,0],[0.2611284852027893,0.4627208709716797,0],[0.3650175929069519,0.5630600452423096,0],[0.33075010776519775,0.24769797921180725,0],[0.2611284852027893,0.4627208709716797,0],[0.2932949960231781,0.6079772114753723,0],[0.3650175929069519,0.5630600452423096,0],[0.2611284852027893,0.4627208709716797,0],[0.06211642175912857,0.4346838593482971,0],[0.2932949960231781,0.6079772114753723,0],[0.2611284852027893,0.4627208709716797,0],[0.27630525827407837,0.3000730574131012,0],[0.22512230277061462,0.18698734045028687,0],[0.2611284852027893,0.4627208709716797,0],[0.06019790098071098,0.3891279697418213,0],[0.06211642175912857,0.4346838593482971,0],[0.0990644246339798,0.0005086151650175452,0],[0.02093544788658619,0.0523127056658268,0],[0.09962562471628189,0.03915441781282425,0],[0.0990644246339798,0.0005086151650175452,0],[0.09370777755975723,-0.03757401555776596,0],[0.07669702172279358,-0.031432997435331345,0],[0.0990644246339798,0.0005086151650175452,0],[0.20874238014221191,0.08320517092943192,0],[0.2877890169620514,0.10946899652481079,0],[0.0990644246339798,0.0005086151650175452,0],[0.07669702172279358,-0.031432997435331345,0],[0.02093544788658619,0.0523127056658268,0],[0.0990644246339798,0.0005086151650175452,0],[0.135721817612648,-0.05977260321378708,0],[0.09370777755975723,-0.03757401555776596,0],[0.0990644246339798,0.0005086151650175452,0],[0.09962562471628189,0.03915441781282425,0],[0.20874238014221191,0.08320517092943192,0],[0.0990644246339798,0.0005086151650175452,0],[0.27264758944511414,-0.09990300983190536,0],[0.135721817612648,-0.05977260321378708,0],[0.2877890169620514,0.10946899652481079,0],[0.3931270241737366,0.17054186761379242,0],[0.3883458971977234,0.07232706993818283,0],[0.2877890169620514,0.10946899652481079,0],[0.3883458971977234,0.07232706993818283,0],[0.5174462199211121,0.008188003674149513,0],[0.2877890169620514,0.10946899652481079,0],[0.20874238014221191,0.08320517092943192,0],[0.2585534155368805,0.14723767340183258,0],[0.2877890169620514,0.10946899652481079,0],[0.2585534155368805,0.14723767340183258,0],[0.3931270241737366,0.17054186761379242,0],[0.5174462199211121,0.008188003674149513,0],[0.3781304955482483,-0.1609034389257431,0],[0.27264758944511414,-0.09990300983190536,0],[0.5174462199211121,0.008188003674149513,0],[0.6593700051307678,-0.03687406703829765,0],[0.3781304955482483,-0.1609034389257431,0],[0.5174462199211121,0.008188003674149513,0],[0.659007728099823,0.05317762866616249,0],[0.6593700051307678,-0.03687406703829765,0],[0.5174462199211121,0.008188003674149513,0],[0.3883458971977234,0.07232706993818283,0],[0.48067793250083923,0.12368301302194595,0],[0.5174462199211121,0.008188003674149513,0],[0.48067793250083923,0.12368301302194595,0],[0.659007728099823,0.05317762866616249,0],[0.6457499265670776,-0.3154330253601074,0],[0.5402669906616211,-0.2545050382614136,0],[0.7893956899642944,-0.22378744184970856,0],[0.6457499265670776,-0.3154330253601074,0],[0.5966308116912842,-0.3996889591217041,0],[0.5402669906616211,-0.2545050382614136,0],[0.6457499265670776,-0.3154330253601074,0],[0.7893956899642944,-0.22378744184970856,0],[0.8469122052192688,-0.22578968107700348,0],[0.6457499265670776,-0.3154330253601074,0],[0.6769722700119019,-0.39747217297554016,0],[0.5966308116912842,-0.3996889591217041,0],[0.6457499265670776,-0.3154330253601074,0],[0.8493261933326721,-0.43279725313186646,0],[0.6769722700119019,-0.39747217297554016,0],[0.8469122052192688,-0.22578968107700348,0],[0.8001180291175842,-0.17035795748233795,0],[0.8601281642913818,-0.2119879573583603,0],[0.8469122052192688,-0.22578968107700348,0],[0.7893956899642944,-0.22378744184970856,0],[0.8001180291175842,-0.17035795748233795,0],[0.8601281642913818,-0.2119879573583603,0],[0.8001180291175842,-0.17035795748233795,0],[0.8668917417526245,-0.19455254077911377,0],[0.8668917417526245,-0.19455254077911377,0],[0.8001180291175842,-0.17035795748233795,0],[0.8665238618850708,-0.1755889654159546,0],[0.8665238618850708,-0.1755889654159546,0],[0.8001180291175842,-0.17035795748233795,0],[0.8585730791091919,-0.15757879614830017,0],[0.8585730791091919,-0.15757879614830017,0],[0.8001180291175842,-0.17035795748233795,0],[0.8447713255882263,-0.14444097876548767,0],[0.8447713255882263,-0.14444097876548767,0],[0.8001180291175842,-0.17035795748233795,0],[0.8273359537124634,-0.1375245451927185,0],[0.8273359537124634,-0.1375245451927185,0],[0.8001180291175842,-0.17035795748233795,0],[0.8083723783493042,-0.1377532035112381,0],[0.8083723783493042,-0.1377532035112381,0],[0.8001180291175842,-0.17035795748233795,0],[0.7989754676818848,-0.14083559811115265,0],[0.7989754676818848,-0.14083559811115265,0],[0.8001180291175842,-0.17035795748233795,0],[0.5402669906616211,-0.2545050382614136,0],[0.5402669906616211,-0.2545050382614136,0],[0.33321332931518555,-0.2474776655435562,0],[0.3781304955482483,-0.1609034389257431,0],[0.5402669906616211,-0.2545050382614136,0],[0.49158260226249695,-0.3390507698059082,0],[0.33321332931518555,-0.2474776655435562,0],[0.5402669906616211,-0.2545050382614136,0],[0.5966308116912842,-0.3996889591217041,0],[0.49158260226249695,-0.3390507698059082,0],[0.5402669906616211,-0.2545050382614136,0],[0.8001180291175842,-0.17035795748233795,0],[0.7893956899642944,-0.22378744184970856,0],[0.6593700051307678,-0.03687406703829765,0],[0.6712898015975952,0.04554465040564537,0],[0.6714731454849243,-0.029285239055752754,0],[0.6593700051307678,-0.03687406703829765,0],[0.659007728099823,0.05317762866616249,0],[0.6712898015975952,0.04554465040564537,0],[0.6714731454849243,-0.029285239055752754,0],[0.6712898015975952,0.04554465040564537,0],[0.6866326928138733,-0.006065955385565758,0],[0.6866326928138733,-0.006065955385565758,0],[0.6712898015975952,0.04554465040564537,0],[0.6887110471725464,0.008043109439313412,0],[0.6887110471725464,0.008043109439313412,0],[0.6712898015975952,0.04554465040564537,0],[0.6866168975830078,0.022237073630094528,0],[0.659007728099823,0.05317762866616249,0],[0.48067793250083923,0.12368301302194595,0],[0.3931270241737366,0.17054186761379242,0],[0.3931270241737366,0.17054186761379242,0],[0.4908581078052521,0.3401404321193695,0],[0.5394700765609741,0.2549426853656769,0],[0.3931270241737366,0.17054186761379242,0],[0.33075010776519775,0.24769797921180725,0],[0.4908581078052521,0.3401404321193695,0],[0.3931270241737366,0.17054186761379242,0],[0.48067793250083923,0.12368301302194595,0],[0.3883458971977234,0.07232706993818283,0],[0.3931270241737366,0.17054186761379242,0],[0.2585534155368805,0.14723767340183258,0],[0.33075010776519775,0.24769797921180725,0],[0.5394700765609741,0.2549426853656769,0],[0.6448081135749817,0.31579822301864624,0],[0.6212678551673889,0.23305761814117432,0],[0.5394700765609741,0.2549426853656769,0],[0.4908581078052521,0.3401404321193695,0],[0.6448081135749817,0.31579822301864624,0],[0.5394700765609741,0.2549426853656769,0],[0.6212678551673889,0.23305761814117432,0],[0.8067785501480103,0.13735799491405487,0],[0.8067785501480103,0.13735799491405487,0],[0.8455125093460083,0.22553017735481262,0],[0.8377492427825928,0.14201554656028748,0],[0.8067785501480103,0.13735799491405487,0],[0.8377492427825928,0.14201554656028748,0],[0.8257421255111694,0.13703028857707977,0],[0.8067785501480103,0.13735799491405487,0],[0.6212678551673889,0.23305761814117432,0],[0.8455125093460083,0.22553017735481262,0],[0.8257421255111694,0.13703028857707977,0],[0.8377492427825928,0.14201554656028748,0],[0.8431774973869324,0.14386804401874542,0],[0.8431774973869324,0.14386804401874542,0],[0.8377492427825928,0.14201554656028748,0],[0.856979250907898,0.15701548755168915,0],[0.856979250907898,0.15701548755168915,0],[0.8377492427825928,0.14201554656028748,0],[0.8649305701255798,0.1751537173986435,0],[0.8649305701255798,0.1751537173986435,0],[0.8377492427825928,0.14201554656028748,0],[0.8653132319450378,0.1941973865032196,0],[0.8653132319450378,0.1941973865032196,0],[0.8377492427825928,0.14201554656028748,0],[0.8586050868034363,0.2116755247116089,0],[0.8586050868034363,0.2116755247116089,0],[0.8377492427825928,0.14201554656028748,0],[0.8455125093460083,0.22553017735481262,0],[0.8455125093460083,0.22553017735481262,0],[0.6212678551673889,0.23305761814117432,0],[0.6448081135749817,0.31579822301864624,0],[0.6448081135749817,0.31579822301864624,0],[0.5960512757301331,0.40114086866378784,0],[0.82105952501297,0.4529188275337219,0],[0.6448081135749817,0.31579822301864624,0],[0.4908581078052521,0.3401404321193695,0],[0.5960512757301331,0.40114086866378784,0],[0.6448081135749817,0.31579822301864624,0],[0.82105952501297,0.4529188275337219,0],[0.856451153755188,0.4391408860683441,0],[0.856451153755188,0.4391408860683441,0],[0.82105952501297,0.4529188275337219,0],[0.833956778049469,0.46223631501197815,0],[0.856451153755188,0.4391408860683441,0],[0.833956778049469,0.46223631501197815,0],[0.8679527044296265,0.45435675978660583,0],[0.8679527044296265,0.45435675978660583,0],[0.833956778049469,0.46223631501197815,0],[0.8727914094924927,0.47251012921333313,0],[0.8727914094924927,0.47251012921333313,0],[0.833956778049469,0.46223631501197815,0],[0.8482766151428223,0.4938187599182129,0],[0.8727914094924927,0.47251012921333313,0],[0.8482766151428223,0.4938187599182129,0],[0.870383083820343,0.49144795536994934,0],[0.870383083820343,0.49144795536994934,0],[0.8482766151428223,0.4938187599182129,0],[0.8662787675857544,0.5005382299423218,0],[0.8662787675857544,0.5005382299423218,0],[0.8482766151428223,0.4938187599182129,0],[0.8604899644851685,0.5086292028427124,0],[0.8604899644851685,0.5086292028427124,0],[0.8482766151428223,0.4938187599182129,0],[0.8453159928321838,0.5201689600944519,0],[0.8453159928321838,0.5201689600944519,0],[0.8482766151428223,0.4938187599182129,0],[0.8271637558937073,0.5250492691993713,0],[0.8271637558937073,0.5250492691993713,0],[0.8482766151428223,0.4938187599182129,0],[0.808213472366333,0.5227063298225403,0],[0.808213472366333,0.5227063298225403,0],[0.82105952501297,0.4529188275337219,0],[0.5960512757301331,0.40114086866378784,0],[0.808213472366333,0.5227063298225403,0],[0.833956778049469,0.46223631501197815,0],[0.82105952501297,0.4529188275337219,0],[0.808213472366333,0.5227063298225403,0],[0.8482766151428223,0.4938187599182129,0],[0.833956778049469,0.46223631501197815,0],[0.5960512757301331,0.40114086866378784,0],[0.4908581078052521,0.3401404321193695,0],[0.513348400592804,0.5125840306282043,0],[0.5960512757301331,0.40114086866378784,0],[0.513348400592804,0.5125840306282043,0],[0.6185794472694397,0.6199789643287659,0],[0.6185794472694397,0.6199789643287659,0],[0.6014026999473572,0.6526554226875305,0],[0.6131357550621033,0.638176441192627,0],[0.6185794472694397,0.6199789643287659,0],[0.5847659111022949,0.6617517471313477,0],[0.6014026999473572,0.6526554226875305,0],[0.6185794472694397,0.6199789643287659,0],[0.5650372505187988,0.6638433933258057,0],[0.5847659111022949,0.6617517471313477,0],[0.6185794472694397,0.6199789643287659,0],[0.5205613970756531,0.6205105781555176,0],[0.5650372505187988,0.6638433933258057,0],[0.6185794472694397,0.6199789643287659,0],[0.513348400592804,0.5125840306282043,0],[0.5205613970756531,0.6205105781555176,0],[0.5650372505187988,0.6638433933258057,0],[0.5205613970756531,0.6205105781555176,0],[0.5466416478157043,0.6584752202033997,0],[0.5466416478157043,0.6584752202033997,0],[0.5205613970756531,0.6205105781555176,0],[0.531896710395813,0.6468347311019897,0],[0.531896710395813,0.6468347311019897,0],[0.5205613970756531,0.6205105781555176,0],[0.5226022601127625,0.6302463412284851,0],[0.5205613970756531,0.6205105781555176,0],[0.513348400592804,0.5125840306282043,0],[0.4908581078052521,0.3401404321193695,0],[0.33075010776519775,0.24769797921180725,0],[0.2585534155368805,0.14723767340183258,0],[0.22512230277061462,0.18698734045028687,0],[0.33075010776519775,0.24769797921180725,0],[0.22512230277061462,0.18698734045028687,0],[0.27630525827407837,0.3000730574131012,0],[0.3650175929069519,0.5630600452423096,0],[0.2932949960231781,0.6079772114753723,0],[0.364542156457901,0.5773388743400574,0],[0.364542156457901,0.5773388743400574,0],[0.2932949960231781,0.6079772114753723,0],[0.3521808683872223,0.6021203398704529,0],[0.3521808683872223,0.6021203398704529,0],[0.2932949960231781,0.6079772114753723,0],[0.3411100506782532,0.6110199689865112,0],[0.3411100506782532,0.6110199689865112,0],[0.30514687299728394,0.6146672368049622,0],[0.328850656747818,0.6162294149398804,0],[0.3411100506782532,0.6110199689865112,0],[0.2932949960231781,0.6079772114753723,0],[0.30514687299728394,0.6146672368049622,0],[0.06211642175912857,0.4346838593482971,0],[-0.03199230879545212,0.595081627368927,0],[0.06211642175912857,0.594936728477478,0],[0.06211642175912857,0.4346838593482971,0],[-0.03199230879545212,0.42287498712539673,0],[-0.03199230879545212,0.595081627368927,0],[0.06211642175912857,0.4346838593482971,0],[0.06019790098071098,0.3891279697418213,0],[-0.016752298921346664,0.4163147807121277,0],[0.06211642175912857,0.4346838593482971,0],[-0.016752298921346664,0.4163147807121277,0],[-0.03199230879545212,0.42287498712539673,0],[0.06211642175912857,0.594936728477478,0],[0.06211642175912857,0.7166478037834167,0],[0.2835146486759186,0.7608405351638794,0],[0.06211642175912857,0.594936728477478,0],[-0.03199230879545212,0.595081627368927,0],[-0.0463532917201519,0.6543837785720825,0],[0.06211642175912857,0.594936728477478,0],[-0.0463532917201519,0.6543837785720825,0],[0.06211642175912857,0.7166478037834167,0],[0.2835146486759186,0.7608405351638794,0],[0.2356627881526947,0.845544695854187,0],[0.2872772812843323,0.7840057015419006,0],[0.2835146486759186,0.7608405351638794,0],[0.06211642175912857,0.7166478037834167,0],[0.2356627881526947,0.845544695854187,0],[0.2835146486759186,0.7608405351638794,0],[0.2872772812843323,0.7840057015419006,0],[0.290790319442749,0.7675151228904724,0],[0.290790319442749,0.7675151228904724,0],[0.2872772812843323,0.7840057015419006,0],[0.29959237575531006,0.7838969230651855,0],[0.29959237575531006,0.7838969230651855,0],[0.2872772812843323,0.7840057015419006,0],[0.30106252431869507,0.8024827241897583,0],[0.30106252431869507,0.8024827241897583,0],[0.2872772812843323,0.7840057015419006,0],[0.29039710760116577,0.8296652436256409,0],[0.29039710760116577,0.8296652436256409,0],[0.2872772812843323,0.7840057015419006,0],[0.26918086409568787,0.8469845652580261,0],[0.26918086409568787,0.8469845652580261,0],[0.2356627881526947,0.845544695854187,0],[0.25265955924987793,0.8499051332473755,0],[0.26918086409568787,0.8469845652580261,0],[0.2872772812843323,0.7840057015419006,0],[0.2356627881526947,0.845544695854187,0],[0.06211642175912857,0.7166478037834167,0],[-0.032064758241176605,0.7167927026748657,0],[0.03525716811418533,0.9367581009864807,0],[0.06211642175912857,0.7166478037834167,0],[-0.0463532917201519,0.6543837785720825,0],[-0.032064758241176605,0.7167927026748657,0],[0.06211642175912857,0.7166478037834167,0],[0.03525716811418533,0.9367581009864807,0],[0.06211642175912857,0.9517385363578796,0],[0.06211642175912857,0.9517385363578796,0],[0.03525716811418533,0.9367581009864807,0],[0.05438000708818436,0.9792648553848267,0],[0.05438000708818436,0.9792648553848267,0],[0.03525716811418533,0.9367581009864807,0],[0.04186449572443962,0.9925784468650818,0],[0.04186449572443962,0.9925784468650818,0],[0.03525716811418533,0.9367581009864807,0],[0.024849342182278633,1,0],[0.024849342182278633,1,0],[0.03525716811418533,0.9367581009864807,0],[0.005202321335673332,1,0],[0.005202321335673332,1,0],[0.03525716811418533,0.9367581009864807,0],[-0.01181283313781023,0.9925784468650818,0],[-0.01181283313781023,0.9925784468650818,0],[0.03525716811418533,0.9367581009864807,0],[-0.02432834543287754,0.9792648553848267,0],[-0.02432834543287754,0.9792648553848267,0],[0.03525716811418533,0.9367581009864807,0],[-0.031155630946159363,0.9616554379463196,0],[-0.031155630946159363,0.9616554379463196,0],[0.03525716811418533,0.9367581009864807,0],[-0.032064758241176605,0.7167927026748657,0],[-0.032064758241176605,0.7167927026748657,0],[-0.0463532917201519,0.6543837785720825,0],[-0.12930582463741302,0.7627535462379456,0],[-0.032064758241176605,0.7167927026748657,0],[-0.12930582463741302,0.7627535462379456,0],[-0.21966230869293213,0.845689594745636,0],[-0.21966230869293213,0.845689594745636,0],[-0.2667207717895508,0.7609854340553284,0],[-0.23938122391700745,0.8500500321388245,0],[-0.21966230869293213,0.845689594745636,0],[-0.12930582463741302,0.7627535462379456,0],[-0.15100060403347015,0.7621524333953857,0],[-0.21966230869293213,0.845689594745636,0],[-0.15100060403347015,0.7621524333953857,0],[-0.2667207717895508,0.7609854340553284,0],[-0.23938122391700745,0.8500500321388245,0],[-0.2859567403793335,0.7840418219566345,0],[-0.25945669412612915,0.8471294641494751,0],[-0.23938122391700745,0.8500500321388245,0],[-0.2667207717895508,0.7609854340553284,0],[-0.2859567403793335,0.7840418219566345,0],[-0.25945669412612915,0.8471294641494751,0],[-0.2859567403793335,0.7840418219566345,0],[-0.2769206762313843,0.8372540473937988,0],[-0.2769206762313843,0.8372540473937988,0],[-0.29115113615989685,0.8026276230812073,0],[-0.2884907126426697,0.8212099671363831,0],[-0.2769206762313843,0.8372540473937988,0],[-0.2859567403793335,0.7840418219566345,0],[-0.29115113615989685,0.8026276230812073,0],[-0.2667207717895508,0.7609854340553284,0],[-0.0463532917201519,0.6543837785720825,0],[-0.03199230879545212,0.595081627368927,0],[-0.2667207717895508,0.7609854340553284,0],[-0.15100060403347015,0.7621524333953857,0],[-0.0463532917201519,0.6543837785720825,0],[-0.03199230879545212,0.42287498712539673,0],[-0.08419256657361984,0.35216090083122253,0],[-0.2617219388484955,0.46301063895225525,0],[-0.03199230879545212,0.42287498712539673,0],[-0.016752298921346664,0.4163147807121277,0],[-0.01154017448425293,0.3469943702220917,0],[-0.03199230879545212,0.42287498712539673,0],[-0.01154017448425293,0.3469943702220917,0],[-0.08419256657361984,0.35216090083122253,0],[-0.03199230879545212,0.42287498712539673,0],[-0.2617219388484955,0.46301063895225525,0],[-0.29359865188598633,0.6083394289016724,0],[-0.29359865188598633,0.6083394289016724,0],[-0.37763723731040955,0.5634222626686096,0],[-0.3069741725921631,0.6150577664375305,0],[-0.29359865188598633,0.6083394289016724,0],[-0.3598012626171112,0.5186842083930969,0],[-0.37763723731040955,0.5634222626686096,0],[-0.29359865188598633,0.6083394289016724,0],[-0.2617219388484955,0.46301063895225525,0],[-0.3598012626171112,0.5186842083930969,0],[-0.3069741725921631,0.6150577664375305,0],[-0.37763723731040955,0.5634222626686096,0],[-0.3375287353992462,0.6165406703948975,0],[-0.3375287353992462,0.6165406703948975,0],[-0.36296671628952026,0.6023806929588318,0],[-0.35155630111694336,0.6112373471260071,0],[-0.3375287353992462,0.6165406703948975,0],[-0.37763723731040955,0.5634222626686096,0],[-0.36296671628952026,0.6023806929588318,0],[-0.36296671628952026,0.6023806929588318,0],[-0.37763723731040955,0.5634222626686096,0],[-0.37682220339775085,0.5776581168174744,0],[-0.37763723731040955,0.5634222626686096,0],[-0.3598012626171112,0.5186842083930969,0],[-0.34547072649002075,0.25769567489624023,0],[-0.34547072649002075,0.25769567489624023,0],[-0.39937135577201843,0.1743815541267395,0],[-0.45581936836242676,0.30750033259391785,0],[-0.34547072649002075,0.25769567489624023,0],[-0.3286181092262268,0.23733896017074585,0],[-0.39937135577201843,0.1743815541267395,0],[-0.34547072649002075,0.25769567489624023,0],[-0.45581936836242676,0.30750033259391785,0],[-0.48964038491249084,0.3402853310108185,0],[-0.34547072649002075,0.25769567489624023,0],[-0.3598012626171112,0.5186842083930969,0],[-0.2617219388484955,0.46301063895225525,0],[-0.34547072649002075,0.25769567489624023,0],[-0.2617219388484955,0.46301063895225525,0],[-0.3286181092262268,0.23733896017074585,0],[-0.48964038491249084,0.3402853310108185,0],[-0.595413088798523,0.40114086866378784,0],[-0.5834694504737854,0.4165639281272888,0],[-0.48964038491249084,0.3402853310108185,0],[-0.5391941666603088,0.25523248314857483,0],[-0.5753597021102905,0.26314303278923035,0],[-0.48964038491249084,0.3402853310108185,0],[-0.5834694504737854,0.4165639281272888,0],[-0.5198507905006409,0.6202932000160217,0],[-0.48964038491249084,0.3402853310108185,0],[-0.5753597021102905,0.26314303278923035,0],[-0.595413088798523,0.40114086866378784,0],[-0.48964038491249084,0.3402853310108185,0],[-0.45581936836242676,0.30750033259391785,0],[-0.5391941666603088,0.25523248314857483,0],[-0.5198507905006409,0.6202932000160217,0],[-0.5313057899475098,0.6465066075325012,0],[-0.5219247341156006,0.6300135850906372,0],[-0.5198507905006409,0.6202932000160217,0],[-0.5461305379867554,0.6580279469490051,0],[-0.5313057899475098,0.6465066075325012,0],[-0.5198507905006409,0.6202932000160217,0],[-0.5645584464073181,0.6633278727531433,0],[-0.5461305379867554,0.6580279469490051,0],[-0.5198507905006409,0.6202932000160217,0],[-0.6180891394615173,0.6197313070297241,0],[-0.5645584464073181,0.6633278727531433,0],[-0.5198507905006409,0.6202932000160217,0],[-0.5834694504737854,0.4165639281272888,0],[-0.5901151895523071,0.5458459854125977,0],[-0.5198507905006409,0.6202932000160217,0],[-0.5901151895523071,0.5458459854125977,0],[-0.6180891394615173,0.6197313070297241,0],[-0.5645584464073181,0.6633278727531433,0],[-0.6180891394615173,0.6197313070297241,0],[-0.5842934846878052,0.6612749099731445,0],[-0.5842934846878052,0.6612749099731445,0],[-0.6180891394615173,0.6197313070297241,0],[-0.6009413003921509,0.6522526741027832,0],[-0.6009413003921509,0.6522526741027832,0],[-0.6180891394615173,0.6197313070297241,0],[-0.612661600112915,0.6378546953201294,0],[-0.6180891394615173,0.6197313070297241,0],[-0.5901151895523071,0.5458459854125977,0],[-0.595413088798523,0.40114086866378784,0],[-0.595413088798523,0.40114086866378784,0],[-0.6446046233177185,0.3160880208015442,0],[-0.7918528914451599,0.49712899327278137,0],[-0.595413088798523,0.40114086866378784,0],[-0.5753597021102905,0.26314303278923035,0],[-0.6446046233177185,0.3160880208015442,0],[-0.595413088798523,0.40114086866378784,0],[-0.5901151895523071,0.5458459854125977,0],[-0.5834694504737854,0.4165639281272888,0],[-0.595413088798523,0.40114086866378784,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8080586791038513,0.5228025317192078,0],[-0.8080586791038513,0.5228025317192078,0],[-0.7918528914451599,0.49712899327278137,0],[-0.826981782913208,0.5251726508140564,0],[-0.826981782913208,0.5251726508140564,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8451340198516846,0.520292341709137,0],[-0.8451340198516846,0.520292341709137,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8603351712226868,0.5087254643440247,0],[-0.8603351712226868,0.5087254643440247,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8661477565765381,0.5006106495857239,0],[-0.8661477565765381,0.5006106495857239,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8702308535575867,0.491568386554718,0],[-0.8702308535575867,0.491568386554718,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8726170659065247,0.472695916891098,0],[-0.8726170659065247,0.472695916891098,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8677495121955872,0.45458415150642395,0],[-0.8677495121955872,0.45458415150642395,0],[-0.7918528914451599,0.49712899327278137,0],[-0.8561715483665466,0.43940648436546326,0],[-0.8561715483665466,0.43940648436546326,0],[-0.7918528914451599,0.49712899327278137,0],[-0.6446046233177185,0.3160880208015442,0],[-0.6446046233177185,0.3160880208015442,0],[-0.5753597021102905,0.26314303278923035,0],[-0.6284266710281372,0.234224334359169,0],[-0.6446046233177185,0.3160880208015442,0],[-0.6284266710281372,0.234224334359169,0],[-0.694442629814148,0.2929976284503937,0],[-0.6446046233177185,0.3160880208015442,0],[-0.694442629814148,0.2929976284503937,0],[-0.8459529876708984,0.2253551483154297,0],[-0.8459529876708984,0.2253551483154297,0],[-0.8571162223815918,0.15666556358337402,0],[-0.8590101599693298,0.21143002808094025,0],[-0.8459529876708984,0.2253551483154297,0],[-0.8430060148239136,0.1435936838388443,0],[-0.8571162223815918,0.15666556358337402,0],[-0.8459529876708984,0.2253551483154297,0],[-0.8064315915107727,0.13722456991672516,0],[-0.8430060148239136,0.1435936838388443,0],[-0.8459529876708984,0.2253551483154297,0],[-0.694442629814148,0.2929976284503937,0],[-0.8064315915107727,0.13722456991672516,0],[-0.8590101599693298,0.21143002808094025,0],[-0.8571162223815918,0.15666556358337402,0],[-0.865754246711731,0.19388481974601746,0],[-0.865754246711731,0.19388481974601746,0],[-0.8571162223815918,0.15666556358337402,0],[-0.8653634190559387,0.17479784786701202,0],[-0.8430060148239136,0.1435936838388443,0],[-0.8064315915107727,0.13722456991672516,0],[-0.8254319429397583,0.13684846460819244,0],[-0.8064315915107727,0.13722456991672516,0],[-0.6284266710281372,0.234224334359169,0],[-0.5391941666603088,0.25523248314857483,0],[-0.8064315915107727,0.13722456991672516,0],[-0.694442629814148,0.2929976284503937,0],[-0.6284266710281372,0.234224334359169,0],[-0.5391941666603088,0.25523248314857483,0],[-0.6284266710281372,0.234224334359169,0],[-0.5753597021102905,0.26314303278923035,0],[-0.5391941666603088,0.25523248314857483,0],[-0.45581936836242676,0.30750033259391785,0],[-0.5115405917167664,0.24096256494522095,0],[-0.5391941666603088,0.25523248314857483,0],[-0.5115405917167664,0.24096256494522095,0],[-0.39937135577201843,0.1743815541267395,0],[-0.39937135577201843,0.1743815541267395,0],[-0.5240001082420349,0.038920897990465164,0],[-0.6743804216384888,0.05317762866616249,0],[-0.39937135577201843,0.1743815541267395,0],[-0.5115405917167664,0.24096256494522095,0],[-0.45581936836242676,0.30750033259391785,0],[-0.6743804216384888,0.05317762866616249,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.6864835619926453,0.04551522061228752,0],[-0.6743804216384888,0.05317762866616249,0],[-0.5240001082420349,0.038920897990465164,0],[-0.5324565768241882,0.008043109439313412,0],[-0.6743804216384888,0.05317762866616249,0],[-0.5324565768241882,0.008043109439313412,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.6864835619926453,0.04551522061228752,0],[-0.7037214636802673,0.008115556091070175,0],[-0.7016431093215942,0.022257449105381966,0],[-0.6864835619926453,0.04551522061228752,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.7037214636802673,0.008115556091070175,0],[-0.7037214636802673,0.008115556091070175,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.7017405033111572,-0.006067087408155203,0],[-0.7017405033111572,-0.006067087408155203,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.6869114637374878,-0.029324857518076897,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.4773484468460083,-0.046849433332681656,0],[-0.38495439291000366,-0.16488802433013916,0],[-0.6747426390647888,-0.03694651648402214,0],[-0.5324565768241882,0.008043109439313412,0],[-0.4773484468460083,-0.046849433332681656,0],[-0.38495439291000366,-0.16488802433013916,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.38495439291000366,-0.16488802433013916,0],[-0.4773484468460083,-0.046849433332681656,0],[-0.3776938021183014,-0.10782800614833832,0],[-0.38495439291000366,-0.16488802433013916,0],[-0.334538996219635,-0.12396079301834106,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.38495439291000366,-0.16488802433013916,0],[-0.3776938021183014,-0.10782800614833832,0],[-0.334538996219635,-0.12396079301834106,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.6457638144493103,-0.3156503438949585,0],[-0.7360799908638,-0.25518661737442017,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.49087199568748474,-0.34006500244140625,0],[-0.5430727005004883,-0.287047803401947,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.49087199568748474,-0.34006500244140625,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.5430727005004883,-0.287047803401947,0],[-0.5976599454879761,-0.3273831903934479,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.7360799908638,-0.25518661737442017,0],[-0.7979026436805725,-0.14076314866542816,0],[-0.5407155752182007,-0.25472238659858704,0],[-0.5976599454879761,-0.3273831903934479,0],[-0.6457638144493103,-0.3156503438949585,0],[-0.7979026436805725,-0.14076314866542816,0],[-0.8464078903198242,-0.22609843313694,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.7979026436805725,-0.14076314866542816,0],[-0.7360799908638,-0.25518661737442017,0],[-0.8464078903198242,-0.22609843313694,0],[-0.7979026436805725,-0.14076314866542816,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8072935938835144,-0.13767269253730774,0],[-0.8072935938835144,-0.13767269253730774,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8262249231338501,-0.13751958310604095,0],[-0.8262249231338501,-0.13751958310604095,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.843634843826294,-0.14458289742469788,0],[-0.843634843826294,-0.14458289742469788,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8574586510658264,-0.15787777304649353,0],[-0.8574586510658264,-0.15787777304649353,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8654643297195435,-0.17600467801094055,0],[-0.8654643297195435,-0.17600467801094055,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8659335374832153,-0.19495297968387604,0],[-0.8659335374832153,-0.19495297968387604,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8593595027923584,-0.2123119831085205,0],[-0.8593595027923584,-0.2123119831085205,0],[-0.8029438853263855,-0.14158715307712555,0],[-0.8464078903198242,-0.22609843313694,0],[-0.8464078903198242,-0.22609843313694,0],[-0.7360799908638,-0.25518661737442017,0],[-0.6457638144493103,-0.3156503438949585,0],[-0.6457638144493103,-0.3156503438949585,0],[-0.6535957455635071,-0.3884078562259674,0],[-0.7494699358940125,-0.4225912094116211,0],[-0.6457638144493103,-0.3156503438949585,0],[-0.7494699358940125,-0.4225912094116211,0],[-0.8574307560920715,-0.43889597058296204,0],[-0.6457638144493103,-0.3156503438949585,0],[-0.5976599454879761,-0.3273831903934479,0],[-0.6535957455635071,-0.3884078562259674,0],[-0.8574307560920715,-0.43889597058296204,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.8689631223678589,-0.4541735053062439,0],[-0.8574307560920715,-0.43889597058296204,0],[-0.7494699358940125,-0.4225912094116211,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.8689631223678589,-0.4541735053062439,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.8738157153129578,-0.47235462069511414,0],[-0.8738157153129578,-0.47235462069511414,0],[-0.8673068881034851,-0.500390350818634,0],[-0.8714110851287842,-0.4912998080253601,0],[-0.8738157153129578,-0.47235462069511414,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.8673068881034851,-0.500390350818634,0],[-0.8673068881034851,-0.500390350818634,0],[-0.8463441133499146,-0.5200134515762329,0],[-0.8615180850028992,-0.5084810853004456,0],[-0.8673068881034851,-0.500390350818634,0],[-0.828191876411438,-0.524865984916687,0],[-0.8463441133499146,-0.5200134515762329,0],[-0.8673068881034851,-0.500390350818634,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.828191876411438,-0.524865984916687,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.6594935059547424,-0.4293631315231323,0],[-0.59657222032547,-0.4009929895401001,0],[-0.8092415928840637,-0.5224613547325134,0],[-0.7494699358940125,-0.4225912094116211,0],[-0.6594935059547424,-0.4293631315231323,0],[-0.59657222032547,-0.4009929895401001,0],[-0.49087199568748474,-0.34006500244140625,0],[-0.5441026091575623,-0.5120972394943237,0],[-0.59657222032547,-0.4009929895401001,0],[-0.6594935059547424,-0.4293631315231323,0],[-0.6535957455635071,-0.3884078562259674,0],[-0.59657222032547,-0.4009929895401001,0],[-0.6535957455635071,-0.3884078562259674,0],[-0.5976599454879761,-0.3273831903934479,0],[-0.59657222032547,-0.4009929895401001,0],[-0.5441026091575623,-0.5120972394943237,0],[-0.5812568664550781,-0.5230157971382141,0],[-0.59657222032547,-0.4009929895401001,0],[-0.5506254434585571,-0.3131994903087616,0],[-0.49087199568748474,-0.34006500244140625,0],[-0.59657222032547,-0.4009929895401001,0],[-0.5812568664550781,-0.5230157971382141,0],[-0.618369460105896,-0.6206196546554565,0],[-0.59657222032547,-0.4009929895401001,0],[-0.5976599454879761,-0.3273831903934479,0],[-0.5506254434585571,-0.3131994903087616,0],[-0.618369460105896,-0.6206196546554565,0],[-0.6010648012161255,-0.6534834504127502,0],[-0.6128787994384766,-0.6388853788375854,0],[-0.618369460105896,-0.6206196546554565,0],[-0.5843267440795898,-0.662675142288208,0],[-0.6010648012161255,-0.6534834504127502,0],[-0.618369460105896,-0.6206196546554565,0],[-0.5645323991775513,-0.6647633910179138,0],[-0.5843267440795898,-0.662675142288208,0],[-0.618369460105896,-0.6206196546554565,0],[-0.5222654938697815,-0.6307206153869629,0],[-0.5645323991775513,-0.6647633910179138,0],[-0.618369460105896,-0.6206196546554565,0],[-0.5812568664550781,-0.5230157971382141,0],[-0.5222654938697815,-0.6307206153869629,0],[-0.5645323991775513,-0.6647633910179138,0],[-0.5222654938697815,-0.6307206153869629,0],[-0.5461814999580383,-0.6592727303504944,0],[-0.5461814999580383,-0.6592727303504944,0],[-0.5222654938697815,-0.6307206153869629,0],[-0.5315152406692505,-0.6474587321281433,0],[-0.5222654938697815,-0.6307206153869629,0],[-0.5441026091575623,-0.5120972394943237,0],[-0.49087199568748474,-0.34006500244140625,0],[-0.5222654938697815,-0.6307206153869629,0],[-0.5812568664550781,-0.5230157971382141,0],[-0.5441026091575623,-0.5120972394943237,0],[-0.49087199568748474,-0.34006500244140625,0],[-0.5506254434585571,-0.3131994903087616,0],[-0.5430727005004883,-0.287047803401947,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.37726524472236633,-0.5336626768112183,0],[-0.38002797961235046,-0.5472636222839355,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.334538996219635,-0.12396079301834106,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.3488757610321045,-0.2581998407840729,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.2901249825954437,-0.26447218656539917,0],[-0.38002797961235046,-0.5472636222839355,0],[-0.34135597944259644,-0.6005348563194275,0],[-0.3795502781867981,-0.5616624355316162,0],[-0.38002797961235046,-0.5472636222839355,0],[-0.37726524472236633,-0.5336626768112183,0],[-0.34135597944259644,-0.6005348563194275,0],[-0.3795502781867981,-0.5616624355316162,0],[-0.35597556829452515,-0.5950786471366882,0],[-0.3671301603317261,-0.5863850116729736,0],[-0.3795502781867981,-0.5616624355316162,0],[-0.34135597944259644,-0.6005348563194275,0],[-0.35597556829452515,-0.5950786471366882,0],[-0.34135597944259644,-0.6005348563194275,0],[-0.37726524472236633,-0.5336626768112183,0],[-0.3079601526260376,-0.5989500284194946,0],[-0.3079601526260376,-0.5989500284194946,0],[-0.37726524472236633,-0.5336626768112183,0],[-0.29403331875801086,-0.5921807885169983,0],[-0.032499440014362335,-0.40671631693840027,0],[0.01041368953883648,-0.4419231712818146,0],[-0.032499440014362335,-0.5950786471366882,0],[-0.032499440014362335,-0.40671631693840027,0],[-0.00177862960845232,-0.37985581159591675,0],[0.01041368953883648,-0.4419231712818146,0],[-0.032499440014362335,-0.5950786471366882,0],[0.06211642175912857,-0.5951511263847351,0],[-0.03458171710371971,-0.6214426755905151,0],[-0.032499440014362335,-0.5950786471366882,0],[-0.03458171710371971,-0.6214426755905151,0],[-0.2689666450023651,-0.7616344690322876,0],[-0.032499440014362335,-0.5950786471366882,0],[0.01041368953883648,-0.4419231712818146,0],[0.06211642175912857,-0.5951511263847351,0],[-0.2689666450023651,-0.7616344690322876,0],[-0.260658860206604,-0.785307765007019,0],[-0.2874385118484497,-0.7845889925956726,0],[-0.2689666450023651,-0.7616344690322876,0],[-0.03458171710371971,-0.6214426755905151,0],[-0.14124298095703125,-0.7904700636863708,0],[-0.2689666450023651,-0.7616344690322876,0],[-0.14124298095703125,-0.7904700636863708,0],[-0.260658860206604,-0.785307765007019,0],[-0.2874385118484497,-0.7845889925956726,0],[-0.260658860206604,-0.785307765007019,0],[-0.29172733426094055,-0.8031747937202454,0],[-0.29172733426094055,-0.8031747937202454,0],[-0.260658860206604,-0.785307765007019,0],[-0.28835147619247437,-0.8218114972114563,0],[-0.28835147619247437,-0.8218114972114563,0],[-0.260658860206604,-0.785307765007019,0],[-0.2765769958496094,-0.8378614783287048,0],[-0.2765769958496094,-0.8378614783287048,0],[-0.260658860206604,-0.785307765007019,0],[-0.2586486041545868,-0.8477148413658142,0],[-0.2586486041545868,-0.8477148413658142,0],[-0.260658860206604,-0.785307765007019,0],[-0.23792195320129395,-0.8506608605384827,0],[-0.23792195320129395,-0.8506608605384827,0],[-0.260658860206604,-0.785307765007019,0],[-0.21771147847175598,-0.8463327288627625,0],[-0.21771147847175598,-0.8463327288627625,0],[-0.260658860206604,-0.785307765007019,0],[-0.14124298095703125,-0.7904700636863708,0],[-0.21771147847175598,-0.8463327288627625,0],[-0.14124298095703125,-0.7904700636863708,0],[-0.032064758241176605,-0.7167173027992249,0],[-0.032064758241176605,-0.7167173027992249,0],[0.06211642175912857,-0.7167173027992249,0],[0.03800278902053833,-0.9397580623626709,0],[-0.032064758241176605,-0.7167173027992249,0],[0.03800278902053833,-0.9397580623626709,0],[-0.032064758241176605,-0.9517355561256409,0],[-0.032064758241176605,-0.7167173027992249,0],[-0.03458171710371971,-0.6214426755905151,0],[0.06211642175912857,-0.7167173027992249,0],[-0.032064758241176605,-0.7167173027992249,0],[-0.14124298095703125,-0.7904700636863708,0],[-0.03458171710371971,-0.6214426755905151,0],[-0.032064758241176605,-0.9517355561256409,0],[0.03800278902053833,-0.9397580623626709,0],[0.04020942375063896,-0.9554398059844971,0],[-0.032064758241176605,-0.9517355561256409,0],[0.04020942375063896,-0.9554398059844971,0],[-0.02432834543287754,-0.979293704032898,0],[-0.02432834543287754,-0.979293704032898,0],[0.04020942375063896,-0.9554398059844971,0],[-0.01181283313781023,-0.9925945401191711,0],[-0.01181283313781023,-0.9925945401191711,0],[0.04020942375063896,-0.9554398059844971,0],[0.005202321335673332,-1,0],[0.005202321335673332,-1,0],[0.04020942375063896,-0.9554398059844971,0],[0.024849342182278633,-1,0],[0.024849342182278633,-1,0],[0.04020942375063896,-0.9554398059844971,0],[0.04186449572443962,-0.9925945401191711,0],[0.04186449572443962,-0.9925945401191711,0],[0.04020942375063896,-0.9554398059844971,0],[0.04747403785586357,-0.9602912068367004,0],[0.04186449572443962,-0.9925945401191711,0],[0.04747403785586357,-0.9602912068367004,0],[0.05438000708818436,-0.979293704032898,0],[0.05438000708818436,-0.979293704032898,0],[0.04747403785586357,-0.9602912068367004,0],[0.06120729446411133,-0.9616732597351074,0],[0.06120729446411133,-0.9616732597351074,0],[0.03800278902053833,-0.9397580623626709,0],[0.06211642175912857,-0.7167173027992249,0],[0.06120729446411133,-0.9616732597351074,0],[0.04747403785586357,-0.9602912068367004,0],[0.03800278902053833,-0.9397580623626709,0],[0.06211642175912857,-0.7167173027992249,0],[0.06211642175912857,-0.5951511263847351,0],[0.2267940789461136,-0.7535961866378784,0],[0.06211642175912857,-0.7167173027992249,0],[-0.03458171710371971,-0.6214426755905151,0],[0.06211642175912857,-0.5951511263847351,0],[0.06211642175912857,-0.7167173027992249,0],[0.2267940789461136,-0.7535961866378784,0],[0.23431827127933502,-0.8457561135292053,0],[0.23431827127933502,-0.8457561135292053,0],[0.2692570090293884,-0.8471671342849731,0],[0.251960813999176,-0.8501003980636597,0],[0.23431827127933502,-0.8457561135292053,0],[0.29119402170181274,-0.8298795819282532,0],[0.2692570090293884,-0.8471671342849731,0],[0.23431827127933502,-0.8457561135292053,0],[0.2853982448577881,-0.7610548734664917,0],[0.29119402170181274,-0.8298795819282532,0],[0.23431827127933502,-0.8457561135292053,0],[0.2267940789461136,-0.7535961866378784,0],[0.2853982448577881,-0.7610548734664917,0],[0.29119402170181274,-0.8298795819282532,0],[0.2853982448577881,-0.7610548734664917,0],[0.3021714389324188,-0.8026270270347595,0],[0.3021714389324188,-0.8026270270347595,0],[0.2853982448577881,-0.7610548734664917,0],[0.3011130392551422,-0.7840285301208496,0],[0.3011130392551422,-0.7840285301208496,0],[0.2853982448577881,-0.7610548734664917,0],[0.292624294757843,-0.7676849365234375,0],[0.2853982448577881,-0.7610548734664917,0],[0.2267940789461136,-0.7535961866378784,0],[0.06211642175912857,-0.5951511263847351,0],[0.06211642175912857,-0.5951511263847351,0],[0.01041368953883648,-0.4419231712818146,0],[0.06211642175912857,-0.4183802902698517,0],[0.06211642175912857,-0.4183802902698517,0],[0.01041368953883648,-0.4419231712818146,0],[-0.00177862960845232,-0.37985581159591675,0],[0.06211642175912857,-0.4183802902698517,0],[-0.00177862960845232,-0.37985581159591675,0],[0.06211642175912857,-0.2965243458747864,0],[0.29307764768600464,-0.5920358896255493,0],[0.3309357762336731,-0.6000843048095703,0],[0.3052804470062256,-0.5987032651901245,0],[0.29307764768600464,-0.5920358896255493,0],[0.3547312319278717,-0.585821270942688,0],[0.3309357762336731,-0.6000843048095703,0],[0.29307764768600464,-0.5920358896255493,0],[0.3662344515323639,-0.5608044266700745,0],[0.3547312319278717,-0.585821270942688,0],[0.29307764768600464,-0.5920358896255493,0],[0.36653897166252136,-0.5463942289352417,0],[0.3662344515323639,-0.5608044266700745,0],[0.3309357762336731,-0.6000843048095703,0],[0.3547312319278717,-0.585821270942688,0],[0.34379059076309204,-0.5949337482452393,0],[0.36653897166252136,-0.5463942289352417,0],[0.324548602104187,-0.3197607398033142,0],[0.33321332931518555,-0.2474776655435562,0],[0.49158260226249695,-0.3390507698059082,0],[0.5966308116912842,-0.3996889591217041,0],[0.5231931209564209,-0.6292901039123535,0],[0.5231931209564209,-0.6292901039123535,0],[0.5470740795135498,-0.6576598882675171,0],[0.5324355363845825,-0.6459268927574158,0],[0.5231931209564209,-0.6292901039123535,0],[0.5653632879257202,-0.6631036400794983,0],[0.5470740795135498,-0.6576598882675171,0],[0.5231931209564209,-0.6292901039123535,0],[0.6189692616462708,-0.6190613508224487,0],[0.5653632879257202,-0.6631036400794983,0],[0.5231931209564209,-0.6292901039123535,0],[0.5966308116912842,-0.3996889591217041,0],[0.6189692616462708,-0.6190613508224487,0],[0.5653632879257202,-0.6631036400794983,0],[0.6189692616462708,-0.6190613508224487,0],[0.5850768089294434,-0.6609971523284912,0],[0.5850768089294434,-0.6609971523284912,0],[0.6134800314903259,-0.637313187122345,0],[0.6017319560050964,-0.6518464684486389,0],[0.5850768089294434,-0.6609971523284912,0],[0.6189692616462708,-0.6190613508224487,0],[0.6134800314903259,-0.637313187122345,0],[0.5966308116912842,-0.3996889591217041,0],[0.6769722700119019,-0.39747217297554016,0],[0.8092764019966125,-0.521350622177124,0],[0.8092764019966125,-0.521350622177124,0],[0.6769722700119019,-0.39747217297554016,0],[0.8493261933326721,-0.43279725313186646,0],[0.8092764019966125,-0.521350622177124,0],[0.8493261933326721,-0.43279725313186646,0],[0.8281995058059692,-0.5237207412719727,0],[0.8281995058059692,-0.5237207412719727,0],[0.8493261933326721,-0.43279725313186646,0],[0.8463517427444458,-0.5188404321670532,0],[0.8463517427444458,-0.5188404321670532,0],[0.8493261933326721,-0.43279725313186646,0],[0.861552894115448,-0.5072735548019409,0],[0.861552894115448,-0.5072735548019409,0],[0.8493261933326721,-0.43279725313186646,0],[0.8673654794692993,-0.4991587698459625,0],[0.8673654794692993,-0.4991587698459625,0],[0.8493261933326721,-0.43279725313186646,0],[0.8714455962181091,-0.4900889992713928,0],[0.8714455962181091,-0.4900889992713928,0],[0.8493261933326721,-0.43279725313186646,0],[0.8738157153129578,-0.4711548686027527,0],[0.8738157153129578,-0.4711548686027527,0],[0.8493261933326721,-0.43279725313186646,0],[0.8689354062080383,-0.4529610276222229,0],[0.8689354062080383,-0.4529610276222229,0],[0.8493261933326721,-0.43279725313186646,0],[0.857368528842926,-0.4376673400402069,0],[0.02093544788658619,0.0523127056658268,0],[0.07669702172279358,-0.031432997435331345,0],[-0.010218738578259945,0.017899414524435997,0],[-0.5430727005004883,-0.287047803401947,0],[-0.5506254434585571,-0.3131994903087616,0],[-0.5976599454879761,-0.3273831903934479,0],[-0.0463532917201519,0.6543837785720825,0],[-0.15100060403347015,0.7621524333953857,0],[-0.12930582463741302,0.7627535462379456,0],[0.03800278902053833,-0.9397580623626709,0],[0.04747403785586357,-0.9602912068367004,0],[0.04020942375063896,-0.9554398059844971,0],[0.20874238014221191,0.08320517092943192,0],[0.09962562471628189,0.03915441781282425,0],[0.14933504164218903,0.09614250808954239,0],[-0.014363478869199753,-0.05202816054224968,0],[-0.05208911374211311,0.0162054356187582,0],[-0.010218738578259945,0.017899414524435997,0],[-0.014363478869199753,-0.05202816054224968,0],[-0.010218738578259945,0.017899414524435997,0],[0.07669702172279358,-0.031432997435331345,0],[0.06019790098071098,0.3891279697418213,0],[-0.01154017448425293,0.3469943702220917,0],[-0.016752298921346664,0.4163147807121277,0],[-0.6594935059547424,-0.4293631315231323,0],[-0.7494699358940125,-0.4225912094116211,0],[-0.6535957455635071,-0.3884078562259674,0],[-0.24841752648353577,-0.18464961647987366,0],[-0.24288570880889893,-0.19698207080364227,0],[-0.2901249825954437,-0.26447218656539917,0]],"cells":[[0,1,2],[3,4,5],[6,7,8],[9,10,11],[12,13,14],[15,16,17],[18,19,20],[21,22,23],[24,25,26],[27,28,29],[30,31,32],[33,34,35],[36,37,38],[39,40,41],[42,43,44],[45,46,47],[48,49,50],[51,52,53],[54,55,56],[57,58,59],[60,61,62],[63,64,65],[66,67,68],[69,70,71],[72,73,74],[75,76,77],[78,79,80],[81,82,83],[84,85,86],[87,88,89],[90,91,92],[93,94,95],[96,97,98],[99,100,101],[102,103,104],[105,106,107],[108,109,110],[111,112,113],[114,115,116],[117,118,119],[120,121,122],[123,124,125],[126,127,128],[129,130,131],[132,133,134],[135,136,137],[138,139,140],[141,142,143],[144,145,146],[147,148,149],[150,151,152],[153,154,155],[156,157,158],[159,160,161],[162,163,164],[165,166,167],[168,169,170],[171,172,173],[174,175,176],[177,178,179],[180,181,182],[183,184,185],[186,187,188],[189,190,191],[192,193,194],[195,196,197],[198,199,200],[201,202,203],[204,205,206],[207,208,209],[210,211,212],[213,214,215],[216,217,218],[219,220,221],[222,223,224],[225,226,227],[228,229,230],[231,232,233],[234,235,236],[237,238,239],[240,241,242],[243,244,245],[246,247,248],[249,250,251],[252,253,254],[255,256,257],[258,259,260],[261,262,263],[264,265,266],[267,268,269],[270,271,272],[273,274,275],[276,277,278],[279,280,281],[282,283,284],[285,286,287],[288,289,290],[291,292,293],[294,295,296],[297,298,299],[300,301,302],[303,304,305],[306,307,308],[309,310,311],[312,313,314],[315,316,317],[318,319,320],[321,322,323],[324,325,326],[327,328,329],[330,331,332],[333,334,335],[336,337,338],[339,340,341],[342,343,344],[345,346,347],[348,349,350],[351,352,353],[354,355,356],[357,358,359],[360,361,362],[363,364,365],[366,367,368],[369,370,371],[372,373,374],[375,376,377],[378,379,380],[381,382,383],[384,385,386],[387,388,389],[390,391,392],[393,394,395],[396,397,398],[399,400,401],[402,403,404],[405,406,407],[408,409,410],[411,412,413],[414,415,416],[417,418,419],[420,421,422],[423,424,425],[426,427,428],[429,430,431],[432,433,434],[435,436,437],[438,439,440],[441,442,443],[444,445,446],[447,448,449],[450,451,452],[453,454,455],[456,457,458],[459,460,461],[462,463,464],[465,466,467],[468,469,470],[471,472,473],[474,475,476],[477,478,479],[480,481,482],[483,484,485],[486,487,488],[489,490,491],[492,493,494],[495,496,497],[498,499,500],[501,502,503],[504,505,506],[507,508,509],[510,511,512],[513,514,515],[516,517,518],[519,520,521],[522,523,524],[525,526,527],[528,529,530],[531,532,533],[534,535,536],[537,538,539],[540,541,542],[543,544,545],[546,547,548],[549,550,551],[552,553,554],[555,556,557],[558,559,560],[561,562,563],[564,565,566],[567,568,569],[570,571,572],[573,574,575],[576,577,578],[579,580,581],[582,583,584],[585,586,587],[588,589,590],[591,592,593],[594,595,596],[597,598,599],[600,601,602],[603,604,605],[606,607,608],[609,610,611],[612,613,614],[615,616,617],[618,619,620],[621,622,623],[624,625,626],[627,628,629],[630,631,632],[633,634,635],[636,637,638],[639,640,641],[642,643,644],[645,646,647],[648,649,650],[651,652,653],[654,655,656],[657,658,659],[660,661,662],[663,664,665],[666,667,668],[669,670,671],[672,673,674],[675,676,677],[678,679,680],[681,682,683],[684,685,686],[687,688,689],[690,691,692],[693,694,695],[696,697,698],[699,700,701],[702,703,704],[705,706,707],[708,709,710],[711,712,713],[714,715,716],[717,718,719],[720,721,722],[723,724,725],[726,727,728],[729,730,731],[732,733,734],[735,736,737],[738,739,740],[741,742,743],[744,745,746],[747,748,749],[750,751,752],[753,754,755],[756,757,758],[759,760,761],[762,763,764],[765,766,767],[768,769,770],[771,772,773],[774,775,776],[777,778,779],[780,781,782],[783,784,785],[786,787,788],[789,790,791],[792,793,794],[795,796,797],[798,799,800],[801,802,803],[804,805,806],[807,808,809],[810,811,812],[813,814,815],[816,817,818],[819,820,821],[822,823,824],[825,826,827],[828,829,830],[831,832,833],[834,835,836],[837,838,839],[840,841,842],[843,844,845],[846,847,848],[849,850,851],[852,853,854],[855,856,857],[858,859,860],[861,862,863],[864,865,866],[867,868,869],[870,871,872],[873,874,875],[876,877,878],[879,880,881],[882,883,884],[885,886,887],[888,889,890],[891,892,893],[894,895,896],[897,898,899],[900,901,902],[903,904,905],[906,907,908],[909,910,911],[912,913,914],[915,916,917],[918,919,920],[921,922,923],[924,925,926],[927,928,929],[930,931,932],[933,934,935],[936,937,938],[939,940,941],[942,943,944],[945,946,947],[948,949,950],[951,952,953],[954,955,956],[957,958,959],[960,961,962],[963,964,965],[966,967,968],[969,970,971],[972,973,974],[975,976,977],[978,979,980],[981,982,983],[984,985,986],[987,988,989],[990,991,992],[993,994,995],[996,997,998],[999,1000,1001],[1002,1003,1004],[1005,1006,1007],[1008,1009,1010],[1011,1012,1013],[1014,1015,1016],[1017,1018,1019],[1020,1021,1022],[1023,1024,1025],[1026,1027,1028],[1029,1030,1031],[1032,1033,1034],[1035,1036,1037],[1038,1039,1040],[1041,1042,1043],[1044,1045,1046],[1047,1048,1049],[1050,1051,1052],[1053,1054,1055],[1056,1057,1058],[1059,1060,1061],[1062,1063,1064],[1065,1066,1067],[1068,1069,1070],[1071,1072,1073],[1074,1075,1076],[1077,1078,1079],[1080,1081,1082],[1083,1084,1085],[1086,1087,1088],[1089,1090,1091],[1092,1093,1094],[1095,1096,1097],[1098,1099,1100],[1101,1102,1103],[1104,1105,1106],[1107,1108,1109],[1110,1111,1112],[1113,1114,1115],[1116,1117,1118],[1119,1120,1121],[1122,1123,1124],[1125,1126,1127],[1128,1129,1130],[1131,1132,1133],[1134,1135,1136],[1137,1138,1139],[1140,1141,1142],[1143,1144,1145],[1146,1147,1148],[1149,1150,1151],[1152,1153,1154],[1155,1156,1157],[1158,1159,1160],[1161,1162,1163],[1164,1165,1166],[1167,1168,1169],[1170,1171,1172],[1173,1174,1175],[1176,1177,1178],[1179,1180,1181],[1182,1183,1184],[1185,1186,1187],[1188,1189,1190],[1191,1192,1193],[1194,1195,1196],[1197,1198,1199],[1200,1201,1202],[1203,1204,1205],[1206,1207,1208],[1209,1210,1211],[1212,1213,1214],[1215,1216,1217],[1218,1219,1220],[1221,1222,1223],[1224,1225,1226],[1227,1228,1229],[1230,1231,1232],[1233,1234,1235],[1236,1237,1238],[1239,1240,1241],[1242,1243,1244],[1245,1246,1247],[1248,1249,1250]]}; 
        </script>
        
        <script>
            const { Program, ArrayBuffer, IndexBuffer, Texture } = window.nanogl;
            
            const canvas = document.createElement('canvas');
            document.body.appendChild(canvas);
            
            const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
            if(!gl) throw 'WebGL not supported';
            
            gl.blendFunc(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA);
            gl.enable(gl.BLEND);
            gl.enable(gl.DEPTH_TEST);
            
            const NUM_PARTICLES = 6000;
            
            const uniforms = {
              u_time: 0,
              u_view: mat4.create(),
              u_mouse: [0, 0],
              u_resolution: [],
              u_projection: mat4.create()
            };
            
            const camera = {};
            camera.eye = [0, 0, 5];
            camera.target = [0, 0, 0];
            camera.up = [0, 1, 0];
            
            const transform = {};
            transform.rotation = [0, 0, 0];
            
            class Snowstorm {
            
              constructor(gl) {
            
                const vert = document.getElementById('snowstorm-vert').textContent;
                const frag = document.getElementById('snowstorm-frag').textContent;
            
                this.gl = gl;
                this.program = new Program(gl, vert, frag, `
                  precision highp float;
                  #define PI 3.141592653589793
                  #define TWO_PI 6.283185307179586
                  #define HALF_PI 1.5707963267948966
                `);
            
                this.program.use();
                this.model = mat4.translate([], mat4.create(), [0, 25, 0]);
            
                const particles = fill(NUM_PARTICLES, function(i) {
                  const x = random(-20, 20);
                  const y = -50;
                  const z = random(-50);
                  return [ x, y, z ];
                }).reduce((result, p) => result.concat(p[0], p[1], p[2]), []);
            
                const position = new ArrayBuffer(gl);
                position.attrib('position', 3, gl.FLOAT);
                position.data(new Float32Array(particles));
            
                const offset = new ArrayBuffer(gl);
                offset.attrib('offset', 1, gl.FLOAT);
                offset.data(new Float32Array(fill(NUM_PARTICLES, i => random())));
            
                const size = new ArrayBuffer(gl);
                size.attrib('size', 1, gl.FLOAT);
                size.data(new Float32Array(fill(NUM_PARTICLES, i => random(1, 6))));
            
                this.buffers = { position, offset, size };
              }
            
              uniforms(uniforms) {
            
                this.program.use();
            
                for(let name in uniforms) {
                  let setter = this.program[name];
                  if(setter === undefined) continue;
                  setter(uniforms[name]);
                }
            
                return this;
              }
            
              render() {
            
                const gl = this.gl;
            
                this.program.use();
                this.program.u_model(this.model);
            
                for(const key in this.buffers) {
                  this.buffers[key].attribPointer(this.program);
                }
            
                this.buffers.position.bind();
                this.buffers.position.draw(gl.POINTS);
              }
            }
            
            class Snowflake {
            
              constructor(gl) {
            
                // snowflake-complex.js
                const positions = window.complex.positions.reduce((result, value) => {
                  return result.concat([value[0], value[1], value[2]]);
                }, []);
            
                const cells = fill(window.complex.positions.length, i => i);
            
                const vert = document.getElementById('snowflake-vert').textContent;
                const frag = document.getElementById('snowflake-frag').textContent;
            
                this.gl = gl;
                this.program = new Program(gl, vert, frag, `
                  precision mediump float;
                  #define PI 3.141592653589793
                  #define TWO_PI 6.283185307179586
                  #define HALF_PI 1.5707963267948966
                `);
                this.program.use();
                this.model = mat4.create();
            
                const position = new ArrayBuffer(gl);
                position.attrib('position', 3, gl.FLOAT);
                position.attribPointer(this.program);
                position.data(new Float32Array(positions));
            
                const colors = cells.reduce(result => {
                  const rgba = [1.0, 1.0, 1.0, 0.1 + Math.random() * 0.5];
                    return result.concat(rgba, rgba, rgba);
                }, []);
            
                const color = new ArrayBuffer(gl);
                color.attrib('color', 4, gl.FLOAT);
                color.attribPointer(this.program);
                color.data(new Float32Array(colors));
            
                const index = new ArrayBuffer(gl);
                const indices = cells.reduce((result, i) => result.concat([i, i, i]), []);
                index.attrib('index', 1, gl.FLOAT);
                index.attribPointer(this.program);
                index.data(new Float32Array(indices));
            
                this.buffers = { position, color, index };
                this.elements = new IndexBuffer(gl);
                this.elements.data(new Uint16Array(cells));
              }
            
              // nanogl's program.my_uniform() is convenient for setting uniforms,
              // but not necessarily safe according to "Can we use the setters directly?"
              // https://webglfundamentals.org/webgl/lessons/webgl-less-code-more-fun.html
              // this uniforms() method addresses that
              // program.uniforms({ u_model: model, u_projection: projection });
              uniforms(uniforms) {
            
                this.program.use();
            
                for(const name in uniforms) {
                  const setter = this.program[name];
                  if(setter === undefined) continue;
                  setter(uniforms[name]);
                }
            
                return this;
              }
            
              render() {
            
                const gl = this.gl;
                const model = this.model;
            
                this.program.use();
                this.program.u_model(this.model);
            
                for(const key in this.buffers) {
                  this.buffers[key].attribPointer(this.program);
                }
            
                this.elements.bind();
                this.elements.draw(gl.TRIANGLES);
              }
            }
            
            const snowstorm = new Snowstorm(gl);
            const snowflake = new Snowflake(gl);
            
            function centroid(triangle) {
            
              const dimension = triangle[0].length;
              let result = new Array(dimension);
            
              for(let i = 0; i < dimension; i++) {
                const t0 = triangle[0][i];
                const t1 = triangle[1][i];
                const t2 = triangle[2][i];
                result[i] = (t0 + t1 + t2) / 3;
              }
            
              return result;
            }
            
            function fill(size, fn) {
              const array = Array(size);
              for(let i = 0; i < size; i++) {
                array[i] = fn(i);
              }
              return array;
            }
            
            function random(min, max) {
            
              if(arguments.length == 0) {
                return Math.random();
              }
            
              if(Array.isArray(min)) {
                return min[ Math.floor(Math.random() * min.length) ];
              }
            
              if(typeof min == 'undefined') min = 1;
              if(typeof max == 'undefined') max = min || 1, min = 0;
            
              return min + Math.random() * (max - min);
            }
            
            function resize(event) {
            
              const scale = window.devicePixelRatio || 1;
              const width = window.innerWidth;
              const height = window.innerHeight;
            
              gl.canvas.width = width * scale;
              gl.canvas.height = height * scale;
              gl.viewport(0, 0, gl.canvas.width, gl.canvas.height);
            
              uniforms.u_resolution = [width, height];
              mat4.perspective(uniforms.u_projection, Math.PI / 4, width / height, 0.01, 100);
            }
            
            function mousemove(event) {
              event.preventDefault();
              const width = window.innerWidth;
              const height = window.innerHeight;
              const x = (event.touches) ? event.touches[0].pageX : event.pageX;
              const y = (event.touches) ? event.touches[0].pageY : event.pageY;
              uniforms.u_mouse[0] = (x / width)  *  2 - 1;
              uniforms.u_mouse[1] = (y / height) * -2 + 1;
            }
            
            function animate(time) {
            
              uniforms.u_time = time;
              mat4.lookAt(uniforms.u_view, camera.eye, camera.target, camera.up);
            
              snowstorm.uniforms(uniforms);
              snowstorm.render();
            
              const mouse = [...uniforms.u_mouse];
              transform.rotation[0] += (-mouse[0] - transform.rotation[0]) * 0.1;
              transform.rotation[1] += ( mouse[1] - transform.rotation[1]) * 0.1;
            
              const model = snowflake.model;
              mat4.identity(model);
              mat4.rotateX(model, model, transform.rotation[1]);
              mat4.rotateY(model, model, transform.rotation[0]);
            
              snowflake.uniforms(uniforms);
              snowflake.uniforms({u_model: model});
              snowflake.render();
            
              requestAnimationFrame(animate);
            }
            
            function init() {
              window.addEventListener('resize', resize);
              window.addEventListener('mousemove', mousemove);
              window.addEventListener('touchmove', mousemove);
              window.addEventListener('touchend', (event) => uniforms.u_mouse = [0, 0]);
              window.addEventListener('contextmenu', (event) => event.preventDefault());
              resize();
              requestAnimationFrame(animate);
            }
            
            init();
        </script>
	</body>
</html>
```

