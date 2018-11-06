## Mongoose是什么？
Mongoose是MongoDB的一个对象模型工具，是基于node-mongodb-native开发的MongoDB nodejs驱动，可以在异步的环境下执行。同时它也是针对MongoDB操作的一个对象模型库，封装了MongoDB对文档的的一些增删改查等常用方法，让NodeJS操作Mongodb数据库变得更加灵活简单。

### 安装与引用
1. 安装
    ```
    npm install mongoose --save
    ```
2. 引用与连接数据库
    ```javascript
    const mongoose = require('mongoose')
    const url = 'mongodb://localhost:27017'  数据库地址
    const dbName = 'admin' // 数据库名

    let db = mongoose.connect(`${url}${dbName}`)

    db.connecton.on('error', function (error) {
      console.log("数据库连接失败：" + error)
    })

    db.connection.on("open", function () {
      console.log("------数据库连接成功！------");
    })
    ```

### 集合
**MongoDB:** 是一个对象数据库，没有表、行等概念，也没有固定的模式和结构，所有的数据以`Document` (文档)的形式存储(`Document`，就是一个关联数组式的对象，它的内部由属性组成，一个属性对应的值可能是一个数、字符串、日期、数组，甚至是一个嵌套的文档。)，后面我们会学习如何创建文档并插入内容。

在 `MongoDB` 中，多个 `Document`可以组成 `Collection`  (集合)，多个集合又可以组成数据库。

1. **文档:** 是 `MongoDB` 的核心概念，是键值对的一个有序集，在 `JavaScript` 里文档被表示成对象。同时它也是 `MongoDB` 中数据的基本单元，非常类似于关系型数据库管理系统中的行，但更具表现力。

2. **集合:** 由一组文档组成，如果将 `MongoDB` 中的一个文档比喻成关系型数据库中的一行，那么一个集合就相当于一张表。

### Schema
**Schema:** 一种以文件形式存储的数据库模型骨架，无法直接通往数据库端，也就是说它不具备对数据库的操作能力，仅仅只是数据库模型在程序片段中的一种表现，可以说是数据属性模型(传统意义的表结构)，又或着是“集合”的模型骨架。

- 定义一个Schema
```javascript
const mongoose = require('mongoose')

let userSchame = new mongoose.Schame({
  username: {type: String},    // 属性username,类型为String
  age: {type: Number, default: 18}, // 属性age,类型为Number,默认为18
  time : {type: Date, default: Date.now },
  email: {type: String, default: ''}
})
```

基本属性类型有：字符串、日期型、数值型、布尔型(Boolean)、null、数组、内嵌文档等。

### Model 
**Model:** 由 `Schema` 构造生成的模型，除了 `Schema` 定义的数据库骨架以外，还具有数据库操作的行为，类似于管理数据库属性、行为的类。

```javascript
const mongoose = require('mongoose')
let db = mongoose.connect(`mongodb://localhost:27017/admin`)

let userSchame = new mongoose.Schame({
  username: {type: String},    // 属性username,类型为String
  age: {type: Number, default: 18}, // 属性age,类型为Number,默认为18
  time : {type: Date, default: Date.now },
  email: {type: String, default: ''}
})

let userModel = db.model('user', userSchame) 

let data = {
  name : "longzhou",
  age  : 18,
  email: "longzhou@qq.com"
}

// 在数据库中建立集合并保存数据
userModel.create(data, (error, docs) => {
  if (error) {
    console.log('error')
  } else {
    console.log(docs)
  }
})

