layout:

- post

title:

- 浏览器端测试：mocha , chai

categories:

- Node.js基础学习

tags: 

- Node.js

- 测试

- mocha

- chai

---

**目标：**
建立一个 lesson7 项目，在其中编写代码，我们在lesson7中新建一个文件夹命名为 vendor 
这次我们测试的对象是之前提到的 fibonacci 函数<br>
此函数的定义为 &nbsp;&nbsp;&nbsp;<span style="color:red;background-color:rgb(249,242,223)">`int fibonacci(int n)`</span><br>
- 当 n === 0 时，返回 0；n === 1时，返回 1;<br>
- n > 1 时，返回 <span style="color:red;background-color:rgb(249,242,223)">`fibonacci(n) === fibonacci(n-1) + fibonacci(n-2)`</span>，如 <span style="color:red;background-color:rgb(249,242,223)">`fibonacci(10) === 55`</span>;<br>


**知识点：**
1. 学习使用测试框架 mocha 进行前端测试 : [http://mochajs.org/](http://mochajs.org/)<br>
2. 了解全栈的断言库 chai: [http://chaijs.com/](http://chaijs.com/)

<!--more-->

**前端脚本单元测试**
前面一节的内容主要是针对后端环境中 node 的一些单元测试方案，出于应用健壮性的考量，针对前端 js 脚本的单元测试也非常重要。而前后端通吃，也是 mocha 的一大特点。首先，前端脚本的单元测试主要有两个困难需要解决。<br>
1. 运行环境应当在浏览器中，可以操纵浏览器的DOM对象，且可以随意定义执行时的 html 上下文。
2. 测试结果应当可以直接反馈给 mocha，判断测试是否通过。

首先我们应该搭建一个测试原型，用 mocha 自带的脚手架自动生成。

![](http://i.imgur.com/hFlqQpD.png)

mocha会生成一个简单的测试原型，目录结构为：

![](http://i.imgur.com/ijq0HGJ.png)

其中 index.html 是单元测试的入口，tests.js 是测试用例文件。<br>
我们直接在 index.html 插入上述示例的 fibonacci 函数以及断言库 chaijs。
```html

    <!DOCTYPE html>
    <html>
      <head>
    <title>Mocha</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="mocha.css" />
      </head>
      <body>
    <div id="mocha"></div>
    <script src="mocha.js"></script>
    <script>mocha.setup('bdd');</script>
    <script src="tests.js"></script>
    	<script src='https://raw.githubusercontent.com/chaijs/chai/master/chai.js'></script>
    <script>
      var fibonacci = function (n) {
    		if (n === 0) {
    		  return 0;
    		}
    		if (n === 1) {
    		  return 1;
    		}
    		return fibonacci(n-1) + fibonacci(n-2);
    	  };
    </script>
      </body>
    </html>
```

然后在tests.js中写入对应测试用例:
```js

    var should = chai.should();
    describe('simple test', function () {
      it('should equal 0 when n === 0', function () {
    window.fibonacci(0).should.equal(0);
      });
    });
```

打开index.html，浏览器显示如图所示:
![](http://i.imgur.com/enKttgO.png)