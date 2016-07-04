ayout:

- post

title:

- 使用 superagent 与 cheerio 完成简单爬虫

categories:

- Node.js基础学习

tags: 

- Node.js

- express

---
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=830 height=86 
	src="http://music.163.com/outchain/player?type=2&id=33887932&auto=0&height=66">
</iframe>

**目标：**
建立一个 lesson3 项目，在其中编写代码。
当在浏览器中访问 http://localhost:3000/ 时，输出 CNode(https://cnodejs.org/ ) 社区首页的所有帖子标题和链接，以 <span style="color:red;background-color:rgb(249,242,223)">`json`</span>的形式。

**知识点：**<br>
1. 学习使用 <span style="color:red;background-color:rgb(249,242,223)">`superagent` </span>抓取网页<br>
2. 学习使用 <span style="color:red;background-color:rgb(249,242,223)">`cheerio` </span>分析网页

<!--more-->

Node.js中异步的场景运用的很多，其中爬虫的场景就比较适合，可以通过异步并发的方式爬取网站的一些相关信息。
此次主要使用了三个包依赖，分别是express、superagent和cheerio。<br>
<span style="color:red;background-color:rgb(249,242,223)">`superagent`</span>(http://visionmedia.github.io/superagent/ ) 是个 http 方面的库，可以发起 get 或 post 请求。<br>
<span style="color:red;background-color:rgb(249,242,223)">`cheerio`</span>(https://github.com/cheeriojs/cheerio ) 大家可以理解成一个 Node.js 版的 jquery，用来从网页中以 css selector 取数据，使用方式跟 jQuery 一样一样的。

**步骤：**<br>
1. 新建文件，然后 npm init;<br>
2. 安装包依赖  npm install --save package_name；<br>
3. 写应用逻辑。

app.js中的代码如下：<br>

```js
    var express = require('express');
    var superagent = require('superagent');
    var cheerio = require('cheerio');

    var app = express(); 

    app.get('/', function (req, res, next) {
      // 用 superagent 去抓取 https://cnodejs.org/ 的内容
      superagent.get('https://cnodejs.org/')
      .end(function (err, sres) {
      // 常规的错误处理
      if (err) {
    return next(err);
      }
       // sres.text 里面存储着网页的 html 内容，将它传给 cheerio.load 之后
      // 就可以得到一个实现了 jquery 接口的变量，我们习惯性地将它命名为 `$`
      // 剩下就都是 jquery 的内容了
      var $ = cheerio.load(sres.text);
      var items = [];
      $('#topic_list .topic_title').each(function (idx, element) {
       var $element = $(element);
      items.push({
      title: $element.attr('title'),
    		  href: $element.attr('href')
    		  });
    		 });
    			
    			 res.send(items);
    		});
    	});

     app.listen(3000, function (req, res) {
	  console.log('app is running at port 3000');
	 });
```

结果如下图所示：

![](http://i.imgur.com/hKxaG4p.png)