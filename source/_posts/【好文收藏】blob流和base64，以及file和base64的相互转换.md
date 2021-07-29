---
title: 【好文收藏】blob流和base64，以及file和base64的相互转换
date: 2021-07-29 22:23:25
tags:	
	- 前端
---



### file文件转换为base64

> 通常用于得到base64格式图片页面展示或上传

```javascript
var reader = new FileReader();
reader.readAsDataURL(this.files[0]);
reader.onload = function(event){
    console.log(event.target.result); //获取到base64格式图片
};
```

### base64转换为file

```javascript
function dataURLtoFile(dataurl, filename) {//将base64转换为文件
  var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
      bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
  while(n--){
      u8arr[n] = bstr.charCodeAt(n);
  }
  return new File([u8arr], filename, {type:mime});
}    
```

测试案例：

```javascript
<input type="file" name="" id="imgfile">
<script>
  var base64Img = '';
  document.getElementById('imgfile').onchange = function(){
      var fileReader = new FileReader();
      fileReader.readAsDataURL(this.files[0]);
      fileReader.onload = function(){
          base64Img = fileReader.result;
          console.log(dataURLtoFile(base64Img,'img11'))
      }
  }
   function dataURLtoFile(dataurl, filename) {//将base64转换为文件
      var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
          bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
      while(n--){
          u8arr[n] = bstr.charCodeAt(n);
      }
      return new File([u8arr], filename, {type:mime});
  }    
</script>
```



### base64转换为blob流

```javascript
function dataURItoBlob(base64Data) {
    //console.log(base64Data);//data:image/png;base64,
    var byteString;
    if(base64Data.split(',')[0].indexOf('base64') >= 0)
        byteString = atob(base64Data.split(',')[1]);//base64 解码
    else{
        byteString = unescape(base64Data.split(',')[1]);
    }
    var mimeString = base64Data.split(',')[0].split(':')[1].split(';')[0];//mime类型 -- image/png

    // var arrayBuffer = new ArrayBuffer(byteString.length); //创建缓冲数组
    // var ia = new Uint8Array(arrayBuffer);//创建视图
    var ia = new Uint8Array(byteString.length);//创建视图
    for(var i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i);
    }
    var blob = new Blob([ia], {
        type: mimeString
    });
    return blob;
}  
```

### blob流转换为base64

```javascript
function blobToDataURI(blob, callback) {
   var reader = new FileReader();
   reader.readAsDataURL(blob);
   reader.onload = function (e) {
       callback(e.target.result);
   }
}
```

测试案例，可直接复制运行

```javascript
<input type="file" id="imgfile">
<\img src="" id="img" alt=""> //这里图片总是转义，暂且这么写，知道是img就行
<\img src="" id="img2" alt="">
<script>
 document.getElementById('imgfile').onchange = function(){
     reads(this.files[0],function(base64Data){   //获取图片的base64格式，显示
         document.getElementById("img").src= base64Data;
         var blob = dataURItoBlob(base64Data); //转换为blob格式
         blobToDataURI(blob,function(result){    //blob格式再转换为base64格式
             document.getElementById('img2').src = result;
         })
     });
 }
 function reads(_file,callback){
     var reader = new FileReader();
     reader.readAsDataURL(_file);
     reader.onload = function(){
         callback(reader.result);
     };
 }
 function dataURItoBlob(base64Data) {
     //console.log(base64Data);//data:image/png;base64,
     var byteString;
     if(base64Data.split(',')[0].indexOf('base64') >= 0)
         byteString = atob(base64Data.split(',')[1]);//base64 解码
     else{
         byteString = unescape(base64Data.split(',')[1]);
     }
     var mimeString = base64Data.split(',')[0].split(':')[1].split(';')[0];//mime类型 -- image/png

     // var arrayBuffer = new ArrayBuffer(byteString.length); //创建缓冲数组
     // var ia = new Uint8Array(arrayBuffer);//创建视图
     var ia = new Uint8Array(byteString.length);//创建视图
     for(var i = 0; i < byteString.length; i++) {
         ia[i] = byteString.charCodeAt(i);
     }
     var blob = new Blob([ia], {
         type: mimeString
     });
     return blob;
 }  

 function blobToDataURI(blob, callback) {
     var reader = new FileReader();
     reader.readAsDataURL(blob);
     reader.onload = function (e) {
         callback(e.target.result);
     }
 }
</script>
```