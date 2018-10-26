# `Revealing Module` (揭示模块) 模式

> 揭示模块模式其实是 `Module` 模式的稍改进版，因为使用 `module`模式时，必须要切换到对象字面量表示法来让某种方法变成公有方法。所以揭示模块模式就是为了改进这种情况而出现的。

- 揭示模块模式能够在私有范围内简单定义 "所有的函数和变量"，并返回一个匿名对象，它拥有指向私有函数的指针，该函数是他希望展示为公有的方法。

  ```javascript
  var Car = function () {
    var type = '小汽车';  // 定义私有变量
    function year () {  // 定义私有函数
      return '2017'
    };

    var color = 'red';  // 定义共有变量
    function money (num) {  // 共有函数
      console.log('这是辆' + this.color + '的' + type + ', 需要' + num + '人民币')
    }

    // 将暴露的公有指针指向到私有函数和属性上
    return {
      setColor: color,
      getfun: money
    }
  } ();
  // 调用
  console.log(Car.setColor); // red
  Car.getfun(20000);    // 这是辆red的小汽车, 需要20000人民币
  Car.setColor = 'blue';  // 修改公有变量
  console.log(Car.setColor); // blue
  Car.getfun(15000);    // 这是辆blue的小汽车, 需要15000人民币
  ```

### 优点
      该模式可以使脚本语法更加一致。在模块代码底部，它也会很容易指出哪些函数和变量可以被公开访问，从而改善可读性。
### 缺点
    如果一个私有函数引用一个公有函数，在需要打补丁时，公有函数是不能被覆盖的。这是因为私有函数将继续引用私有实现，改模式并不适用于公有成员，只适用于函数。
