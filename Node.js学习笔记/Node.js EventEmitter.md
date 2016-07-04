layout:

- post

title:

- Node.js EventEmitter

categories:

- Node.js学习笔记

tags: 

- Node.js

- JavaScript
 
---
## **Node.js EventEmitter** ##
**前言：**Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。
Node.js里面的许多对象都会分发事件：一个net.Server对象会在每次有新连接时分发一个事件， 一个fs.readStream对象会在文件被打开的时候发出一个事件。 所有这些产生事件的对象都是events.EventEmitter 的实例。

### **EventEmitter 类** ###
events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。
可以通过require("events");来访问该模块。
<!--more-->

示例：
```js
    // 引入 events 模块
    var events = require('events');
    // 创建 eventEmitter 对象
    var eventEmitter = new events.EventEmitter();
    eventEmitter.on('some_event', function() { 
    	console.log('some_event 事件触发'); 
    }); 
    setTimeout(function() { 
    	event.emit('some_event'); 
    }, 1000); 
```
运行这段代码，1 秒后控制台输出了**some_event 事件触发**。

**将自定义事件添加到JavaScript对象**

有时，我们想着能够直接将事件添加到JavaScript对象中，要做到这一点，就需要通过在对象实例中调用events.EventEmitter.**call**(this)来在对象中继承EventEmitter功能，还需要将events.EventEmitter.**prototype**添加到对象的原型中去。如下：
```js
    function myobj(){
    events.EventEmitter.call(this);
    }
    myobj.prototype.__proto__ = events.EventEmitter.prototype;
```
然后，就可以直接从对象实例中发出事件：
```js
    var myobj = new myobj();
    myobj.emit("someEvent");
```
**把事件监听器添加到对象**
- **.addListener(eventName,callback):**将回调函数附加到对象的监听器中。每当eventName事件被触发时，回调函数就被放置在事件队列中执行
- **.on(eventName,callback):**同 .addListener（）
- **.once(eventName,callback):**只有eventName事件第一次被触发时，回调函数才能被放置在事件队列中执行。（即 监听器最多只会触发一次，触发后立刻解除该监听器）

**从对象中删除监听器**
监听器在Node.js非常有用，也是Node.js编程的重要组成部分。然而，它们的开销也很巨大，因此只在必要的时候使用。
- **.listeners(eventName):**返回一个连接到eventName事件的监听器函数的数组
- **.setMaxListeners(n):**如果多余n的监听器都加入到EventEmitter对象，就触发警报，它默认值是10。
- **.removeListener(eventName,callback):**将callback函数从EventEmitter对象的eventName事件中删除。
- **removeAllListeners([event]):**移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。

**实现事件监听器和发射器事件**
实例：
```js
    var events = require('events');
    function Account(){
    	this.balance = 0;
    	events.EventEmitter.call(this);
    	this.deposit = function(amount){
    		this.balance += amount;
    		this.emit('balance');
    	};
    this.withdraw = function(amount){
    this.balance -= amount;
    		this.emit('balance');
    }; 
    }
    Account.prototype.__proto__ = events.EventEmitter.prototype;
    function display(){
    	console.log("Account balance: $%d",this.balance);
    }
    
    function checkOverdraw(){
    if(this.balance<0){
     console.log("Account overdrawn");
    }
    }
    
    function checkGoal(acc,goal){
    if(acc.balance>goal){
    console.log("Goal Achieved");
    }
    }
    
    
    var account = new Account();
    account.on("balance",display);
    account.on("balance",checkOverdraw);
    account.on("balance",function(){
    	checkGoal(this,1000)
    });
    
    account.deposit(220);
    account.deposit(320);
    account.deposit(600);
    account.withdraw(1200);
```
控制台输出如下图所示：
![](http://i.imgur.com/VrA2KUO.png)





