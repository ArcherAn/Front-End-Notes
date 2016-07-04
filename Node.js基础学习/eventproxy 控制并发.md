ayout:

- post

title:

- 使用 eventproxy 控制并发

categories:

- Node.js基础学习

tags: 

- Node.js

- express

- 爬虫

- eventproxy

---
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=830 height=86 
	src="http://music.163.com/outchain/player?type=2&id=3950050&auto=0&height=66">
</iframe>
**目标：**
建立一个lesson4项目，输出CNode(https://cnodejs.org/ ) 社区首页的所有主题的标题，链接和第一条评论，以 <span style="color:red;background-color:rgb(249,242,223)">`json`
</span>的格式。

**知识点：**
1. 体会Node.js的<span style="color:red;background-color:rgb(249,242,223)">`callback hell`</span>之美<br>
2. 学习使用<span style="color:red;background-color:rgb(249,242,223)">`eventproxy`</span>控制并发
<!--more-->

在lesson3中我们介绍了如何使用 superagent 和 cheerio 来取主页内容，那只需要发起一次 http get 请求就能办到。但这次，我们需要取出每个主题的第一条评论，这就要求我们对每个主题的链接发起请求，并用 cheerio 去取出其中的第一条评论。

CNode 目前每一页有 40 个主题，于是我们就需要发起 1 + 40 个请求，来达到我们的目标。<br>
首先了解下同步异步的概念:<br>

**同步：**操作IO时，IO没有返回，你就在那等着吧，所以同步的程序象apache，就要开很多的进程，专门存放存放这此同步的操作，关键是一整套的操作全在那里挂着耗内存和别的资源。(形象的例子：吃饭和说话，只能一件事一件事的来，因为只有一张嘴。)<br>

**异步：**发送IO操作请求后，你可以去做别的事，IO返回后会自动做下一步的，而且你可以同时发起多个IO然后一起汇总。所以只用单线程就可以了，每一步的操作可以用整体更少的资源满足要求。（形象的例子：吃饭和听音乐是异步的，因为，听音乐并不引响我们吃饭。）<br>

后者的 40 个请求，我们并发地发起，而且不会遇到多线程、锁什么的，Node.js 的并发模型跟多线程不同，Node.js 可以使用单线程实现这些功能。

这次用到的库为:<span style="color:red;background-color:rgb(249,242,223)">`superagent`</span>、<span style="color:red;background-color:rgb(249,242,223)">`cheerio`</span>、<span style="color:red;background-color:rgb(249,242,223)">`eventproxy`</span>

省略前面的步骤，新建app.js文件，输入以下代码:
```js

    // 引入依赖
    var eventproxy = require('eventproxy');
    var superagent = require('superagent');
    var cheerio = require('cheerio');
    
    // url 模块是 Node.js 标准库里面的
    // http://nodejs.org/api/url.html
    var url = require('url');
    
    var cnodeUrl = 'https://cnodejs.org/';
    
    superagent.get(cnodeUrl)
      .end(function (err, res) {
    if (err) {
      return console.error(err);
    }
    var topicUrls = [];
    var $ = cheerio.load(res.text);
    	// 获取首页所有的链接
    $('#topic_list .topic_title').each(function (idx, element) {
      var $element = $(element);
      // $element.attr('href') 本来的样子是 /topic/542acd7d5d28233425538b04
      // 我们用 url.resolve 来自动推断出完整 url，变成
      // https://cnodejs.org/topic/542acd7d5d28233425538b04 的形式
      // 具体请看 http://nodejs.org/api/url.html#url_url_resolve_from_to 的示例
      var href = url.resolve(cnodeUrl, $element.attr('href'));
      topicUrls.push(href);
    });
    	console.log(topicUrls);
      });
```

运行 <span style="color:red;background-color:rgb(249,242,223)">`node app.js`</span>

输出如下图所示：

![](http://i.imgur.com/LtlYSR2.png)

这时候我们已经得到首页所有主题的完整url地址了，接下来，我们把这些地址都抓取一遍，获得第一条评论就可以了。在抓取之前，介绍一下eventproxy库。<br>
这时候我们已经得到所有 url 的地址了，接下来，我们把这些地址都抓取一遍，就完成了，Node.js 就是这么简单。先定义一个 var count = 0，然后每次抓取成功以后，就 count++。如果你是要抓取三个源的数据，由于你根本不知道这些异步操作到底谁先完成，那么每次当抓取成功的时候，就判断一下 count == 3。当值为真时，使用另一个函数继续完成操作。<br>
而 eventproxy 就起到了这个计数器的作用，它来帮你管理到底这些异步操作是否完成，完成之后，它会自动调用你提供的处理函数，并将抓取到的数据当参数传过来。<br>

假设我们不使用 eventproxy 也不使用计数器时，抓取三个源的写法是这样的：
```js

    // 参考 jquery 的 $.get 的方法
    $.get("http://data1_source", function (data1) {
      // something
      $.get("http://data2_source", function (data2) {
    // something
    $.get("http://data3_source", function (data3) {
      // something
      var html = fuck(data1, data2, data3); //fuck()表进程
      render(html);
    });
      });
    });
```<br>
先获取 data1，获取完成之后获取 data2，然后再获取 data3，然后 fuck 它们，进行输出。<br>

但是这三个源的数据，可以并行去获取的，data2 的获取并不依赖 data1 的完成，data3 同理也不依赖 data2。所以可以采用计数器的形式来写：
```js

    (function () {
    var count = 0;
    var result = {};

    $.get('http://data1_source', function (data) {
    result.data1 = data;
    count++;
    handle();
    });
      $.get('http://data2_source', function (data) {
    result.data2 = data;
    count++;
    handle();
    });
      $.get('http://data3_source', function (data) {
    result.data3 = data;
    count++;
    handle();
    });
      function handle() {
    if (count === 3) {
      var html = fuck(result.data1, result.data2, result.data3);
      render(html);
    }
      }
    })();
```

如果使用eventproxy，应该是这样的：
```js

    var ep = new eventproxy();
    ep.all('data1_event', 'data2_event', 'data3_event', function (data1, data2, data3) {
      var html = fuck(data1, data2, data3);
      render(html);
    });
    
    $.get('http://data1_source', function (data) {
      ep.emit('data1_event', data);
      });
    
    $.get('http://data2_source', function (data) {
      ep.emit('data2_event', data);
      });
    
    $.get('http://data3_source', function (data) {
      ep.emit('data3_event', data);
      });
```

<span style="color:red;background-color:rgb(249,242,223)">`ep.all('data1_event', 'data2_event', 'data3_event', function (data1, data2, data3) {});`</span><br>
这一句监听了三个事件，分别是<span style="color:red;background-color:rgb(249,242,223)">`data1_event, data2_event, data3_event`</span>,每次当一个源的数据抓取完成时，就通过 ep.emit() 来告诉 ep 自己，某某事件已经完成了。

当三个事件未同时完成时，<span style="color:red;background-color:rgb(249,242,223)">`ep.emit()`</span> 调用之后不会做任何事；当三个事件都完成的时候，就会调用末尾的那个回调函数，来对它们进行统一处理。<br>
eventproxy 提供了不少其他场景所需的 API，但最最常用的用法就是以上的这种，即：<br>
1. 先 <span style="color:red;background-color:rgb(249,242,223)">`var ep = new eventproxy()`</span>; 得到一个 eventproxy 实例。<br>
2. 告诉它要监听哪些事件，并给一个回调函数。<span style="color:red;background-color:rgb(249,242,223)">`ep.all('event1', 'event2', function (result1, result2) {})`</span>。<br>
3. 在适当的时候 <span style="color:red;background-color:rgb(249,242,223)">`ep.emit('event_name', eventData)`</span>。<br>

回到正题，之前我们已经得到了一个长度为 40 的 <span style="color:red;background-color:rgb(249,242,223)">`topicUrls`</span>数组，里面包含了每条主题的链接。那么意味着，我们接下来要发出 40 个并发请求。我们需要用到 eventproxy 的<span style="color:red;background-color:rgb(249,242,223)">`#after`</span>API。

完整的代码如下所示：
```js

    // 引入依赖
    var eventproxy = require('eventproxy');
    var superagent = require('superagent');
    var cheerio = require('cheerio');
    
    // url 模块是 Node.js 标准库里面的
    // http://nodejs.org/api/url.html
    var url = require('url');
    
    var cnodeUrl = 'https://cnodejs.org/';
    
    superagent.get(cnodeUrl)
      .end(function (err, res) {
    if (err) {
      return console.error(err);
    }
    var topicUrls = [];
    var $ = cheerio.load(res.text);
    	// 获取首页所有的链接
    $('#topic_list .topic_title').each(function (idx, element) {
      var $element = $(element);
      // $element.attr('href') 本来的样子是 /topic/542acd7d5d28233425538b04
      // 我们用 url.resolve 来自动推断出完整 url，变成
      // https://cnodejs.org/topic/542acd7d5d28233425538b04 的形式
      // 具体请看 http://nodejs.org/api/url.html#url_url_resolve_from_to 的示例
      var href = url.resolve(cnodeUrl, $element.attr('href'));
      topicUrls.push(href);
    });
    	var ep = new eventproxy();
    // 命令 ep 重复监听 topicUrls.length 次（在这里也就是 40 次） `topic_html` 事件再行动
    ep.after('topic_html', topicUrls.length, function (topics) {
    	  // topics 是个数组，包含了 40 次 ep.emit('topic_html', pair) 中的那 40 个 pair
     	
      topics = topics.map(function (topicPair) {
    var topicUrl = topicPair[0];
    var topicHtml = topicPair[1];
    var $ = cheerio.load(topicHtml);
    return ({
      title: $('.topic_full_title').text().trim(),
      href: topicUrl,
      author: $('.changes a').text().trim(),
      comment1: $('.reply_content').eq(0).text().trim(),
    });
      });
    
      console.log('final:');
      console.log(topics);
    });
    
    topicUrls.forEach(function (topicUrl) {
      superagent.get(topicUrl)
    .end(function (err, res) {
      console.log('fetch ' + topicUrl + ' successful');
      ep.emit('topic_html', [topicUrl, res.text]);
    });
    });
    });
```

输出如下图所示：
![](http://i.imgur.com/VU587E8.jpg)



