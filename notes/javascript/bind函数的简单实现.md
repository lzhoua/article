### bind函数的简单实现
>介绍： bind()方法创建一个新函数，在调用时，将其this关键字设置为提供的值，并在调用新函数时提供任何前面提供的给定参数序列。
- **语法：**`function.bind(thisArg, [arg1, arg2, arg3])`
  
  **参数：** 
  1. `this.Arg`: this调用绑定函数时作为参数传递给目标函数的值。如果使用new运算符构造绑定函数，则忽略该值。
  2. `arg1, arg2, ……：`在调用目标函数时预先添加到绑定函数的参数的参数。


 **首先来个例子**
 ```javascript
  let a = 10
  let obj = {
    a: 20,
    getA: funciton () {
      return this.a
    }
  }
  obj.getA()  //  20
  let newA = obj.getA
  newA()    // 10
  /**
  *    如上所示，当我们调用函数所在的作用域改变时，this指针相应的更改
  *    我们有什么办法能一直读取到 obj 中的变量 a ,而不是读取全局中的变量 a 呢，使用bind()方法即可
  **/
  let newA2 = obj.getA.bind(obj)
  newA2()    // 20
  // 【注意】当 bind() 的第一个参数为 null 时，即不更改 this 指向
 ```

- `bind` 函数返回一个新函数，并且 `this` 指针指向第一个参数，同时，后面还可以添加绑定函数的参数的参数。

1. 第一步:实现返回一个新函数，并且 `this` 指向第一个参数

```javascript
// 因为 bind 是函数的方法 ， 所以在函数的原型上添加
Function.prototype.bind = function (fn) {
  let _this = this  
  return function () {  // 返回一个新函数
    return _this.apply(fn)
  }
}
```

2. 第二步：实现参数的添加

```javascript
// 因为 bind 是函数的方法 ， 所以在函数的原型上添加
Function.prototype.bind = function (fn) {
  let _this = this
  let args = Arrag.prototype.slice.call(arguments, 1) // 取得从第一个参数至最后一个参数组成的数组
  return function () {  // 返回一个新函数
    return _this.apply(fn, args)
  }
}
```

3. 第三步：给返回的新函数增加参数传递

```javascript
// 因为 bind 是函数的方法 ， 所以在函数的原型上添加
Function.prototype.bind = function (fn) {
  let _this = this
  let args = Arrag.prototype.slice.call(arguments, 1) // 取得从第一个参数至最后一个参数组成的数组
  return function () {  // 返回一个新函数
    let agg = Arrag.prototype.slice.call(arguments, 0)
    return _this.apply(fn, args.concat(agg))
  }
}
```

4. 官方文档上有一句话: 绑定过后的函数被new实例化之后，需要继承原函数的原型链方法，且绑定过程中提供的this被忽略（继承原函数的this对象），但是参数还是会使用。

```javascript
    😁有空再来实现
```
