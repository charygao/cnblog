nodeJS 菜鸟入门
====


#### 从一个简单的 HTTP 服务开始旅程……

创建一个 <code>server.js</code> 文件，写入：

    //最简单的 http 服务例子
    var http = require("http");
    http.createServer(function(request, response) {
        response.writeHead(200, {"Content-Type": "text/html"});
        response.write("<h1>Hi NodeJs</h1>");
        response.end();
    }).listen(8080);
    console.log("成功的提示：httpd start @8080");

打开 <code>http://localhost:8080/</code> 你会看到惊喜~

> tips
>
> 执行： <code>node server.js</code>启动服务。
> 按 <kbd>Ctrl + c</kbd> 结束 刚刚创建的服务。

#### 分析该HTTP服务

1. http服务器： Node.JS 自带的， **http** 模块
2. createServer： 调用该返回的对象中的 listen 方法，对服务端口进行监听
3. 复习下 *匿名函数* 

###### 匿名函数的变化

    //自带的http 模块
    var http = require("http");

    function onRequest(request, response) {
      //console.log("请求来了，事件响应");
      response.writeHead(200, {"Content-Type": "text/plain"});
      response.write("<h1>Hi NodeJs</h1>");
      response.end();
    }

    http.createServer(onRequest).listen(8080);
    console.log("成功的提示：httpd start @8080");

###### 扩展 **事件驱动**

Felix Geisendörfer 的 [ Understanding node.js (理解NodeJS) ][1]
 * php： 任何时候当有请求进入的时候，网页服务器（通常是Apache）就为这一请求新建一个进程，并且开始从头到尾执行相应的PHP脚本；
 * Node.js： 事件驱动设计
 * 证明 NodeJS 的事件驱动设计： 去掉以上代码 这个注释
 <code>//console.log("请求来了，事件响应");</code> 启动 server.js
 * 结果： 启动服务时，输出 *成功……* 执行网页请求时，输出 *请求……* 
 * 一次http请求输出俩次事件是因为：大部分服务器都会在你访问 http://localhost:8080 /时尝试读取 http://localhost:8080/favicon.ico 

#### 模块化

###### 把 server.js 变成一个模块: 

    var http = require("http");

    function start() {
      function onRequest(request, response) {
        //console.log("请求来了，事件响应");
        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("Hi NodeJS");
        response.end();
      }

      http.createServer(onRequest).listen(8080);
      console.log("成功的提示：httpd start @8080");
    }
    //nodejs中exports对象，理解 **module.exports** 和 **exports**
    exports.start = start;

> ##### 理解 module.exports 和 exports :
>
> **exports** 获取的所有的属性和方法，都会传递给 **Module.exports** 
> 但是 **Module.exports** 本身不具备任何属性和方法。如果， **Module.exports** 已经具备某些属性或方法，那么 **exports** 传递的属性或者方法会被忽略（失败）。
>
> ##### 代码举例

    // a.js
    exports.words = function() {
        console.log('Hi');
    };

    // b.js
    var say = require('./a.js');
    say.words(); // 'Hi'

    //----分割线----
    
    // aa.js
    module.exports = 'Wellcome';
    exports.words = function() {
        console.log('Hi');
    };

    // bb.js
    var say = require('./aa.js');
    say.words(); // TypeError: Object Wellcome has no method 'words'

>


###### 调用

创建 index.js 写入：

    var server = require("./server");

    server.start();

启动： <code>node index.js</code> 看看吧！

###### 牛刀小试 路由选择模块

作为 ThinkPHP 的玩家，肯定能想到 TP 的路由： 通过实例化对象来实现路由选择；
现在来看看 node 是怎么来实现的：

1 、提取出请求的URL以及GET/POST参数，这里需要额外的NodeJS模块：*URL* 、 *querystring*

<code>仔细看下图：</code>


                           url.parse(string).query
                                               |
               url.parse(string).pathname      |
                           |                   |
                           |                   |
                         -----   ------------------
    http://localhost:8080/start?foo=bar&hello=world
                                    ---       -----
                                     |          |
                                     |          |
                  querystring(string)["foo"]    |
                                                |
                             querystring(string)["hello"]



2 、帮助 *onRequest()函数* 找出浏览器请求的URL路径 

先新建一个 **router.js** 写入，能够输出当前请求的路径名称

    function route(pathname) {
        console.log("请求路径是：" + pathname);
    }
    exports.route = route;

