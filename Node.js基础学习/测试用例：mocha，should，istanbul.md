layout:

- post

title:

- 测试用例：mocha，should

categories:

- Node.js基础学习

tags: 

- Node.js

- 测试

- should

- mocha

---

**目标：**
建立一个 lesson6 项目，在其中编写代码。main.js: 其中有个 fibonacci 函数。<br>
此函数的定义为 &nbsp;&nbsp;&nbsp;<span style="color:red;background-color:rgb(249,242,223)">`int fibonacci(int n)`</span><br>
- 当 n === 0 时，返回 0；n === 1时，返回 1;<br>
- n > 1 时，返回 <span style="color:red;background-color:rgb(249,242,223)">`fibonacci(n) === fibonacci(n-1) + fibonacci(n-2)`</span>，如 <span style="color:red;background-color:rgb(249,242,223)">`fibonacci(10) === 55`</span>;<br>
- n 不可大于10，否则抛错，因为 Node.js 的计算性能没那么强。<br>
- n 也不可小于 0，否则抛错，因为没意义。<br>
- n 不为数字时，抛错。<br>

test/main.test.js: 对 main 函数进行测试，并使行覆盖率和分支覆盖率都达到 100%。


<!--more-->
**知识点：**
1. 学习使用测试框架 mocha : [http://mochajs.org/](http://mochajs.org/)<br>
2. 学习使用断言库 should : [https://github.com/tj/should.js](https://github.com/tj/should.js)<br>

首先执行 <span style="color:red;background-color:rgb(249,242,223)">`npm init`</span> 创建 package.json<br>
其次，建立main.js文件，编写 fibonacci 函数。
```js

    var fibonacci = function (n) {
      if (n === 0) {
        return 0;
      }
      if (n === 1) {
        return 1;
      }
      return fibonacci(n-1) + fibonacci(n-2);
    };
    
    if (require.main === module) {
      // 如果是直接执行 main.js，则进入此处
      // 如果 main.js 被其他文件 require，则此处不会执行。
      var n = Number(process.argv[2]);
      console.log('fibonacci(' + n + ') is', fibonacci(n));
    }
```

执行 <span style="color:red;background-color:rgb(249,242,223)">`$ node main.js 10`</span>

![](http://i.imgur.com/zTevhDq.png)

接下来开始测试驱动开发，现在简单的实现已经完成，那我们就对它进行一下简单测试吧。

我们首先得把 main.js 里面的 fibonacci 暴露出来。加一句

<span style="color:red;background-color:rgb(249,242,223)">`exports.fibonacci = fibonacci`</span>

然后在 test/main.test.js 中引用我们的 main.js，并开始一个简单的测试。
```js

    // file: test/main.test.js
    var main = require('../main');
    var should = require('should');
    
    describe('test/main.test.js', function () {
      it('should equal 55 when n === 10', function () {
    main.fibonacci(10).should.equal(55);
      });
    });
```

安装全局的 mocha: <span style="color:red;background-color:rgb(249,242,223)">`$ npm install mocha -g`</span><br>

<span style="color:red;background-color:rgb(249,242,223)">`-g`</span> 与 非<span style="color:red;background-color:rgb(249,242,223)">`-g`</span> 的区别，就是安装位置的区别，g 是 global 的意思。如果不加的话，则安装 mocha 在你的项目目录下面；如果加了，则这个 mocha 是安装在全局的，如果 mocha 有可执行命令的话，那么这个命令也会自动加入到你系统 $PATH 中的某个地方。

然后执行 <span style="color:red;background-color:rgb(249,242,223)">`$ mocha`</span>

![](http://i.imgur.com/5wLaqy1.png)

其中 <span style="color:red;background-color:rgb(249,242,223)">`describe`</span> 中的字符串，用来描述你要测的主体是什么；<span style="color:red;background-color:rgb(249,242,223)">`it`</span> 当中，描述具体的 case 内容。

引入的should模块，是一个断言库，而 mocha是测试框架。<br>
比如测试一个数是不是大于3，则是 <span style="color:red;background-color:rgb(249,242,223)">`(5).should.above(3)`</span>；测试一个字符串是否有着特定前缀：<span style="color:red;background-color:rgb(249,242,223)">`'foobar'.should.startWith('foo')`</span>;

这个时候再看一遍fibonacci 函数的几个要求：<br>
<div style="background-color:black;color:white">
* 当 n === 0 时，返回 0；n === 1时，返回 1;<br>
* n > 1 时，返回 `fibonacci(n) === fibonacci(n-1) + fibonacci(n-2)`，如 `fibonacci(10) === 55`;<br>
* n 不可大于10，否则抛错，因为 Node.js 的计算性能没那么强。<br>
* n 也不可小于 0，否则抛错，因为没意义。<br>
* n 不为数字时，抛错。<br>
</div>

根据上述要求，更新 main.test.js 如下：
```js

    var main = require('../main');
    var should = require('should');
    
    describe('test/main.test.js', function () {
      it('should equal 0 when n === 0', function () {
    main.fibonacci(0).should.equal(0);
      });
    
      it('should equal 1 when n === 1', function () {
    main.fibonacci(1).should.equal(1);
      });
    
      it('should equal 55 when n === 10', function () {
    main.fibonacci(10).should.equal(55);
      });
    
      it('should throw when n > 10', function () {
    (function () {
      main.fibonacci(11);
    }).should.throw('n should <= 10');
      });
    
      it('should throw when n < 0', function () {
    (function () {
      main.fibonacci(-1);
    }).should.throw('n should >= 0');
      });
    
      it('should throw when n isnt Number', function () {
    (function () {
      main.fibonacci('呵呵');
    }).should.throw('n should be a Number');
      });
    });
```

这个时候再跑一下 $ mocha ,发现有三个 case 没过

![](http://i.imgur.com/dDcNcBk.png)

这个时候再更新 main.js 中的 fibonacci 函数，如下所示:
```js

    var fibonacci = function (n) {
      if (typeof n !== 'number') {
    throw new Error('n should be a Number');
      }
      if (n < 0) {
    throw new Error('n should >= 0')
      }
      if (n > 10) {
    throw new Error('n should <= 10');
      }
      if (n === 0) {
    return 0;
      }
      if (n === 1) {
    return 1;
      }
    
      return fibonacci(n-1) + fibonacci(n-2);
    };
```

再运行一次 $ mocha ，则全部通过，这就是一种测试驱动开发。

![](http://i.imgur.com/FAYSJFz.png)




