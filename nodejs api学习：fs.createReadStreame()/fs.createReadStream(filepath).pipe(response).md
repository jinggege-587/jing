#fs.createReadStream(filepath).pipe(response);这句是什么意思？

```
'use strict';

var
    fs = require('fs'),
    url = require('url'),
    path = require('path'),
    http = require('http');

// 从命令行参数获取root目录，默认是当前目录:
var root = path.resolve(process.argv[2] || '.');

console.log('Static root dir: ' + root);

// 创建服务器:
var server = http.createServer(function (request, response) {
    // 获得URL的path，类似 '/css/bootstrap.css':
    var pathname = url.parse(request.url).pathname;
    // 获得对应的本地文件路径，类似 '/srv/www/css/bootstrap.css':
    var filepath = path.join(root, pathname);
    // 获取文件状态:
    fs.stat(filepath, function (err, stats) {
        if (!err && stats.isFile()) {
            // 没有出错并且文件存在:
            console.log('200 ' + request.url);
            // 发送200响应:
            response.writeHead(200);
            // 将文件流导向response:
            fs.createReadStream(filepath).pipe(response);
            // 这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里↑↑这里
        } else {
            // 出错了或者文件不存在:
            console.log('404 ' + request.url);
            // 发送404响应:
            response.writeHead(404);
            response.end('404 Not Found');
        }
    });
});

server.listen(8080);

console.log('Server is running at http://127.0.0.1:8080/');

```


fs.createReadStream(filepath).pipe(response);

假设传入一个txt文件，.createReadStream是返回一个读取流，response是响应头，pipe是将两个数据流连接起来。
为什么这样可以输出数据流？
和WriteStream有区别吗？
答案：
http.createServer回调中的response参数，你可以去看看文档，在这里： 
[Class: http.ServerResponse ](https://nodejs.org/api/http.html#http_class_http_serverresponse)
看这句话：

>The response implements, but does not inherit from, the Writable Stream interface.
response实现了WritableStream的接口，但并未继承它。

所以response对象，对外可以当作WritableStream来使用，但是它并不是WritableStream的实例。

