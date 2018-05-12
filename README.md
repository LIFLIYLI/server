# server
制作一个服务器
1：找一个安全的地方建一个文件夹，进入这个文件夹，创建一个server.js文件。
2：编辑这个server.js文件，start server.js  代码如下
var http = require('http')
var fs = require('fs')
var url = require('url')
var port = process.argv[2]

if(!port){                                                                
  console.log('请指定端口号好不啦？\nnode server.js 8888 这样不会吗？')          //如果没有指定端口号将报错，报错内容见括号。
  process.exit(1)
}

var server = http.createServer(function(request, response){
  var parsedUrl = url.parse(request.url, true)
  var path = request.url 
  var query = ''
  if(path.indexOf('?') >= 0){ query = path.substring(path.indexOf('?')) }
  var pathNoQuery = parsedUrl.pathname
  var queryObject = parsedUrl.query
  var method = request.method

  /******** 从这里开始看，上面不要看 ************/
  console.log('HTTP 路径为\n' + path)                             //如果指定路径如下
  if(path == '/style.js'){                                 //如果指定路径为/style.js，则响应CSS，注意，后缀没有什么意义，可以变。
    response.setHeader('Content-Type', 'text/css; charset=utf-8')
    response.write('body{background-color: #ddd;}h1{color: red;}')
    response.end()
  }else if(path == '/script.html'){                            //如果指定路径为/script,则响应JS文件，不要看后缀，看response.setHeader行的                                                                                          script就行，因为response.setHeader表示响应的格式
    response.setHeader('Content-Type', 'text/javascript; charset=utf-8')
    response.write('alert("这是JS执行的")')
    response.end()
  }else if(path == '/'){                           //如果指定路径为/ ，则响应HTML文件
    response.setHeader('Content-Type', 'text/html; charset=utf-8')
    response.write('<!DOCTYPE>\n<html>'  +                       //表示响应的第四部分的内容
      '<head><link rel="stylesheet" href="/style.js">' +
      '</head><body>'  +
      '<h1>你好</h1>' +
      '<script src="/script.html"></script>' +
      '</body></html>')
    response.end()
  }else{                                       //否则
    response.statusCode = 404                 //响应404
    response.end()
  }

  /******** 代码结束，下面不要看 ************/
})

server.listen(port)
console.log('监听 ' + port + ' 成功\n请用在空中转体720度然后用电饭煲打开 http://localhost:' + port)

3：如上，代码编辑完成。命令行输入node server.js或node server。服务器就开始运行，要终端运行则按Ctrl+C,中断。

