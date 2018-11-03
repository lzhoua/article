[express教程](https://www.runoob.com/w3cnote/express-4-x-api.html#toc_15)

### 创建一个最简单的express
  ```javascript
    const express = require('express')
    const app = express()
    app.get('/', (req, res) => {
      res.send('hello word')
    })
    app.listen(3000, () => {
      console.log('已经在3000端口跑起来了')
    })
  ```
  express()用来创建一个Express的程序。express()方法是express模块导出的顶层方法。

### request 和 response 对象的具体介绍
- `Request` 对象 - `request` 对象表示 `HTTP` 请求，包含了请求查询字符串，参数，内容，`HTTP` 头部等属性。常见属性有：
  1. `req.app`：当 `callback` 为外部文件时，用 `req.app` 访问 `express` 的实例
  2. `req.baseUrl`：获取路由当前安装的URL路径
  3. `req.body / req.cookies`：获得「请求主体」/ `Cookies`
  4. `req.fresh / req.stale`：判断请求是否还「新鲜」
  5. `req.hostname / req.ip`：获取主机名和IP地址
  6. `req.originalUrl`：获取原始请求URL
  7. `req.params`：获取路由的 `parameters`
  8. `req.path`：获取请求路径
  9. `req.protocol`：获取协议类型
  10. `req.query`：获取URL的查询参数串
  11. `req.route`：获取当前匹配的路由
  12. `req.subdomains`：获取子域名
  13. `req.accepts()`：检查可接受的请求的文档类型
  14. `req.acceptsCharsets / req.acceptsEncodings / req.acceptsLanguages`：返回指定字符集的第一个可接受字符编码
  15. `req.get()`：获取指定的HTTP请求头
  16. `req.is()`：判断请求头 `Content-Type` 的MIME类型

- `Response` 对象 - `response` 对象表示 `HTTP` 响应，即在接收到请求时向客户端发送的 `HTTP` 响应数据。常见属性有：

  1. `res.app`：同 `req.app` 一样
  2. `res.append()`：追加指定HTTP头
  3. `res.set()`：在res.append()后将重置之前设置的头
  4. `res.cookie(name，value [，option])`：设置Cookie
  5. `opition`: domain / expires / httpOnly / maxAge / path / secure / signed
  6. `res.clearCookie()`：清除Cookie
  7. `res.download()`：传送指定路径的文件
  8. `res.get()`：返回指定的HTTP头
  9. `res.json()`：传送JSON响应
  10. `res.jsonp()`：传送JSONP响应
  11. `res.location()`：只设置响应的 `Location HTTP` 头，不设置状态码或者 `close response`
  12. `res.redirect()`：设置响应的 `Location HTTP` 头，并且设置状态码302
  13. `res.render(view,[locals],callback)`：渲染一个view，同时向 `callback` 传递渲染后的字符串，如果在渲染过程中有错误发生next(err)将会被自动调用。`callback` 将会被传入一个可能发生的错误以及渲染后的页面，这样就不会自动输出了。
  14. `res.send()`：传送HTTP响应
  15. `res.sendFile(path [，options] [，fn])`：传送指定路径的文件 -会自动根据文件 `extension` 设定 `Content-Type`
  16. `res.set()`：设置 `HTTP` 头，传入 `object` 可以一次设置多个头
  17. `res.status()`：设置 `HTTP` 状态码
  18. `res.type()`：设置 `Content-Type` 的MIME类型

### express.static(root, [options])
`express.static` 是 `Express` 中唯一的内建中间件。它以 `server-static`模块为基础开发，负责托管 `Express` 应用内的静态资源。 参数 `root` 为静态资源的所在的根目录。 参数 `options` 是可选的，支持以下的属性： 
|属性|描述|类型|默认值|
|:-:|:-:|:-:|:-:|
|dotfiles|是否响应点文件。供选择的值有"allow"，"deny"和"ignore"|String|"ignore"|
|etag|使能或者关闭etag|Boolean|true|
|extensions|设置文件延期回退|Boolean|true|
|index|发送目录索引文件。设置false将不发送。|Mixed|"index.html"|
|lastModified|设置文件在系统中的最后修改时间到Last-Modified头部。可能的取值有false和true。	|Boolean|true|
|maxAge|在Cache-Control头部中设置max-age属性，精度为毫秒(ms)或则一段ms format的字符串|Number|0|
|redirect|当请求的pathname是一个目录的时候，重定向到尾随"/"|Boolean|true|
|setHeaders|当响应静态文件请求时设置headers的方法|Funtion||
如果你想获得更多关于使用中间件的细节，你可以查阅[Serving static files in Express。](http://expressjs.com/en/starter/static-files.html)


### Application()
`app` 对象一般用来表示 `Express` 程序，通过调用 `Express` 模块导出的顶层的 `express()` 方法来创建它:
  ```javascript
    const express = require('express')
    const app = express()
    app.get('/', (req, res) => {
      res.send('hello word')
    })
    app.listen(3000)
  ```
`app` 对象具有以下的方法：
- 路由HTTP请求；具体可以看 `app.METHOD` 和 `app.param` 这两个例子。
- 配置中间件；具体请看 `app.route`。
- 渲染HTML视图；具体请看 `app.render` 。
- 注册模板引擎；具体请看 `app.engine`。
- 它还有一些属性设置，这些属性可以改变程序的行为。[获得更多的信息，可以查阅](https://www.runoob.com/w3cnote/express-4-x-api.html#app.settings.table)。

### 子程序
可以创建一个子程序，可以挂载在其他路径下
```javascript
const express = require('express')
const app = express()
const admin = express()

admin.get('/', function (req, res) {
  res.sendFile(__dirname + '/index.html')
})
app.use('/index', admin)

app.listen(3000)
```

### app.Method 
1. `app.all(path, callback[, callback ...]`

2. `app.delete(path, callback[, callback ...])`

3. `app.disabled(name)`

4. `app.enable(name)`

5. `app.engine(ext, callback) 设置模板引擎`

6. `app.get(path, callback [, callback ...])`

7. `app.listen(port, [hostname], [backlog], [callback])` 

8. `app.param([name], callback)`

通过调用express()返回得到的app实际上是一个JavaScript的Function，被设计用来作为一个回调传递给Node HTTP servers来处理请求。这样，其就可以很简便的基于同一份代码提供http和https版本，所以app没有从这些继承(它只是一个简单的回调)。
```javascript
  var express = require('express');
  var https = require('https');
  var http = require('http');
  http.createServer(app).listen(80);
  https.createServer(options, app).listen(443);
```