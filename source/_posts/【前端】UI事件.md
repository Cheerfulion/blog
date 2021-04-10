---
title: UI事件
description: '-'
tags:
  - WEB开发
  - Web前端
  - 从入门到放弃系列
abbrlink: cf5573cc
date: 2021-03-29 22:22:30
---



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>UI事件</title>
	</head>
	<body>
        <input type="text" name="username" style="border-width: 10px;">
        <script>
            // 1. 焦点处理（focus, blur）
            // 默认情况下，整个文档处于焦点状态，但是点击或者Tab键可以改变焦点的位置。
            var input = document.getElementsByName('username')[0];
            window.onload = function(){
                // 注意： 如果是隐藏字段(<input type="hidden" />)或者css设置了display:none;或者visibility:hidden;设置其获取焦点将引发异常  
                input.focus();
            }
            input.onfocus = function(){
                this.style.borderStyle = 'outset';
            }
            input.onblur = function(){
                this.style.borderStyle = 'inset';
            }
            
            
            // 2. 选择文本
            // 当在文本框或文本区域内选择文本时，将触发select事件。
            // 注意：页面选择文字不会触发，可以使用select()方法触发该事件。
            var input = document.getElementsByName('username')[0];
            input.onselect = function(){
                if(document.selection){
                    // IE
                    var o = document.selection.createRange(); // 创建一个选择区域
                    o.text.length > 0 && alert("your selected text is: " + o.text);
                }else{
                    // DOM    
                    var p1= input.selectionStart;
                    var p2= input.selectionEnd;
                    var text = input.value.substring(p1, p2);
                    text.length > 1 && alert("your selected text is: " + text);
                    
                }
            }
            
            
            3. 字段值变化监测 onchange
            表单元素值变化时触发，主要用于input,select和textarea元素。对于input和textarea来说，当失去焦点且值改变的时候触发，对于select，在其选项改变时触发。也就是说，对于readonly的input和textarea，值改变无法触发change事件，因为没有失去焦点这个过程。但是对于select来说，不失去焦点也会触发chagne事件。
            
             关于blue和change事件发生顺序并没有严格的规定，不同浏览器没有统一规定。因此不能假定这两个事件总会以某种顺序依次触发。
            
        </script>
	</body>
</html>

```