再扩展 **server.js**

    var http = require("http"),
        //URL模块可以读取URL、分析诸如hostname、port之类的信息
        url  = require("url");

    //传入 route (回调函数)
    function start(route) {
        function onRequest(request, response) {
            var pathname = url.parse(request.url).pathname;
            //console.log("请求 "+pathname+" ，事件");

            route(pathname);

            response.writeHead(200, {"Content-Type": "text/plain"});
            response.write("Hi NodeJS");
            response.end();
        }

        http.createServer(onRequest).listen(8080);
        console.log("成功的提示：httpd start @8080");
    }

    exports.start = start;


最后扩展 **index.js**

    var server = require("./server");
    var router = require("./router");

    server.start(router.route);


启动，输入 <code>http://localhost:8080/a</code> 结果

    $ node index.js
    成功的提示：httpd start @8080
    请求路径是：/a
    请求路径是：/favicon.ico


> 扩展：
>
> Martin Fowlers [关于依赖注入的大作][2]


#### 函数编程 

注重：数学本质、抽象本质。
重要的概念：循环可以没有（描述如何解决问题），递归（描述这个问题的定义）是不可或缺；
面向对象编程是 传递对象；而在函数式编程中，传递的是函数(更专业的叫：叫做高阶函数)
高阶函数：a、接受一个或多个函数输入；b、输出一个函数

其他， 行为驱动执行 (BDD) 和 测试驱动开发(TDD)

> 函数编程扩展阅读：
>
> [函数式编程扫盲篇][4]
> [Steve Yegge 名词王国中的死刑][3]


#### 路由处理函数

示例，创建一个 <code>requestHandlers.js</code> 模块

    function start() {
        console.log("处理请求 'start' 开启.");
    }

    function upload() {
        console.log("处理请求 'upload' 开启.");
    }

    exports.start = start;
    exports.upload = upload;


###### 对象传递

将一系列请求处理程序通过一个对象来传递，并且需要使用松耦合的方式将这个对象注入到 *route()* 函数中


1、先将这个对象引入到主文件 **index.js** 中

    var server = require("./server"),
        router = require("./router"),
        requestHandlers = require("./requestHandlers");

    var handle = {};
        handle["/"] = requestHandlers.start;
        handle["/start"] = requestHandlers.start;
        handle["/upload"] = requestHandlers.upload;

    server.start(router.route, handle);

2、把额外的传递参数 *handle* 给服务器 **server.js**

    var http = require("http"),
        url  = require("url");

    function start(route, handle) {
        function onRequest(request, response) {
            var pathname = url.parse(request.url).pathname;
            console.log("请求 "+pathname+" 响应");

            route(handle, pathname);

            response.writeHead(200, {"Content-Type": "text/plain"});
            response.write("Hi NodeJS");
            response.end();
        }

        http.createServer(onRequest).listen(8080);
        console.log("成功的提示：httpd start @8080");
    }

    exports.start = start;

3、修改 **router.js**

    function route(handle, pathname) {
        console.log("route 请求路径：" + pathname);
        if (typeof handle[pathname] === 'function') {
            handle[pathname]();
        } else {
            console.log("找不到路径 " + pathname);
        }
    }

    exports.route = route;


4、运行结果： <code>http://localhost:8080/start</code>

    $ node index.js
    成功的提示：httpd start @8080
    请求 /start 响应
    route 请求路径：/start
    处理请求 'start' 开启.
    请求 /favicon.ico 响应
    route 请求路径：/favicon.ico
    找不到路径 /favicon.ico

#### 和浏览器的互动

浏览器需要对请求作出响应。

###### 不好的实现方式

1、将 **requestHandler.js** 修改为

    function start() {
        console.log("处理请求 'start' 开启.");
        return "Hello Start";
    }

    function upload() {
        console.log("处理请求 'upload' 开启.");
        return "Hello Upload";
    }

    exports.start = start;
    exports.upload = upload;

2、将 **router.js** 修改为

    function route(handle, pathname) {
        console.log("route 请求路径：" + pathname);
        if (typeof handle[pathname] === 'function') {
            return handle[pathname]();
        } else {
            console.log("找不到路径 " + pathname);
            return '404 Not Found'
        }
    }

    exports.route = route;


3、需要对 **server.js** 进行重构，以使得它能够将请求处理程序通过请求路由返回的内容 响应 给浏览器

    var http = require("http"),
        url  = require("url");

    function start(route, handle) {
        function onRequest(request, response) {
            var pathname = url.parse(request.url).pathname;
            console.log("请求 "+pathname+" 响应");
            response.writeHead(200, {"Content-Type": "text/plain"});

            var content = route(handle, pathname);

            response.write(content);
            response.end();
        }
        http.createServer(onRequest).listen(8080);
        console.log("成功的提示：httpd start @8080");
    }

    exports.start = start;

