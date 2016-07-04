layout:

- post

title:

- jQuery-Ajax

categories:

- jQuery学习笔记

tags: 

- jQuery

- JavaScript

---
## **jQuery-Ajax** ##
### **jQuery-AJAX load()方法** ###

**load()**方法从服务器加载数据，并把返回的数据放入被选元素中
**语法：**
    `$(selector).load(URL,data,callback);`
- URL - 必须的参数，规定您希望加载的 URL
- data - 可选的参数，规定与请求一同发送的查询字符串键/值对集合
- callback - 可选的参数，是 load() 方法完成后所执行的函数名称

<!--more-->

具体实现如图所示:
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
	    <script>
	    /*
	    Ajax是与服务器交换数据的技术，它在不重载全部页面的情况下，实现了对页面的部分刷新
	     */
	    $(document).ready(function(){
	        $("button").click(function(){
	          $("#div1").load("demo_test.txt");
	        });
	    });
	    </script>
    </head>
    <body>
	    <div id="div1"><h2>使用 jQuery AJAX 修改文本内容</h2></div>
	    <button>获取外部内容</button>
    </body>
    </html>
```
可选的**callback**参数规定当 load() 方法完成后所要允许的回调函数。回调函数可以设置不同的参数：
- responseTxt - 包含调用成功时的结果内容
- statusTXT - 包含调用的状态
- xhr - 包含 XMLHttpRequest 对象

核心代码示例如下：

```js
    $("button").click(function(){
      $("#div1").load("demo_test.txt",function(responseTxt,statusTxt,xhr){
         if(statusTxt=="success")
           alert("External content loaded successfully!");
         if(statusTxt=="error")
           alert("Error: "+xhr.status+": "+xhr.statusText);
      });
    });
```

### **jQuery-AJAX get()/post()方法** ###

- GET - 从指定的资源请求数据
- POST - 向指定的资源提交要处理的数据

GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据。
POST 也可用于从服务器获取数据。不过，POST 方法不会缓存数据，并且常用于连同请求一起发送数据。

**$.get()**方法通过 HTTP GET 请求从服务器上请求数据。
**语法：**
`$.get(URL,callback);`

**$.post()**方法通过 HTTP POST 请求从服务器上请求数据。
**语法:**
`$.post(URL,data,callback);`

具体实例如下：
```html
    <!DOCTYPE html>
    <html>
    <head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
          <script>
             $(document).ready(function(){
                $("#btn1").click(function(){
                  $.get("demo_test.php",function(data,status){
                     alert("数据: "+ data + "\n状态: "+ status);
                  });
               });
            });
            $(document).ready(function(){
                 $("#bnt2").click(function(){
                     $.post("demo_test_post.php",
                     {
                       name:"David",
                       city:"WuHan"
                     },
                     function(data,status){
                       alert("Data: "+ data + "\nStatus: "+ status);
                     });
                });
            });
    </script>
    </head>
    <body>
	    <button id="btn1">发送一个HTTP GET请求并获取返回结果</button>
	    <br>
	    <button id="btn2">发送一个HTTP POST请求页面并获取返回结果</button>
    </body>
    </html>
```
其中demo_test.php为：
```php
    <? php
       echo "This is some text from an external PHP file.";
    ?>
```
demo_test_post.php为:
```php
    <?php
       $name = isset($_POST['name']) ? htmlspecialchars($_POST['name']) : '';
       $city = isset($_POST['city']) ? htmlspecialchars($_POST['city']) : '';
       echo 'Dear ' . $name;
       echo 'Hope you live well in ' . $city;
    ?>
```