```
user: 【**注意，`mongoose` 会自动添加 `s`, 大写会变为小写，所以数据库中的集合名是 `users`**】数据库中的集合名称,当我们对其添加数据时如果 `users` 已经存在，则会保存到其目录下，如果未存在，则会创建 `users` 集合，然后在保存数据。

创建一个 `Model` 模型，我们需要指定：1.集合名称，2.集合的 `Schema` 结构对象，满足这两个条件，我们就会拥有一个操作数据库的金钥匙。


### Entity

**Entity:** 由 `Model` 创建的实体，使用 `save` 方法保存数据，`Model` 和 `Entity` 都有能影响数据库的操作，但 `Model` 比 `Entity` 更具操作性。
  ```javascript
  let userEntity = new userModel({
    name : "longzhou",
    age  : 18,
    email: "longzhou@qq.com"
  })
  console.log(userEntity.name) // longzhou

  // 在数据库中建立集合并保存数据
  userEntity.save((error, docs) => {
    if (error) {
      console.log('error')
    } else {
      console.log(docs)
    }
  })
  ```

### 查询
1. find查询： obj.find(查询条件,callback);
    ```javascript
    Model.find({}, function(error, docs) {
      //若没有向find传递参数，默认的是显示所有文档
    })
    ```
2. 查询 age 为 20 的所有文档
    ```javascript
      Model.find({age: 20}, function(error, docs) {
        // 查询age为28的所有文档
      })
    ```
3. 过滤查询：.属性过滤 find(Conditions,field,callback); field省略或为Null，则返回所有属性。
    ```javascript
      返回只包含一个键值name、age的所有记录
      Model.find({},{name:1, age:1, _id:0},function(err,docs){
        //docs 查询结果集
      }
    ```
4. findOne 查询
   ```javascript
    Model.findOne({ age: 27},function(err,docs){
      // 查询符合age等于27的第一条数据
    }
   ```
5. findById
    ```javascript
    Model.findById(_id ,function(err,docs){
      //根据id查询
    }
    ```
### 条件查询
|符号|意义|
|:-:|:-:|
|$lt|小于|
|`$lt`    | 小于   |
|`$lte`   | 小于等于 |
|`$gt`    | 大于 |
|`$gte`   | 大于等于 |
|`$ne`    | 不等于 |
|`$in`    | 可单值和多个值的匹配 |
|`$or`    | 查询多个键值的任意给定值 |
|`$exists`|  表示是否存在的意思 |
|`$all`   | 全部 |

1. $gt、$lt
    ```javascript
    Model.find({"age":{"$gt":18}},function(error,docs){
      //查询所有nage大于18的数据
    });
    ```
2. $ne
    ```javascript
      Model.find({name:{$ne:"tom"},age:{$gte:18}},function(error,docs){
        //查询name不等于tom、age>=18的所有数据
      });
    ```
3. $in
    ```javascript
    Model.find({ age:{ $in: 20}},function(error,docs){
      //查询age等于20的所有数据
    });
    
    Model.find({ age:{$in:[20,30]}},function(error,docs){
      //可以把多个值组织成一个数组
    });
    ```
4. $or
    ```javascript
    Model.find({"$or":[{"name":"longzhou"},{"age":28}]},function(error,docs){
      //查询 name 为 longzhou 或 age 为 28 的全部文档
    });
    ```
5. $exists
    ```javascript
    Model.find({name: {$exists: true}},function(error,docs){
      //查询所有存在name属性的文档
    });
    Model.find({telephone: {$exists: false}},function(error,docs){
      //查询所有不存在telephone属性的文档
    });
    ```
### 更新
1. 更新所有 age 为 20 的文档的name 为 ‘longzhou’
    ```javascript
      Model.updata({age: 20}, {$set: {name: 'longzhou'}}, (error) => {
        // .....
      })
    ```

### 删除数据
1. 删除所有 age 为 20 的文档
```javascript
  Model.remove({age: 20}, (error) => {
    // .....
  })
```

### 游标

1. limit函数的基本用法
    ```javascript
    Model.find({},null,{limit:20},function(err,docs){
        // 限制返回的文档数量为 20
        console.log(docs);
    });
    ```
   如果匹配的结果不到20个，则返回匹配数量的结果，也就是说limit函数指定的是上限而非下限。

2. skip函数的基本用法

    kip函数和limit类似，都是对返回结果数量进行操作，不同的是skip函数的功能是略过指定数量的匹配结果，返回余下的查询结果
    ```javascript
    Model.find({},null,{skip:4},function(err,docs){
       // 如果查询结果数量中少于4个的话，则不会返回任何结果。
        console.log(docs);
    });
    ```

3. sort函数的基本用法

    ort函数可以将查询结果数据进行排序操作，该函数的参数是一个或多个键/值对，键代表要排序的键名，值代表排序的方向，1是升序，-1是降序。
    ```javascript
    Model.find({},null,{sort:{age:-1}},function(err,docs){
      //查询所有数据，并按照age降序顺序返回数据docs
    });
    ```