4、运行结果：当输入 /start 是浏览器显示 hello start ……

> 缺点:
>
> 当未来有请求处理程序需要进行 **非阻塞** 的操作的时候，我们的应用就“挂”了。

##### 阻塞与非阻塞

在请求处理程序中加入阻塞操作的用例 **requestHandlers.js**：

    function start() {
        console.log("处理请求 'start' 开启.");

        function sleep (milliSeconds) {
            var startTime = new Date().getTime();
            while (new Date().getTime() < startTime + milliSeconds);
        }

        sleep(10000);
        return "Hello Start";
    }

    function upload() {
        console.log("处理请求 'upload' 开启.");
        return "Hello Upload";
    }

    exports.start = start;
    exports.upload = upload;

这样，当 *start()* 被调用的时候，Node.js会先等待10秒，之后才会返回 “Hello Start” 。当调用 *upload()* 的时候，会和此前一样立即返回。

但是，当你打开两个浏览器窗口或者标签页，
第一个输入 http://localhost:8080/start 但是先不“回车”；
第二个输入 http://localhost:8080/upload 同样先不“回车” 
接下来在第一个窗口中 /start 按下回车，快速的切到第二个窗口再按下回车……
会看到： /start 花了10s加载url， upload 居然也是！
原因就是 start() 包含了阻塞操作。形象的说就是“它阻塞了所有其他的处理工作”。


> tips
>
> 1、Node.js是单线程的。它通过事件轮询（event loop）来实现并行操作
> 2、避免阻塞操作，多使用非阻塞操作————回调（ **callbackFunction()** ）


##### 一种错误的使用非阻塞操作的方式

修改 **requestHandlers.js** 中的 start 请求处理程序

    //child_process 可以创建多进程，利用多核计算资源。
    var exec = require("child_process").exec;

    function start() {
        console.log("处理请求 'start' 开启.");
        var content = "empty";

        exec("ls -lah", function (error, stdout, stderr) {
            content = stdout;
          });

        return content;
    }

    function upload() {
        console.log("处理请求 'upload' 开启.");
        return "Hello Upload";
    }

    exports.start = start;
    exports.upload = upload;


上述代码创建了一个新的变量content（初始值为“empty”），执行 “ls -lah” 命令，将结果赋值给 content ，最后将content返回。
接下来 重启服务 访问 http://localhost:8080/start 结果是 empty ，
这时，exec() 在非阻塞这块发挥了作用。它可以执行非常耗时的 shell 操作而无需网页应用等待该操作。

但是代码是同步执行的，这就意味着在调用 exec() 之后，Node.js会立即执行 <code>return content;</code> 在这个时候，content仍然是 “empty” ，因为传递给 exec() 的回调函数还未执行到————因为 exec() 的操作是异步的……


> tips:
> child_process 模块提供四个创建子进程的函数: spawn，exec，execFile和fork
> 其中 spawn 是最原始的创建子进程的函数，其他三个都是对 spawn 不同程度的封装。 spawn 只能运行指定的程序，参数需要在列表中给出，相当于 execvp 系统函数，而 exec 可以直接运行复杂的命令
> 例子：运行ls -lh /usr 

    //使用 spawn
    spawn('ls', ['-lh', '/usr'])
    //使用 exec
    exec('ls -lh /usr')

> exec 是启动了一个系统 shell 命令来解析参数，可以执行复杂的命令，包括管道和重定向。
> exec 还可以直接接受一个回调函数作为参数，回调函数有三个参数，分别是 err, stdout, stderr


##### 以非阻塞操作进行请求响应（正确的方式）

实现方案： 函数编程(函数传递)

目前的结果：通过应用各层之间传递值的方式（请求处理程序 -> 请求路由 -> 服务器）将请求处理程序返回的内容（请求处理程序最终要显示给用户的内容）传递给HTTP服务器。

新实现方式：将服务器“传递”给内容的方式。从实践角度来说，就是将response对象（从服务器的回调函数 onRequest() 获取）通过请求路由传递给请求处理程序。 随后，处理程序就可以采用该对象上的函数来对请求作出响应。

