layout:

- post

title:

- jQuery效果

categories:

- jQuery学习笔记

tags: 

- jQuery

- JavaScript

---
## **jQuery效果** ##
- 隐藏/显示

  **语法：**
    $(selector).show(speed,callback);
    $(selector).hide(speed,callback);
    $(selector).toggle(speed,callback);
  
- jQuery滑动效果
jQuery 拥有以下滑动方法：
    jQuery slideDown()；
    jQuery slideUp();
    jQuery slideToggle();

<!--more-->

- jQuery动画效果
**语法：**
    $(selector).animate({params},speed,callback);
必需的 params 参数定义形成动画的 CSS 属性。
可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是动画完成后所执行的函数名称。
    
- jQuery Callback方法
Callback 函数在当前动画**100%**完成之后执行。

以上具体效果实现如下示例：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
    <script>
    /*
    若要加入淡入淡出，则调用fadeToggle()函数即可;
    滑动效果为：slideUp()、sildeDown()、slideToggle();
     */
    $(document).ready(function(){
         $("#hide_show").click(function(){
           $(".div1").toggle(1000);
           $(".div2").toggle(2000);
           $(".div3").toggle(3000);
         });
    });
    /*
    jQuery动画 - animate()方法
    $(selector).animate({params},speed,callback)
    必需的 params 参数定义形成动画的 CSS 属性。
    可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
    可选的 callback 参数是动画完成后所执行的函数名称。
    stop()方法用于停止所有jQuery中的动画效果。
     */
    $(document).ready(function(){
        $("#animate1").click(function(){
           $(".div4").animate({
              left:'250px',
              opacity:'0.5',  //透明度
              height:'+=150px',  //表示相对值
              width:'+=150px'
           });
       });
    });
    
    $(document).ready(function(){
       $("#animate2").click(function(){
         var div =$(".div5");
         div.animate({height:'300px',opacity:'0.4'},"slow");
         div.animate({width:'300px',opacity:'0.8'},"slow");
         div.animate({height:'100px',opacity:'0.4'},"slow");
         div.animate({width:'100px',opacity:'0.8'},"slow");
         div.animate({fontSize:'2em'},"slow");
       });
    });
    
    </script>
    <style>
    </style>
    </head>
    <body>
       <button id="hide_show">Click me</button>
       <p></p>
       <div class="div1" style="width: 80px;height: 80px;background-color: red"></div>
       <div class="div2" style="width: 80px;height: 80px;background-color: green"></div>
       <div class="div3" style="width: 80px;height: 80px;background-color: blue"></div>
       <button id="animate1">测试动画效果</button>
       <div class="div4" style="width: 80px;height: 80px;background-color: darkviolet"></div>
       <button id="animate2">测试动画队列功能</button>
       <div class="div5" style="width: 100px;height: 100px;background-color: yellow">HELLO</div>
    </body>
    </html>
```


