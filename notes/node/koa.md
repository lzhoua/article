### 下载koa
```
  npm install koa --save
```

### koa 简单使用
  ```javascript
  const Koa = require('koa')
  const app = new Koa()
  // 配置中间件
  app.use(async (ctx) => {
    ctx.body = 'hello world'
  })
  // 监听端口
  app.listen(3000)
  ```

### koa路由 `koa-router`
`koa` 路由需要手动引入，`express` 是自带
- 下载
  ```
  npm install koa-router --save
  ```
- 使用
  ```javascript
  const Koa = require('koa')
  const router = require('koa-router')() // 需要实例化

  const app = new Koa()

  router.get('/', (ctx, next) => {
    ctx.body = 'hello, 这是首页'
  })

  app
    .use(router.routes())      // 启动路由，挂载路由中间件
    .use(router.allowedMethods())

  app.listen(3000, () => {
    console.log('start server at port 3000')
  })
  ```

- `get`传值接收
  1. ctx.query：以对象的形式
  2. ctx.querystring: 以字符串形式
  3. 也可以使用 ct.request.query

- 动态路由
  ```javascript
    router.get('/home/:id', async (ctx) => {
      let id = ctx.params.id
      // ...
    })
  ```

 ### 中间件
  通俗的讲： 中间件就是匹配路由之前或者匹配路由完成后做的一系列操作
  
 - 分类： 
  1. 应用级中间件(全局中间件)
  2. 路由级中间件（只存在某个路由）
  3. 错误处理中间件
  4. 第三方中间件

- 应用级中间件
  ```javascript
  // ......
  app.use(async (ctx, next) => { // 在任一个路由匹配之前都会执行
    console.log('应用级中间件')
    await next() // 如果不用next将会在这里停止
  })

  router.get('/home', async (ctx, next) => {
    ctx.body = '这是home页面'
  })
  ```

- 路由级中间件
  ```javascript
  // ......
  router.get('/home', async (ctx, next) => { // 先执行此路由匹配
    console.log('会先执行此匹配')
    await next() // 如果不用next将不会接着匹配下个路由
  })

  router.get('/home', async (ctx, next) => {
    ctx.body = '这是home页面'
  })
  ```
- 错误处理中间件
  ```javascript
  // ......
  app.use(async (ctx, next) => { // 在任一个路由匹配之前都会执行
    console.log('先执行此步骤')
    await next() // 跳到相应路由匹配

    // 执行完路由匹配后再回来执行下面代码
    if (ctx.status === 404) {
      ctx.body = '这是一个404页面'
    } else {
      console.log('没有错误')
    }
  })
  ```

- 第三方中间件
  ```JAVASCRIPT
   // 参考上面路由中间件的使用
  ```

- 洋葱模型
  ```javascript
  app.use(async (ctx, next) => {
    console.log('这是第 1 步执行')
    await next()

    console.log('这是第 6 步执行')
  })

  app.use(async (ctx, next) => {
    console.log('这是第 2 步执行')
    await next()

    console.log('这是第 5 步执行')
  })

  router.get('/home', async (ctx, next) => {
    console.log('这是第 3 步执行')
    await next()
  })

  router.get('/home', async (ctx) => {
    console.log('这是第 4 步执行')
  })
  ```

### post 接收数据
- node 原生写法
  ```javascript
    // 封装数据接收

    function parsePost () {
      return new Promise((resolve, reject) => {
        try {
          let postData = ''
          ctx.req.on('data', (data) => {
            postData = postData + data
          })
          ctx.req.on('end', () => {
            resolve(postData)
          })
        } catch(err) {
          reject(err)
        }
      })
    }

    router.post('/login', async (ctx) => {
      let data = await parsePost(ctx)
      console.log(data)
    })
  ```

- 使用 `koa-bodyparser` 中间件
 1. 安装
    ```
    npm install koa-bodyparser --save
    ```

 2. 用法
    ```javascript
      const Koa = require('koa')
      const bodyParser = require('koa-bodyparser')
      const app = new Koa()
      app.use(bodyParser())

      app.post('/login', async (ctx) => {
        ctx.body = ctx.request.body // 使用ctx.request.body拿到数据
      })
    ```

  - cooking 的使用


  - session 的使用


  - 


