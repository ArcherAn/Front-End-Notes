ayout:

- post

title:

- 使用 async 控制并发

categories:

- Node.js基础学习

tags: 

- Node.js

- express

- 爬虫

- async

---
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=830 height=86 
	src="http://music.163.com/outchain/player?type=2&id=1791490&auto=0&height=66">
</iframe>
**目标：**
新建一个 lesson5 项目，在其中编写代码。
代码的入口是 app.js，当调用 node app.js 时，它会输出 CNode(https://cnodejs.org/ ) 社区首页的所有主题的标题，链接和第一条评论，以 json 的格式。

注意：与之前不同的是，并发连接数需要控制在 5 个。

**知识点：**
1. 学习 <span style="color:red;background-color:rgb(249,242,223)">`async(https://github.com/caolan/async )`</span>的使用。<br>
2. 学习使用 async 来控制并发连接数。

<!--more-->
在上一节中的eventproxy控制并发中，代码并不完美，因为我们一次性发出了40个并发请求出去，要知道，除去CNode之外，很多网站可能会因为发出的并发数连接太多而当成是恶意请求，会将IP封掉。<br>

在写爬虫的程序时，如果有1000个链接要去爬，那么不可能同时发出1000个并发链接出去，需要控制一下并发的数量，比如并发10个就好，然后慢慢爬完这1000个链接，用async来做这件事就很简单。

但是有一个问题是什么时候使用eventproxy，什么时候使用async？它们都是用来做异步流程控制的<br>

一般来说当你需要去多个源(一般是小于 10 个)汇总数据的时候，用 eventproxy 方便；当你需要用到队列，需要控制并发数，或者你喜欢函数式编程思维时，使用 async。大部分场景是前者，所以我个人大部分时间是用 eventproxy 的。

首先，我们伪造一个 <span style="color:red;background-color:rgb(249,242,223)">`fetchUrl(url, callback)`</span>函数，这个函数的作用就是，当你通过
```js

    fetchUrl('http://www.baidu.com', function (err, content) {
      // do something with `content`
    });
```

调用它时，它会返回<span style="color:red;background-color:rgb(249,242,223)">`http://www.baidu.com`</span>的页面内容回来。<br>
当然，我们这里的返回内容是假的，返回延时是随机的。并且在它被调用时，会告诉你它现在一共被多少个地方并发调用着。
```js
    
    // 并发连接数的计数器
    var concurrencyCount = 0;
    var fetchUrl = function (url, callback) {
      // delay 的值在 2000 以内，是个随机的整数
      var delay = parseInt((Math.random() * 10000000) % 2000, 10);
      concurrencyCount++;
      console.log('现在的并发数是', concurrencyCount, '，正在抓取的是', url, '，耗时' + delay + '毫秒');
      setTimeout(function () {
    concurrencyCount--;
    callback(null, url + ' html content');
      }, delay);
    };
```

我们接着来伪造一组链接
```js

    var urls = [];
    for(var i = 0; i < 30; i++) {
      urls.push('http://datasource_' + i);
    }
    console.log(urls);
```

生成的链接如下图所示：

![](http://i.imgur.com/8984jFu.png)
    
接着，我们使用 <span style="color:red;background-color:rgb(249,242,223)">`async.mapLimit`</span>来并发抓取，并获取结果。
```js

    async.mapLimit(urls, 5, function (url, callback) {
      fetchUrl(url, callback);
    }, function (err, result) {
      console.log('final:');
      console.log(result);
    });
```

运行输出是这样的：

![](http://i.imgur.com/VX7qT2V.png)

可以看到，一开始，并发连接数是从1开始增长的，增长到5时，就不再增加。当其中任务完成时，再继续抓取。并发连接数控制在5个。

<span style="color:red">**并发连接数解释**(**Simultaneous Browser Connections**)：</span><br>
并发连接数指的是客户端向服务器发起请求，并建立了TCP连接。每秒钟服务器链接的总TCP数量，就是并发连接数。

完整代码：
```js

    var async = require('async');
    
    var concurrencyCount = 0;
    var fetchUrl = function (url, callback) {
      var delay = parseInt((Math.random() * 10000000) % 2000, 10);
      concurrencyCount++;
      console.log('现在的并发数是', concurrencyCount, '，正在抓取的是', url, '，耗时' + delay + '毫秒');
      setTimeout(function () {
    concurrencyCount--;
    callback(null, url + ' html content');
      }, delay);
    };
    
    var urls = [];
    for(var i = 0; i < 30; i++) {
      urls.push('http://datasource_' + i);
    }
    
    async.mapLimit(urls, 5, function (url, callback) {
      fetchUrl(url, callback);
    }, function (err, result) {
      console.log('final:');
      console.log(result);
    });
```


