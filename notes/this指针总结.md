### this 指针问题

**【注意】 this 的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁。**

【例子1】
如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window
```javascript
function a () {
  var name = 'aaaa';
  console.log(this.name)
  console.log(this)
}
a()  //  undefind   window
```

【例子2】如果一个函数中有this，这个函数被上一级的对象所调用，那么this指向的就是上一级的对象。
```javascript
var obj = {
  name: 'dfdfd',
  fn: function () {
    console.log(this.name)
  }
}
obj.fn()  //  dfdfd
```

【例子3】 如果一个函数中有this，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象。

```javascript
var obj = {
  name: 'long',
  b: {
    name: 'zhou',
    fn: function () {
      console.log(this.name)
    } 
  }
}
obj.b.fn()  //  zhou
```
【例子4】 特殊情况：要注意只有函数执行的时候才能确定this到底指向谁, 比如下面的例子，将函数赋值给变量 a，在这个过程中并没有执行函数，最后 a 在window 作用域内执行，所以指向 window。
```javascript
var obj = {
  name: 'long',
  b: {
    name: 'zhou',
    fn: function () {
      console.log(this.name)
      console.log(this)
    } 
  }
}
var a = obj.b.fn // undefind   window
a()
```
