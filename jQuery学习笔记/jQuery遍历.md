layout:

- post

title:

- jQuery遍历

categories:

- jQuery学习笔记

tags: 

- jQuery

- JavaScript

---
## **jQuery遍历** ##
### **jQuery向上遍历** ###
- parent() - 返回被选元素的直接父元素
- parents() - 返回被选元素的所有祖先元素
- parentsUntil() - 返回介于两个给定元素之间的所有祖先元素

<!--more-->

### **jQuery向下遍历** ###
- children() - 返回被选元素的所有直接子元素
- find() - 返回被选元素的后代元素，一路向下直到最后一个后代

### **jQuery同节点遍历** ###

- siblings() - 返回被选元素的所有同胞元素
- next() - 返回被选元素的下一个同胞元素，只返回一个元素
- nextAll() - 返回被选元素的所有跟随的同胞元素
- nextUntil() - 返回介于两个给定参数之间的所有跟随的同胞元素
- prev() - 返回被选元素的上一个同胞元素，只返回一个元素
- prevAll() - 返回被选元素的所有之前的同胞元素
- prevUntil() - 返回介于两个给定参数之间的所有之前的同胞元素

### **jQuery过滤** ###

- first()  - 返回被选元素的首个元素
- last() - 返回被选元素的最后一个元素
- eq() - 返回被选元素中带有指定索引号的元素
- filter() - 允许规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回
- not() - not() 方法与 filter() 相反。返回不匹配标准的所有元素

具体上述实例如下所示：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
         <script>
            $(document).ready(function(){
               $("span").parent().css({"color":"red","border":"2px solid red"});
            });
           /*
           过滤功能
           其它过滤函数功能类似
           */
           $(document).ready(function(){
              $("p").filter(".intro").css("color","yellow");
           });
    </script>
    <style type="text/css">
          .ancestors *{
             display: block;
             border: 2px solid lightgrey;
             color: lightgrey;
             padding: 5px;
             margin: 15px;
          }
    </style>
    </head>
    <body  class="ancestors"> body (great-great-grandparent)
	    <div style="width:500px;">div (great-grandparent)
	    <ul>ul (grandparent)
	    <li>li (direct parent)
	    <span>span</span>
	    </li>
	    </ul>
	    </div>
	    <p>My name is Donald.</p>
	    <p class="intro">I live in Duckburg.</p>
	    <p class="intro">I love Duckburg.</p>
	    <p>My best friend is Mickey.</p>
    </body>
    </html>
```