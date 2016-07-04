layout:

- post

title:

- jQuery-HTML

categories:

- jQuery学习笔记

tags: 

- jQuery

- JavaScript

---
## **jQuery-HTML** ##

### **jQuery捕获/设置** ###
jQuery 中非常重要的部分，就是操作 DOM 的能力。
jQuery 提供一系列与 DOM 相关的方法，这使访问和操作元素和属性变得很容易。
**获得内容 - text()、html() 以及 val()**
- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素的内容（包括 HTML 标记）
- val()  - 设置或返回表单字段的值
- attr() - 设置或改变属性值

<!--more-->

### **jQuery添加/删除元素** ###
**添加新的HTML内容有以下四种jQuery方法：**
- append()  - 在被选元素的结尾插入内容
- prepend() - 在被选元素的开头插入内容
- after()   - 在被选元素之后插入内容
- before()  - 在被选元素之前插入内容

**如需删除元素和内容，一般可使用以下两个 jQuery 方法：**
- remove() - 删除被选元素（及其子元素）
- empty()  - 从被选元素中删除子元素

具体实例如下所示：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
    <script>
        /*
        jQuery 捕获/设置
        text() - 设置或返回所选元素的文本内容
        html() - 设置或返回所选元素的内容（包括 HTML 标记）
        val() - 设置或返回表单字段的值
        attr()- 方法用于获取属性值
        */
        $(document).ready(function(){
          $("#btn1").click(function(){
            alert("Text: "+$("#test").text());   //设置：$("#test").text("Hello World")
          });
         $("#btn2").click(function(){
            alert("HTML: "+$("#test").html());  //设置：$("#test").html("<b>Hello World</b>")
         });
         $("#btn3").click(function(){
            alert("Value: "+$("#test1").val());  //设置：$("#test").val("Jack")
         });
         $("#btn4").click(function(){
            alert(" Href Value: "+$("#bd").attr("href")); //设置：$("#bd").attr("href","https://sina.cn")
         });
       });
    </script>
    
    <script>
        /*
        jQuery 添加/删除元素
        jQuery 删除元素的方法有两种：remove()、empty()
        */
        $(document).ready(function(){
            $("#btn5").click(function(){
              $("ol").append("<li> Appended item</li>");  // 在被选元素的结尾插入内容
              $("ol").prepend("<li> Appended item</li>");  //在被选元素的开头插入内容
            });
        });
    </script>
    </head>
    <body>
       <p id="test">This is some <b>blod</b> text in a paragraph</p>
       <p>Name:<input type="text" id="test1"value="HuangAn"></p>
       <p><a href="https://www.baidu.com" id="bd">百度</a></p>
       <button id="btn1">Show Text</button>
       <button id="btn2">Show HTML</button>
       <button id="btn3">Show Value</button>
       <button id="btn4">Show Href Value</button>
       <ol>
       <li>item 1</li>
       <li>item 2</li>
       </ol>
       <button id="btn5">Append item</button>
    </body>
    </html>
```

### **jQuery操作CSS** ###
**jQuery拥有以下若干种进行css操作的方法：**
- addClass() - 向被选元素添加一个或多个类
- removeClass() - 从被选元素删除一个或多个类
- toggleClass() - 对被选元素进行添加/删除类的切换操作
- css() - 设置或返回样式属性

具体实例如下所示：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
    <script>
         /*
         添加类使用函数addClass("xx")
         删除class属性为removeClass("xx")
         切换添加/删除类的操作为toggleClass("xx")
         */
         $(document).ready(function(){
            $("#btn1").click(function(){
               $("h1,p").addClass("blue");
               $("div").addClass("important blue");  //在addClass中添加了多个类
            });
         });
    
         /*
         css()方法设置或返回被选元素一个或多个样式属性
         */
         $(document).ready(function(){
            $("#btn2").click(function(){
               $(".p").css({"background-color":"yellow","font-size":"200%"});
            });
         });
    </script>
    <style type="text/css">
       .important{
         font-weight: bold;
         font-size: xx-large;
       }
       .blue{
         color:blue;
       }
    </style>
    </head>
    <body>
      <h1>Heading 1</h1>
      <p>This is a paragraph</p>
      <div>This is some important text!</div>
      <br>
      <button id="btn1">Add some classes to elements</button>
      <p class="p" style="background-color: red">This is first paragraph</p>
      <p class="p" style="background-color: blue">This is second paragraph</p>
      <button id="btn2">Change the css </button>
    </body>
    </html>
```
### **jQuery处理元素尺寸** ###
**jQuery 提供多个处理尺寸的重要方法：**
- width()  -设置或返回元素的宽度（**不包括**内边距、边框或外边距）。
- height()  -设置或返回元素的高度（**不包括**内边距、边框或外边距）。
- innerWidth() -返回元素的宽度（**包括**内边距）。
- innerHeight() -返回元素的高度（**包括**内边距）。
- outerWidth() -返回元素的宽度（**包括**内边距和边框）。
- outerHeight() -返回元素的高度（**包括**内边距和边框）。

其中各个方法改变的尺寸结构如下图所示：
![](http://i.imgur.com/jI4cKck.gif)


具体实现实例如下所示：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
    <script>
         $(document).ready(function(){
            $("button").click(function(){
               var txt="";
               txt+="Width of div: " + $("#div1").width() + "</br>";
               txt+="Height of div: " +$("#div1").height() + "</br>";
               txt+="Inner width of div: "+$("#div1").innerWidth() + "</br>";
               txt+="Inner height of div: "+$("#div1").innerHeight() + "</br>";
               txt+="Outer width of div: "+$("#div1").outerWidth() + "</br>";
               txt+="Outer height of div: "+$("#div1").outerHeight();
               $("#div1").html(txt);
           })；
        })；
    </script>
    </head>
    <body>
       <div id="div1" style="height:100px;width:300px;padding:10px;margin:3px;border:1px solid blue;background-color:lightblue;"></div>
       <br>
       <button>Display dimensions of div</button>
    </body>
    </html>
```