1、 **server.js**


    var http = require("http"),
        url  = require("url");

    function start(route, handle) {
        function onRequest(request, response) {
            var pathname = url.parse(request.url).pathname;
            console.log("请求 "+pathname+" 响应");

            route(handle, pathname, response);
            
        }
        http.createServer(onRequest).listen(8080);
        console.log("成功的提示：httpd start @8080");
    }

    exports.start = start;


不再从 route() 函数获取返回值，而将 response 对象作为第三个参数传递给 route() 函数，并且，移除 onRequest() 中有关 response 的函数调用，这部分让 route() 函数完成。


2、 **router.js**

    function route(handle, pathname, response) {
        console.log("route 请求路径：" + pathname);
        if (typeof handle[pathname] === 'function') {
            handle[pathname](response);
        } else {
            console.log("找不到路径 " + pathname);
            response.writeHead(404, {"Content-Type": "text/plain"});
            response.write("404 Not found");
            response.end();
        }
    }

    exports.route = route;


相对此前从请求处理程序中获取返回值，这次取而代之的是直接传递 response 对象。


3、 **requestHandler.js**


    var exec = require("child_process").exec;

    function start(response) {
        console.log("处理请求 'start' 开启.");
        
        exec("ls -lah", function (error, stdout, stderr) {
            response.writeHead(200, {"Content-Type": "text/plain"});
            response.write(stdout);
            response.end();
        });
    }

    function upload(response) {
        console.log("处理请求 'upload' 开启.");
        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("Hello Upload");
        response.end();
    }

    exports.start = start;
    exports.upload = upload;


start 处理程序在 exec() 的匿名回调函数中做请求响应的操作
upload 处理程序这次是使用 response 对象。

4、运行结果： 运行很好

> 如何 **证明** /start 处理程序中耗时的操作不会阻塞对 /upload 请求作出立即响应？
> 修改 **requestHandlers.js**

    var exec = require("child_process").exec;

    function start(response) {
        console.log("处理请求 'start' 开启.");
        
        exec("find /",
            { timeout: 10000, maxBuffer: 2000*1024},
            function (error, stdout, stderr) {
                response.writeHead(200, {"Content-Type": "text/plain"});
                response.write(stdout);
                response.end();
            }
        );
    }

    function upload(response) {
        console.log("处理请求 'upload' 开启.");
        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("Hello Upload");
        response.end();
    }

    exports.start = start;
    exports.upload = upload;

结果： /upload 时会立即响应，不管 /start 是否还在处理中

#### 场景实战：一个图片上传并在浏览器中显示的功能

简单的例子： 用 post 请求提交给服务器 文本区 中的内容； **requestHandlers.js**

    function start(response) {
        console.log("处理请求 'start' 开启.");
        
        var body = '<html>'+
            '<head>'+
            '<meta http-equiv="Content-Type" content="text/html; '+
            'charset=UTF-8" />'+
            '</head>'+
            '<body>'+
            '<form action="/upload" method="post">'+
            '<textarea name="text" rows="20" cols="60"></textarea>'+
            '<input type="submit" value=" 提 交 " />'+
            '</form>'+
            '</body>'+
            '</html>';

        response.writeHead(200, {"Content-Type": "text/html"});
        response.write(body);
        response.end();

    }

    function upload(response) {
        console.log("处理请求 'upload' 开启.");
        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("Hello Upload");
        response.end();
    }

    exports.start = start;
    exports.upload = upload;


因为 POST 请求会比较大（用户可能会填大量内容），为了使整个过程非阻塞， NodeJS 会将数据拆分成很多小数据块，然后通过触发特定的事件，将这些小数据块传递给回调函数。这里的特定的事件有 **data** 事件（表示新的小数据块到达了）以及 **end**事件（表示所有的数据都已经接收完毕）。

通过在 request 对象上注册监听器（listener）来实现这些事件的触发，以及回调；request对象是每次接收到HTTP请求时候，都会把该对象传递给 onRequest 回调函数。

    request.addListener("data", function(chunk) {
      // called when a new chunk of data was received
    });

    request.addListener("end", function() {
      // called when all chunks of data have been received
    });

这部分的逻辑应该写在哪里？
因为获取所有来自请求的数据，然后将这些数据给应用层处理，应该是HTTP服务器要做的事情，所以可以在服务器中处理 POST 数据，然后将最终的数据传递给请求路由和请求处理器，让他们来进行进一步的处理。

