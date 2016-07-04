layout:

- post

title:

- Node.js简介

categories:

- Node.js学习笔记

tags: 

- Node.js

- JavaScript
 
---
## **Node.js简介** ##
**前言：**
Node.js之所以是一个出色的框架，主要是基于以下几个原因：
- **JavaScript端到端**：Node.js最大的一个优点在于，它可以让你用JavaScript同时编写服务器端和客户端脚本。在决定是把逻辑放入客户端脚本还是服务器端脚本方面一直有困难。利用Node.js，你可以在客户端上编写JavaScript，并轻松地在服务器上适应它，反之亦然。另一个好处是，客户端的开发者和服务器的开发者使用同一种语言。
- **事件驱动的可扩展性**：Node.js应用独特的逻辑来处理Web请求。使用Node.js，不是让多个线程等待处理Web请求，而是采用一种基本的事件模型在同一个线程上处理它们。这使得Node.js Web服务器可以用传统的Web服务器不能做到的方式进行扩展。

<!--more-->
- **可扩展性**：Node.js有很多的追随者和活跃的开发社区。人们正在不断地提供新的模板来扩展Node.js的功能。此外，在Node.js中安装和包含新的模块是非常简单的，可以在几分钟扩展Node.js的项目来包含新的功能。
- **快速执行**：建立Node.js，并在其中开发是超级容易的。在短短的几分钟内就可以安装Node.js，并拥有一个能工作的Web服务器。

### **Node.js创建应用** ###
首先了解**Node.js**是由哪几部分组成:、
**1.** **引入 required 模块：**我们可以使用 require 指令来载入 Node.js 模块
**2.** **创建服务器：**服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器
**3. ** **接收请求与响应请求：**服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据

实例：
```js
var http = require('http');  //引入required模块，实例化HTTP变量赋值给变量http

http.createServer(function (request, response) {

	// 发送 HTTP 头部 
	// HTTP 状态值: 200 : OK
	// 内容类型: text/plain
	response.writeHead(200, {'Content-Type': 'text/plain'});

	// 发送响应数据 "Hello World"
	response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```
使用node命令执行上述代码：

`node server.js`
`Server running at http://127.0.0.1:8888/`

分析Node.js 的 HTTP 服务器：
- 第一行请求（require）Node.js 自带的 http 模块，并且把它赋值给 http 变量。
- 接下来调用 http 模块提供的函数： createServer 。这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。

### **NPM使用介绍** ###
**NPM**是随同**Node.js**一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：
- 允许用户从NPM服务器下载别人编写的第三方包到本地使用
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用

#### **使用 npm 命令安装模块** ####
npm 安装 Node.js 模块语法格式如下：

`$ npm install <Module Name>`
以下实例，我们使用 npm 命令安装常用的 Node.js web框架模块 **express**
`$ npm install express`  
安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。
`var express = require('express');`

