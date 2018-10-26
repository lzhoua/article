
> `javascript` 不支持类的概念，但它支持与对象一起用特殊的 `construtor`（构造器）函数。通过在构造器前面加  `new` 关键字，告诉 `javascript` 像使用构造器一样实例化一个新对象，并且对象成员由该函数定义。

## 基本构造器模式
```javascript
// 定义一个Car构造函数
function Car (color, year, money) {
    this.color = color;
    this.year = year;
    this.money = money;
  // 创建一个 toString 方法
    this.toString = function () {
      return 'this car is ' + this.color + ' create ' + this.year + ' need ' + this.money;
    }
  }
  // 创建 Car实例
  var one = new Car('red', 2009, 200000);
  var two = new Car('blue', 2016, 120000);
  console.log(one.toString());
  // this car is red create 2009 need 200000
  console.log(two.toString());
  // this car is blue create 2016 need 120000
```
这是一个简单的构造器模式版本，但是会有一些问题：
  - 继承变得困难
  - 每次新定义对象而使用 `Car` 构造器时 `toString()`都会分别重新定义

> 这是很不理想的，我们需要 `toString()` 这样的函数在所有的 `Car` 之间共享。所以下面升级一下这个设计模式。
### 带原型的 `Constructor`

> js中有一个名为 `prototype` 的属性。调用 js 构造器创建一个对象后，新的对象就会具有构造器原型的所有属性。通过这种方式，可以创建多个 Car 对象，并访问相同的原型。

- 将上面代码修改如下:
```javascript
  function Car (color, year, money) {
    this.model = model;
    this.year = year;
    this.money = money;
  }

  Car.prototype.toString = function () {
    return 'this car is ' + this.color + ' create ' + this.year + ' need ' + this.money;
  }
  var one = new Car('red', 2009, 200000);
  var two = new Car('blue', 2016, 120000);
  console.log(one.toString());
  // this car is red create 2009 need 200000
  console.log(two.toString());
  // this car is blue create 2016 need 120000
```
这样 `toString()` 就能在所有的Car对象之间共享。