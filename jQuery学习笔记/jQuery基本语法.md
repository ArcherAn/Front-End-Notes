layout:

- post

title:

- jQuery基本语法

categories:

- jQuery学习笔记

tags: 

- jQuery

- JavaScript

---
## **jQuery基本语法** ##
### **jQuery语法** ###
jQuery 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。
基础语法： $(selector).action()

- $符号定义 jQuery
- 选择符（selector）"查询"和"查找" HTML 元素
- jQuery 的 action() 执行对元素的操作

<!--more-->

实例：

- $(this).hide() - 隐藏当前元素
- $("p").hide() - 隐藏所有p元素
- $("p.test").hide() - 隐藏所有 class="test" 的p元素
- $("#test").hide() - 隐藏所有 id="test" 的元素



示例：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
    <script>
        $(document).ready(function(){
           $("p").click(function(){
             $(this).hide();
           });
        });
    </script>
    </head>
    <p>如果你点我，我就会消失</p>
    <p>继续点我</p>
    <p>接着点我</p>
    </body>
    </html>
```

**$(document).ready()**方法允许我们在文档完全加载完后执行函数。

### **jQuery选择器** ###
jQuery 选择器允许您对 HTML 元素组或单个元素进行操作。
jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 CSS 选择器，除此之外，它还有一些自定义的选择器。
jQuery 中所有选择器都以美元符号开头：$()

示例：

    
```html
    <!DOCTYPE html>
    <html>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js">
    </script>
    <script>
    $(document).ready(function(){
        $("button").click(function(){
           $("ul li:first-child").hide();
       });
    });
    </script>
    </head>
    <body>
    
    <p>List 1:</p>
    <ul>
      <li>Coffee</li>
      <li>Milk</li>
      <li>Tea</li>
    </ul>
    
    <p>List 2:</p>
    <ul>
      <li>Coffee</li>
      <li>Milk</li>
      <li>Tea</li>
    </ul>
    
    <button>Click me</button>
    
    </body>
    </html>
```

其中**$("ul li:first-child")**表示每一个ul下的第一个li元素。

### jQuery事件 ###
常见的DOM事件：


| 鼠标事件 | 键盘事件 | 表单事件 | 文档/窗口事件 |
| :--------: | :--------:| :--: |:--:|
| click| keypress| submit| load|
| dbclick| keydown| change| resize|
| mouseenter| keyup| focus| scroll|
| mouseleave| | blur| unload |


具体事件示例如下：
```html
    <!DOCTYPE html>
    <html>
       <head lang="en">
       <meta charset="UTF-8">
       <title></title>
       <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
       <script>
          $(document).ready(function(){
             $("p.test1").click(function(){
                $(this).hide();
             });
          });
          $(document).ready(function(){
             $("p.test2").dblclick(function(){
                $(this).hide();
             });
          });
          $(document).ready(function(){
             $("#p1").mouseenter(function(){
                 alert("您的鼠标移动到了id=p1的元素上了");
             });
          });
          $(document).ready(function(){
             $("input").focus(function(){
                $(this).css("background-color","#cccccc");
             });
             $("input").blur(function(){
                $(this).css("background-color","#ffffff");
             });
         });
    </script>
    </head>
      <p class="test1">如果你点我，我就会消失</p>
      <p class="test1">继续点我</p>
      <p class="test1">接着点我</p>
      <p class="test2">双击鼠标左键的，我就消失。</p>
      <p class="test2">双击我消失！</p>
      <p class="test2">双击我也消失！</p>
      <p id="p1">鼠标指针进入此处，会看到弹窗。</p>
    
      Name: <input type="text" name="fullname"><br>
      Email: <input type="text" name="email">
    
    </body>
    </html>
```


