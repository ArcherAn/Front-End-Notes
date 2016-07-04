ayout:

- post

title:

- 一个简单的express应用

categories:

- Node.js基础学习

tags: 

- Node.js

- express

---
**知识点：**

1. 包管理器 npm 。使用 npm 安装包，并自动安装所需依赖。
2. 框架 express 。学习新建 express 实例，并定义 routes ，产生输出。

**包管理器 npm**

npm 可以自动管理包的依赖。只需要安装你想要的包, 不必考虑这个包的依赖包。在 PHP 中, 包管理使用的 <span style="color:red;background-color:rgb(249,242,223)">`Composer`</span>, python 中，包管理使用 <span style="color:red;background-color:rgb(249,242,223)">`easy_install`</span> 或者 <span style="color:red;background-color:rgb(249,242,223)">`pip`</span>，ruby 中我们使用 <span style="color:red;background-color:rgb(249,242,223)">`gem`</span>。而在 Node.js 中，对应就是 <span style="color:red;background-color:rgb(249,242,223)">`npm`</span>，npm 是 <span style="color:red;background-color:rgb(249,242,223)">`Node.js Package Manager`</span> 的意思。

**框架 Express**

**express** 是 Node.js 应用最广泛的 web 框架，现在是 4.x 版本，它非常薄。跟 Rails 比起来，完全两个极端。

<!--more-->

首先新建一个文件夹lesson1，安装 express

![](http://i.imgur.com/f6mvraM.png)

安装完成后，我们的 lesson1 目录下应该会出现一个
 <span style="color:red;background-color:rgb(249,242,223)">`node_modules` </span>文件夹。<br>
里面如果出现 express 文件夹则说明安装成功。<br>
或者使用npm命令提供更清晰直观的显示：<br>
 <div style="color:red;background-color:rgb(249,242,223);height:25px;width:100px;text-align:center">   $ npm list </div>
新建一个 app.js 文件,copy进去以下代码：

    // 这句的意思就是引入 `express` 模块，并将它赋予 `express` 这个变量等待使用。
    var express = require('express');
    // 调用 express 实例，它是一个函数，不带参数调用时，会返回一个 express 实例，将这个变量赋予 app 变量。
    var app = express();
    // app 本身有很多方法，其中包括最常用的 get、post、put/patch、delete，在这里我们调用其中的 get 方法，为我们的 `/` 路径指定一个 handler 函数。
    // 这个 handler 函数会接收 req 和 res 两个对象，他们分别是请求的 request 和 response。
    // request 中包含了浏览器传来的各种信息，比如 query 啊，body 啊，headers 啊之类的，都可以通过 req 对象访问到。
    // res 对象，我们一般不从里面取信息，而是通过它来定制我们向浏览器输出的信息，比如 header 信息，比如想要向浏览器输出的内容。这里我们调用了它的 #send 方法，向浏览器输出一个字符串。
    app.get('/', function (req, res) {
      res.send('Hello World');
    });
    
    // 定义好我们 app 的行为之后，让它监听本地的 3000 端口。这里的第二个函数是个回调函数，会在 listen 动作成功后执行，我们这里执行了一个命令行输出操作，告诉我们监听动作已完成。
    app.listen(3000, function () {
      console.log('app is listening at port 3000');
    });


执行 <div style="color:red;background-color:rgb(249,242,223);height:25px;width:100px;text-align:center">$ node app.js</div>

![](http://i.imgur.com/1h3XaCi.png)

**补充知识：**<br>
**端口的作用：**<br>
<p style="color:red">通过端口来区分出同一电脑内不同应用或者进程，从而实现一条物理网线(通过分组交换技术-比如internet)同时链接多个程序。</p><br>
端口号是一个 16位的 uint, 所以其范围为 1 to 65535 (对TCP来说, port 0 被保留，不能被使用。对于UDP来说, source端的端口号是可选的， 为0时表示无端口)。<br>
**URL：**<br>
RFC1738 定义的url格式笼统版本<span style="color:red;background-color:rgb(249,242,223)">scheme:scheme-specific-part</span>， scheme有我们很熟悉的http、https、ftp，以及著名的<span style="color:red;background-color:rgb(249,242,223)">ed2k</span>，<span style="color:red;background-color:rgb(249,242,223)">thunder</span>。