安装过程输出如下内容，第一行输出了模块的版本号及安装位置，如下所示：
![](http://i.imgur.com/Yzedv4G.png)

#### **使用 package.json** ####
package.json 位于模块的目录下，用于定义包的属性。看下 express 包的 package.json 文件，位于 node_modules/express/package.json 内容：
```json
    {
      "name": "express",
      "description": "Fast, unopinionated, minimalist web framework",
      "version": "4.13.4",
      "author": {
    "name": "TJ Holowaychuk",
    "email": "tj@vision-media.ca"
      },
      "contributors": [
    {
      "name": "Aaron Heckmann",
      "email": "aaron.heckmann+github@gmail.com"
    },
    {
      "name": "Ciaran Jessup",
      "email": "ciaranj@gmail.com"
    },
    {
      "name": "Douglas Christopher Wilson",
      "email": "doug@somethingdoug.com"
    },
    {
      "name": "Guillermo Rauch",
      "email": "rauchg@gmail.com"
    },
    {
      "name": "Jonathan Ong",
      "email": "me@jongleberry.com"
    },
    {
      "name": "Roman Shtylman",
      "email": "shtylman+expressjs@gmail.com"
    },
    {
      "name": "Young Jae Sim",
      "email": "hanul@hanul.me"
    }
      ],
      "license": "MIT",
      "repository": {
    "type": "git",
    "url": "https://github.com/expressjs/express"
      },
      "keywords": [
    "express",
    "framework",
    "sinatra",
    "web",
    "rest",
    "restful",
    "router",
    "app",
    "api"
      ],
      "dependencies": {
    "accepts": "~1.2.12",
    "array-flatten": "1.1.1",
    "content-disposition": "0.5.1",
    "content-type": "~1.0.1",
    "cookie": "0.1.5",
    "cookie-signature": "1.0.6",
    "debug": "~2.2.0",
    "depd": "~1.1.0",
    "escape-html": "~1.0.3",
    "etag": "~1.7.0",
    "finalhandler": "0.4.1",
    "fresh": "0.3.0",
    "merge-descriptors": "1.0.1",
    "methods": "~1.1.2",
    "on-finished": "~2.3.0",
    "parseurl": "~1.3.1",
    "path-to-regexp": "0.1.7",
    "proxy-addr": "~1.0.10",
    "qs": "4.0.0",
    "range-parser": "~1.0.3",
    "send": "0.13.1",
    "serve-static": "~1.10.2",
    "type-is": "~1.6.6",
    "utils-merge": "1.0.0",
    "vary": "~1.0.1"
      },
      "devDependencies": {
    "after": "0.8.1",
    "ejs": "2.3.4",
    "istanbul": "0.4.2",
    "marked": "0.3.5",
    "mocha": "2.3.4",
    "should": "7.1.1",
    "supertest": "1.1.0",
    "body-parser": "~1.14.2",
    "connect-redis": "~2.4.1",
    "cookie-parser": "~1.4.1",
    "cookie-session": "~1.2.0",
    "express-session": "~1.13.0",
    "jade": "~1.11.0",
    "method-override": "~2.3.5",
    "morgan": "~1.6.1",
    "multiparty": "~4.1.2",
    "vhost": "~3.0.1"
      },
      "engines": {
    "node": ">= 0.10.0"
      },
      "files": [
    "LICENSE",
    "History.md",
    "Readme.md",
    "index.js",
    "lib/"
      ],
      "scripts": {
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "istanbul cover node_modules/mocha/bin/_mocha --report lcovonly -- --require test/support/env --reporter spec --check-leaks test/ test/acceptance/",
    "test-cov": "istanbul cover node_modules/mocha/bin/_mocha -- --require test/support/env --reporter dot --check-leaks test/ test/acceptance/",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
      },
      "gitHead": "193bed2649c55c1fd362e46cd4702c773f3e7434",
      "bugs": {
    "url": "https://github.com/expressjs/express/issues"
      },
      "homepage": "https://github.com/expressjs/express",
      "_id": "express@4.13.4",
      "_shasum": "3c0b76f3c77590c8345739061ec0bd3ba067ec24",
      "_from": "express@latest",
      "_npmVersion": "1.4.28",
      "_npmUser": {
    "name": "dougwilson",
    "email": "doug@somethingdoug.com"
      },
      "maintainers": [
    {
      "name": "dougwilson",
      "email": "doug@somethingdoug.com"
    }
      ],
      "dist": {
    "shasum": "3c0b76f3c77590c8345739061ec0bd3ba067ec24",
    "tarball": "https://registry.npmjs.org/express/-/express-4.13.4.tgz"
      },
      "directories": {},
      "_resolved": "https://registry.npmjs.org/express/-/express-4.13.4.tgz"
    }
```
#### **Package.json 属性说明** ####
- **name** - 包名
- **version** - 包的版本号
- **description** - 包的描述
- **homepage** - 包的官网 url
- **author** - 包的作者姓名
- **contributors** - 包的其他贡献者姓名
- **dependencies** - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下
- **repository** - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上
- **main** - main 字段是一个模块ID，它是一个指向你程序的主要项目。就是说，如果你包的名字叫 express，然后用户安装它，然后require("express")
- **keywords** - 关键字

#### **NPM常用命令** ####
| **选项** | **说明**  | **示例** | 
| :--------: | :--------:| :--------:|
| **search**| 在存储库中查找模块包|npm search express
| **install**| 使用在存储库或本地位置的一个package.json文件来安装包| npm install express
| **install -g**|在全局可访问的位置安装一个包| npm install express -g
| **remove**|删除一个模块 | npm remove express
| **pack**|把在一个package.json文件中定义的模块封装成.tgz文件 | npm pack
| **view**| 显示模块的详细信息| npm view express
| **publish**| 把在一个package.json文件中定义的模块发布到注册表|npm publish
| **unpublish**| 取消发布已发布的一个模块|npm unpublish myModule
| **owner**| 允许在存储库中添加、删除包和列出包的所有者|npm add bdayley myModule
    