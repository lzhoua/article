1.  使用方法：
    ```javascript
    class Point {  // 定义一个类
      constructor (x) { // 类的构造方法
        this.x = x
      }

      toString () { // 类的方法
        return `${this.x}`
      }
    }

    let p = new Point(1) // 和构造函数一样使用
    ```

2.  等价
    ```javascript
    class Point { 
      constructor (x) {this.x = x}
      toString () {}
    }

    【等价于】=>

    function Point1 (x) {
      this.x = x
    }
    Point1.propotype.toString = function () {}


    Point.prototype.constructor === Point1
    ```

3. 类内部所有定义的方法都是不可以枚举的，与es5中在原型上定义的方法可枚举不同

4. 类的属性名可以采用表达式
    ```javascript
    let methodName = 'getArea';

    class Square {
      constructor(length) {
        // ...
      }

      [methodName]() {
        // ...
      }
    }
    ```
5. 类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式

6. `constructor` 方法是类的默认方法，通过 `new` 命令生成对象实例时，自动调用该方法。一个类必须有 `constructor` 方法，如果没有显式定义，一个空的 `constructor` 方法会被默认添加。
    ```javascript
      class Point {
      }
      // 等同于
      class Point {
        constructor() {}
      }
    ```
7. 类必须用 `new` 调用

8. 可以使用表达式的方式使用
    ```javascript
      let myclass = class {}
      let myclass = new class {} // 立即执行
    ```
9. 不存在变量提升