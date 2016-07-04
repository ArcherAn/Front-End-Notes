layout:

- post

title:

- jQuery属性

categories:

- jQuery学习笔记

tags: 

- jQuery

- JavaScript

---
## **jQuery属性** ##
- context - **在版本 1.10 中被废弃**。包含被传递到 jQuery 的原始上下文(检测上下文)
- jquery - 返回的字符串包含 jQuery 的版本号
- jQuery.fx.interval - 用于改变以毫秒计的动画运行速率

<!--more-->

   **示例如下：**
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js">
    </script>
    <script>
         $(document).ready(function(){
            $("#toggle").on("click",function(){
              $("div").toggle(5000);
             });
           $("#interval").on("click",function(){
             jQuery.fx.interval = 500;
           });
         });
    </script>
    </head>
    <body>
	    <button id="toggle">Toggle div</button>
	    <button id="interval">Run animation with less frames</button>
	    <div style="background:#98bf21;height:100px;width:100px;margin:50px;"></div>
    </body>
    </html>
```
- jQuery.fx.off - 对所有动画进行全局禁用或启用。其中可以用$.fx.off代替jQuery.fx.off
- jQuery.support - 表示不同浏览器特性或漏洞的属性集
- length - 包含 jQuery 对象中元素的数目
	`$("button").click(function(){
	alert($("li").length);
	});`
