layout:

- post

title:

- Node.js事件模型

categories:

- Node.js学习笔记

tags: 

- Node.js

- JavaScript
 
---
## **Node.js事件模型** ##
**前言：**Node.js应用程序在一个单线程的事件驱动模型中运行。虽然Node.js在后台实现了一个线程池来工作，但应用程序本身不具备多线程的任何概念。
### **Node.js回调函数** ###
**Node.js异步编程**的直接体现就是**回调**。异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。回调函数在完成任务后就会被调用，Node 使用了大量的回调函数，Node 所有 API 都支持回调函数。
**例如**，我们可以一边读取文件，一边执行其他命令，在文件读取完成后，我们将文件内容作为回调函数的参数返回。这样在执行代码时就没有阻塞或等待文件 I/O 操作。这就大大提高了 Node.js 的性能，可以处理大量的并发请求。
<!--more-->

**阻塞代码实例：**
创建一个文件index.txt，内容如下：
   `这是一段测试阻塞/非阻塞实例的段落`

创建main.js文件，如下：
```js
	ar fs = require("fs");
	var data = fs.readFileSync('index.txt');
	console.log(data.toString());
	console.log("程序执行结束!");
```
以上代码执行结果如下：
`$ node main.js`
`这是一段测试阻塞/非阻塞实例的段落`
`程序执行结束!`

**非阻塞代码实例：**
```js
创建main.js文件，如下：
var fs = require("fs");
fs.readFile('index.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});
console.log("程序执行结束!");
```
以上代码执行结果如下：
`$ node main.js`
`程序执行结束!`
`这是一段测试阻塞/非阻塞实例的段落`

以上两个实例了解了阻塞与非阻塞调用的不同。第一个实例在文件读取完后才执行完程序。 第二个实例不需要等待文件读取完，这样就可以在读取文件时同时执行接下来的代码，大大提高了程序的性能。

### **线程模型** ###
如下图显示了处理GetFile和GetData两个请求的线程模型。GetFile请求打开文件，读取内同，然后再一个响应中将数据发回。所有这些在相同的线程中按顺序发生。GetData请求连接数据库，查询所需的数据，然后在响应中将数据发送出去。
线程式模型为每一个Web请求开启一个线程处理。线程式又包括单线程模型、多线程模型，也就是一个进程中几个个线程，像Apache就可以选择Worker MPM(多进程多线程)还是Prefork MPM(多进程单线程)。多线程模型处理并发的方式是将每一个IO操作分配到单独的线程中。当客户端发出请求给服务器，服务器会对请求处理并准备好响应回传给客户端，服务器通过维持一个有限的线程池来执行能分离开的处理任务。
![](http://i.imgur.com/i8PJgM8.png)


### **Node.js事件驱动** ###
思考Node.js事件模型的工作原理。Node.js不是在各个线程为每个请求执行所有的工作。反之，它把工作添加到一个事件队列中，然后有一个单独的线程运行一个事件循环把这个工作提取出来。事件循环抓取事件队列中最上面的条目，并执行它，然后抓取下一个条目。当执行长期运行或有阻塞I/O的代码时，它不是直接调用该函数，而是把函数随同一个要在次函数完成后执行的回调一起添加到事件队列中。当Node.js事件队列中的所有事件都被执行完成时，Node.js应用程序停止。

![](http://i.imgur.com/UtIYV2V.png)

上图中显示了Node.js如何处理GetFile和GetData两个请求。它将GetFile和GetData请求添加到事件队列中。每个线程不一定要遵循直接交错顺序。例如：连接（Connect）请求比读（Read）请求需要更长的时间来完成，所以Send（file）在Query(db)之前调用。

![](http://i.imgur.com/DHLkrYj.png)

上图显示了完整的Node.js事件模型，包括了事件队列，事件循环和线程池。**注意：**事件循环要么在事件循环线程本身上执行功能，要么在一个单独的线程上执行功能，对于阻塞I/O，则采取后一种方式。
node基于事件的工作调度能很自然地将主要的调度工作限制到了一个线程，应用能很高效地处理多任务。程序每一时刻也只需管理一个工作中的任务。当必须处理堵塞IO时，通过将这个部分的IO控制权交给池中的线程，能最小地影响到应用处理事件，快速地反应web请求。 当然对机器方便的事情对于写代码的人来说就需要更小心地划分业务逻辑，我们需要将工作划分为合理大小的任务来适配事件模型这一套机制。

### **Node.js事件循环** ###
- Node.js 是单进程单线程应用程序，但是通过事件和回调支持并发，所以性能非常高。
- Node.js 的每一个 API 都是异步的，并作为一个独立线程运行，使用异步函数调用，并处理并发。
- Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。
- Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数。

**事件驱动程序**
Node.js 使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。
当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。
这个模型非常高效可扩展性非常强，因为webserver一直接受请求而不等待任何读写操作。（这也被称之为非阻塞式IO或者事件驱动IO）在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。
![](http://i.imgur.com/LWeOQ6L.jpg)
整个事件驱动的流程就是这么实现的。

实例:
```js
    // 引入 events 模块
    var events = require('events');
    // 创建 eventEmitter 对象
    var eventEmitter = new events.EventEmitter();
    
    // 创建事件处理程序
    var connectHandler = function connected() {
       console.log('连接成功。');
      
       // 触发 data_received 事件 
       eventEmitter.emit('data_received');
    }
    
    // 绑定 connection 事件处理程序
    eventEmitter.on('connection', connectHandler);
     
    // 使用匿名函数绑定 data_received 事件
    eventEmitter.on('data_received', function(){
       console.log('数据接收成功。');
    });
    
    // 触发 connection 事件 
    eventEmitter.emit('connection');
    
    console.log("程序执行完毕。");
```