1、 **server.js**


    var http = require("http"),
        url  = require("url");

    function start(route, handle) {
        function onRequest(request, response) {
            var postData = '',
                pathname = url.parse(request.url).pathname;
            console.log("请求 "+pathname+" 响应");

            request.setEncoding('UTF-8');

            request.addListener('data', function(postDataChunk){
                postData += postDataChunk;
                console.log("收到数据块 ‘" + postDataChunk + "’.")
            })

            request.addListener('end', function(){
                route(handle, pathname, response, postData);
            });

        }
        
        http.createServer(onRequest).listen(8080);
        console.log("成功的提示：httpd start @8080");
    }

    exports.start = start;


以上代码做了三件事：

* 设置接收数据的编码 utf-8
* 注册 data 事件，收集每次接收到的新数据块并赋值给 postData
* 当所有数据接收完成在 end 事件中调用路由，传递给请求程序

2、接下来需要在 /upload 页面展示用户输入的内容，将 postData 传递给请求处理程序 **router.js**

    function route(handle, pathname, response, postData) {
        console.log("route 请求路径：" + pathname);
        if (typeof handle[pathname] === 'function') {
            handle[pathname](response, postData);
        } else {
            console.log("找不到路径 " + pathname);
            response.writeHead(404, {"Content-Type": "text/plain"});
            response.write("404 Not found");
            response.end();
        }
    }

    exports.route = route;


3、 **requestHandlers.js**

    function start(response, postData) {
        console.log("处理请求 'start' 开启.");
        
        var body = '<html>'+
            '<head>'+
            '<meta http-equiv="Content-Type" content="text/html; '+
            'charset=UTF-8" />'+
            '</head>'+
            '<body>'+
            '<form action="/upload" method="post">'+
            '<textarea name="text" rows="20" cols="60"></textarea>'+
            '<input type="submit" value=" 提 交 " />'+
            '</form>'+
            '</body>'+
            '</html>';

        response.writeHead(200, {"Content-Type": "text/html"});
        response.write(body);
        response.end();

    }

    function upload(response, postData) {
        console.log("处理请求 'upload' 开启.");
        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("你写的:" + decodeURIComponent(postData));
        response.end();
    }

    exports.start = start;
    exports.upload = upload;


4、运行结果： 

在 /start 中输入“ 你好 ” ，在 /upload 输出 <code>你写的:text=你好</code>

> 缺点： 其实一般情况下需要的只是 text 字段；那么，方法是调用 *querystring* 模块
> 修改 requestHandlers.js 

    //原生自带,包括4个方法,看 tips
    var querystring = require('querystring');

    function start(response, postData) {
        console.log("处理请求 'start' 开启.");
        
        var body = '<html>'+
            '<head>'+
            '<meta http-equiv="Content-Type" content="text/html; '+
            'charset=UTF-8" />'+
            '</head>'+
            '<body>'+
            '<form action="/upload" method="post">'+
            '<textarea name="text" rows="20" cols="60"></textarea>'+
            '<input type="submit" value=" 提 交 " />'+
            '</form>'+
            '</body>'+
            '</html>';

        response.writeHead(200, {"Content-Type": "text/html"});
        response.write(body);
        response.end();

    }

    function upload(response, postData) {
        console.log("处理请求 'upload' 开启.");
        response.writeHead(200, {"Content-Type": "text/plain"});
        response.write("你写的：<br>" + querystring.parse(decodeURIComponent(postData).text));
        response.end();
    }

    exports.start = start;
    exports.upload = upload;

> tips 

> querystring 类包含4个方法

> 1、querystring.stringify(obj, [sep], [eq])  将对象转换成字符串
> 2、querystring.parse(str, [sep], [eq], [options]) 将字符串转换成对象
> 3、querystring.escape 参数编码
> 4、querystring.unescape 参数解码


#### 处理文件上传

效果：允许用户上传图片，并将该图片在浏览器中显示出来。

需要用到外部模块： *Felix Geisendörfer* 开发的 **node-formidable** 模块

用 NPM 包管理器安装

    npm install formidable

完成后

