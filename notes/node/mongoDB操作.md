1. `show dbs`: 显示所有数据的列表。

2. `db`:  显示当前数据库对象或集合。

3. `use [库名]`: `use` 命令可以连接到一个指定的数据库。如果数据库不存在，则创建数据库，否则切换到指定数据库

4. `db.dropDatabase()`: 删除当前数据库，默认为 test，你可以使用 db 命令查看当前数据库名。

5. `db.[collection 集合名].drop()`: 集合删除

6. `db.createCollection(name, options)`: 创建集合
    - name: 要创建的集合名称
    - options: 可选参数, 指定有关内存大小及索引的选项

7. `show collections || show tables`: 查看已有集合

8. `db.[COLLECTION_NAME 集合名].insert(document)`: 向集合中插入文档

9. `db.[COLLECTION_NAME 集合名].find()`: 查看已插入文档

10. `db.collection.update( <query>,<update>,{upsert: <boolean>,multi: <boolean>,writeConcern: <document>})`: 更新文档
    - query : update的查询条件，类似sql update查询内where后面的。
    - update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
    - upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
    - multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
    - writeConcern :可选，抛出异常的级别。

11. `db.collection.save(<document>, {writeConcern: <document>})`: 通过传入的文档来替换已有文档
    - document : 文档数据。
    - writeConcern :可选，抛出异常的级别。

12. `db.collection.remove(<query>,{justOne: <boolean>,writeConcern: <document> })`: 删除文档
    - query :（可选）删除的文档的条件。
    - justOne : （可选）如果设为 true 或 1，则只删除一个文档。
    - riteConcern :（可选）抛出异常的级别。

13. `db.collection.find(query, projection)`: 查询文档
    - query ：可选，使用查询操作符指定查询条件
    - projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

14. `db.collection.find({"title" : {$type : 'string'}})`: 查找文档中 title 都是 string 类型的数据

15. 

### MongoDB中常用的几种数据类型

|数据类型|描述|
|:-:|:-:|
|String|字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的|
|Integer|整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。|
|Boolean|布尔值。用于存储布尔值（真/假）。|
|Double|双精度浮点值。用于存储浮点值。|
|Min/Max keys|将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比.|
|Array|用于将数组或列表或多个值存储为一个键。|
|Timestamp|时间戳。记录文档修改或添加的具体时间。|
|Object|用于内嵌文档。|
|Null|用于创建空值。|
|Symbol|符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。|
|Date|日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。|
|Object ID|对象 ID。用于创建文档的 ID。|
|Binary Data|二进制数据。用于存储二进制数据。|
|Code|代码类型。用于在文档中存储 JavaScript 代码。|
|Regular expression	|正则表达式类型。用于存储正则表达式。|