1、 **requestHandlers.js** 添加图片展示模块，修改上传模块


    var querystring = require('querystring'),
        //可将文件读取到服务器中
        fs = require('fs');
        //解析上传文件数据
        formidable = require("formidable");

    function start(response) {
        console.log("处理请求 'start' 开启.");
        
        var body = '<html>'+
            '<head>'+
            '<meta http-equiv="Content-Type" content="text/html; '+
            'charset=UTF-8" />'+
            '</head>'+
            '<body>'+
            '<form action="/upload" enctype="multipart/form-data" '+
            'method="post">'+
            '<input type="file" name="upload">'+
            '<input type="submit" value=" 上 传 " />'+
            '</form>'+
            '</body>'+
            '</html>';

        response.writeHead(200, {"Content-Type": "text/html"});
        response.write(body);
        response.end();

    }

    function upload(response, request) {
        console.log("处理请求 'upload' 开启.");

        var form = new formidable.IncomingForm();
        console.log("加载解析");
        form.parse(request, function(error, fields, files) {
            console.log("解析完成");

            var is = fs.createReadStream(files.upload.path);
            var os = fs.createWriteStream("./tmp/test.png");
            is.pipe(os);
            is.on('end',function(){
                fs.unlinkSync(files.upload.path);
            });
            
            //fs.renameSync(files.upload.path, "./tmp/test.png");
            response.writeHead(200, {"Content-Type": "text/html"});
            response.write("收到图片:<br/>");
            response.write("<img src='/show' />");
            response.end();
        });

    }


    function show(response) {
        console.log("处理请求 'show' 开启.");
        fs.readFile("./tmp/test.png", "binary", function(error, file) {
            if(error) {
                response.writeHead(500, {"Content-Type": "text/plain"});
                response.write(error + "\n");
                response.end();
            } else {
                response.writeHead(200, {"Content-Type": "image/png"});
                response.write(file, "binary");
                response.end();
            }
        });
    }

    exports.start = start;
    exports.upload = upload;
    exports.show = show;


2、 **index.js** 添加新的请求处理到 路由映射表

    var server = require("./server"),
        router = require("./router"),
        requestHandlers = require("./requestHandlers");

    var handle = {};
        handle["/"] = requestHandlers.start;
        handle["/start"] = requestHandlers.start;
        handle["/upload"] = requestHandlers.upload;
        handle["/show"] = requestHandlers.show; 

    server.start(router.route, handle);

3、 **server.js** 移除对postData的处理以及request.setEncoding （这部分node-formidable自身会处理），转而采用将request对象传递给请求路由的方式：

    var http = require("http"),
        url  = require("url");

    function start(route, handle) {
        function onRequest(request, response) {
            var pathname = url.parse(request.url).pathname;
            console.log("请求 "+pathname+" 响应");
            route(handle, pathname, response, request);
        }

        http.createServer(onRequest).listen(8080);
        console.log("成功的提示：httpd start @8080");
    }

    exports.start = start;

4、 **router.js** 不需要传递postData了，而要传递request对象

    function route(handle, pathname, response, request) {
        console.log("route 请求路径：" + pathname);
        if (typeof handle[pathname] === 'function') {
            handle[pathname](response, request);
        } else {
            console.log("找不到路径 " + pathname);
            response.writeHead(404, {"Content-Type": "text/plain"});
            response.write("404 Not found");
            response.end();
        }
    }

    exports.route = route;


> 小坑： 

    //fs.renameSync(files.upload.path, "./tmp/test.png")
    //报错如下
    fs.js:543
      return binding.rename(pathModule._makeLong(oldPath),
                     ^
    Error: EXDEV, cross-device link not permitted 'C:\User……
    ……


> 修改成如下代码即可：

    var is = fs.createReadStream(files.upload.path);
    var os = fs.createWriteStream("./tmp/test.png");
    is.pipe(os);
    is.on('end',function(){
        fs.unlinkSync(files.upload.path);
    });

    //fs.renameSync(files.upload.path, "./tmp/test.png");


5、效果演示：(楼主以 .gif为例)


![最终效果][8]




> 注：本文笔记来自于 [Manuel Kiessling][0]写的 [Node入门 一本全面的Node.js教程][5]
> 其他： [Node.js community wiki][6]——[NodeJS社区][7]

完
----



[0]: https://github.com/manuelkiessling/nodebeginner.org "Manuel Kiessling"
[1]: http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb "理解NodeJS"
[2]: http://martinfowler.com/articles/injection.html "关于依赖注入的大作"
[3]: http://steve-yegge.blogspot.com/2006/03/execution-in-kingdom-of-nouns.html "名词王国中的死刑"
[4]: http://www.cnblogs.com/kym/archive/2011/03/07/1976519.html "函数式编程扫盲篇"
[5]: https://leanpub.com/nodebeginner-chinese "Node入门 一本全面的Node.js教程"
[6]: https://github.com/joyent/node/wiki    "Node.js community wiki"
[7]: http://www.nodecloud.org/              "NodeJS社区"
[8]: http://images.cnitblog.com/blog/531703/201502/111631185424695.gif "最终效果"

